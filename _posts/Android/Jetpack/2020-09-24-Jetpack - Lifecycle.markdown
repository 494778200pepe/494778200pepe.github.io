---
layout: post
title:  "Jetpack - Lifecycle"
date:   2020-09-24 11:58:00 +0800
categories: Android
tags: Jetpack
author: pepe
description: 『 Lifecycle 』
---

官方文档：

[使用生命周期感知型组件处理生命周期 - Lifecycle - Android Developers](
https://developer.android.google.cn/topic/libraries/architecture/lifecycle)

源码分析：

`Activity`里面获取`Lifecycle`

```
	@NonNull
    @Override
    public Lifecycle getLifecycle() {
        return mLifecycleRegistry;
    }
```

`mLifecycleRegistry`是`ComponentActivity`的成员变量，`final`修饰。
	
```
	private final LifecycleRegistry mLifecycleRegistry = new LifecycleRegistry(this);
	// this 是 ComponentActivity，implements LifecycleOwner
```

`LifecycleRegistry`是`Lifecycle`的实现类

```
	public class LifecycleRegistry extends Lifecycle {
		...
	}
```

同时，在`LifecycleRegistry`以弱引用的方式持有`ComponentActivity`

```
	public class ComponentActivity extends androidx.core.app.ComponentActivity implements
        LifecycleOwner,
        ViewModelStoreOwner,
        SavedStateRegistryOwner,
        OnBackPressedDispatcherOwner {
		...
	}
```

此处`ComponentActivity`为`androidx.activity.ComponentActivity`

```
	public class ComponentActivity extends androidx.core.app.ComponentActivity
```

看完了，`Lifecycle` 和 `ComponentActivity` 的关系，我们再来看 `addObserver()` 干了什么


先看看 `LifecycleObserver`：
```
public class MyObserver implements LifecycleObserver {

    @OnLifecycleEvent(Lifecycle.Event.ON_RESUME)
    public void connectListener() {
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_PAUSE)
    public void disconnectListener() {
    }
}

// LifecycleObserver 一个没有抽象方法的接口
public interface LifecycleObserver {

}

```
 
`LifecycleRegistry` 中的 `addObserver()`

```
    @Override
    public void addObserver(@NonNull LifecycleObserver observer) {
		// 获取初始状态
        State initialState = mState == DESTROYED ? DESTROYED : INITIALIZED;
        ObserverWithState statefulObserver = new ObserverWithState(observer, initialState);
		// 将全状态的 statefulObserver ，存入 map 中，并返回
        ObserverWithState previous = mObserverMap.putIfAbsent(observer, statefulObserver);

		// 如果 map 中已经存在该 Observer，那么不执行后续操作
        if (previous != null) {
            return;
        }
		
		// 从弱引用中，获取 ComponentActivity，如果为null，那么已经 destroyed，返回
        LifecycleOwner lifecycleOwner = mLifecycleOwner.get();
        if (lifecycleOwner == null) {
            // it is null we should be destroyed. Fallback quickly
            return;
        }

		// 一个确保同步的判断，
		// mAddingObserverCounter != 0,是否正在添加 Observer
		// mHandlingEvent,是否正在处理 Event
        boolean isReentrance = mAddingObserverCounter != 0 || mHandlingEvent;
        State targetState = calculateTargetState(observer);
        mAddingObserverCounter++;
		
		// 也是确保同步的操作，应该不会执行
        while ((statefulObserver.mState.compareTo(targetState) < 0
                && mObserverMap.contains(observer))) {
            pushParentState(statefulObserver.mState);
            statefulObserver.dispatchEvent(lifecycleOwner, upEvent(statefulObserver.mState));
            popParentState();
            // mState / subling may have been changed recalculate
            targetState = calculateTargetState(observer);
        }

		// 执行同步操作
        if (!isReentrance) {
            // we do sync only on the top level.
            sync();
        }
        mAddingObserverCounter--;
    }
```

来看看`ObserverWithState`

```
	// 一个 Observer 的包装类，包含了当前的 state
	static class ObserverWithState {
        State mState;
		// 实际上的 观察这
        LifecycleEventObserver mLifecycleObserver;

        ObserverWithState(LifecycleObserver observer, State initialState) {
            mLifecycleObserver = Lifecycling.lifecycleEventObserver(observer);
            mState = initialState;
        }

        void dispatchEvent(LifecycleOwner owner, Event event) {
            State newState = getStateAfter(event);
            mState = min(mState, newState);
			// 分发事件的时候，实际上是 mLifecycleObserver 在执行 
            mLifecycleObserver.onStateChanged(owner, event);
            mState = newState;
        }
    }
```

看看`LifecycleEventObserver`的构造：
 
```
	@NonNull
    static LifecycleEventObserver lifecycleEventObserver(Object object) {
        boolean isLifecycleEventObserver = object instanceof LifecycleEventObserver;
        boolean isFullLifecycleObserver = object instanceof FullLifecycleObserver;
        if (isLifecycleEventObserver && isFullLifecycleObserver) {
            return new FullLifecycleObserverAdapter((FullLifecycleObserver) object,
                    (LifecycleEventObserver) object);
        }
        if (isFullLifecycleObserver) {
            return new FullLifecycleObserverAdapter((FullLifecycleObserver) object, null);
        }

        if (isLifecycleEventObserver) {
            return (LifecycleEventObserver) object;
        }

        final Class<?> klass = object.getClass();
		// 通过反射，解析类中的方法，确定是回调，还是注解反射
        int type = getObserverConstructorType(klass);
        if (type == GENERATED_CALLBACK) {
            List<Constructor<? extends GeneratedAdapter>> constructors =
                    sClassToAdapters.get(klass);
            if (constructors.size() == 1) {
                GeneratedAdapter generatedAdapter = createGeneratedAdapter(
                        constructors.get(0), object);
                return new SingleGeneratedAdapterObserver(generatedAdapter);
            }
            GeneratedAdapter[] adapters = new GeneratedAdapter[constructors.size()];
            for (int i = 0; i < constructors.size(); i++) {
                adapters[i] = createGeneratedAdapter(constructors.get(i), object);
            }
            return new CompositeGeneratedAdaptersObserver(adapters);
        }
        return new ReflectiveGenericLifecycleObserver(object);
    }
	
	// FullLifecycleObserverAdapter
	// LifecycleEventObserver
	// SingleGeneratedAdapterObserver
	// CompositeGeneratedAdaptersObserver
	// ReflectiveGenericLifecycleObserver //通过注解反射
	// 通过以上实现类执行onStateChanged()
```

接下来看，同步操作`sync()`

```
    // happens only on the top of stack (never in reentrance),
    // so it doesn't have to take in account parents
    private void sync() {
        LifecycleOwner lifecycleOwner = mLifecycleOwner.get();
        if (lifecycleOwner == null) {
            throw new IllegalStateException("LifecycleOwner of this LifecycleRegistry is already"
                    + "garbage collected. It is too late to change lifecycle state.");
        }
        while (!isSynced()) {
            mNewEventOccurred = false;
			// 根据当前的状态和 mObserverMap 里面存的所有 ObserverWithState，对比，确定向前向后
            // no need to check eldest for nullability, because isSynced does it for us.
            if (mState.compareTo(mObserverMap.eldest().getValue().mState) < 0) {
                backwardPass(lifecycleOwner);
            }
            Entry<LifecycleObserver, ObserverWithState> newest = mObserverMap.newest();
            if (!mNewEventOccurred && newest != null
                    && mState.compareTo(newest.getValue().mState) > 0) {
                forwardPass(lifecycleOwner);
            }
        }
        mNewEventOccurred = false;
    }
```

```
	// 向前
    private void forwardPass(LifecycleOwner lifecycleOwner) {
        Iterator<Entry<LifecycleObserver, ObserverWithState>> ascendingIterator =
                mObserverMap.iteratorWithAdditions();
		// 循环遍历 map ，分发事件
        while (ascendingIterator.hasNext() && !mNewEventOccurred) {
            Entry<LifecycleObserver, ObserverWithState> entry = ascendingIterator.next();
            ObserverWithState observer = entry.getValue();
            while ((observer.mState.compareTo(mState) < 0 && !mNewEventOccurred
                    && mObserverMap.contains(entry.getKey()))) {
                pushParentState(observer.mState);
                observer.dispatchEvent(lifecycleOwner, upEvent(observer.mState));
                popParentState();
            }
        }
    }

	// 向后
    private void backwardPass(LifecycleOwner lifecycleOwner) {
        Iterator<Entry<LifecycleObserver, ObserverWithState>> descendingIterator =
                mObserverMap.descendingIterator();
		// 循环遍历 map ，分发事件
        while (descendingIterator.hasNext() && !mNewEventOccurred) {
            Entry<LifecycleObserver, ObserverWithState> entry = descendingIterator.next();
            ObserverWithState observer = entry.getValue();
            while ((observer.mState.compareTo(mState) > 0 && !mNewEventOccurred
                    && mObserverMap.contains(entry.getKey()))) {
                Event event = downEvent(observer.mState);
                pushParentState(getStateAfter(event));
                observer.dispatchEvent(lifecycleOwner, event);
                popParentState();
            }
        }
    }
```
看到这里，我们基本上知道，状态的更新，是通过`sync()`执行的，我们来追踪下`sync()`的调用
除了`addObserver()`时的同步之外，只有另一处调用：

```
	private void moveToState(State next) {
        if (mState == next) {
            return;
        }
        mState = next;
        if (mHandlingEvent || mAddingObserverCounter != 0) {
            mNewEventOccurred = true;
            // we will figure out what to do on upper level.
            return;
        }
        mHandlingEvent = true;
        sync();
        mHandlingEvent = false;
    }
```

`moveToState(State next)`的调用也有两处，第一处：

```
 ·@CallSuper
    @Override
    protected void onSaveInstanceState(@NonNull Bundle outState) {
        mLifecycleRegistry.markState(Lifecycle.State.CREATED);
        super.onSaveInstanceState(outState);
    }
	
	@Deprecated
    @MainThread
    public void markState(@NonNull State state) {
        setCurrentState(state);
    }
	
	@MainThread
    public void setCurrentState(@NonNull State state) {
        moveToState(state);
    }
```

第二处：

```
    public void handleLifecycleEvent(@NonNull Lifecycle.Event event) {
        State next = getStateAfter(event);
        moveToState(next);
    }
```
那么，`handleLifecycleEvent()`有是哪里调用的呢？至此线索已经断了

继续从`ComponentActivity`里面找，因为生命周期的所有者是`ComponentActivity`，那么其相关事件也一定是在`ComponentActivity`的生命周期方法中触发。

在`androidx.core.app.ComponentActivity`的`onCreate`中

```
	@Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        mSavedStateRegistryController.performRestore(savedInstanceState);
		// Report ? 报告吗？
        ReportFragment.injectIfNeededIn(this);
        if (mContentLayoutId != 0) {
            setContentView(mContentLayoutId);
        }
    }
```
进入`ReportFragment`看看，果然

```
	@Override
    public void onActivityCreated(Bundle savedInstanceState) {
        super.onActivityCreated(savedInstanceState);
        dispatchCreate(mProcessListener);
        dispatch(Lifecycle.Event.ON_CREATE);
    }

    @Override
    public void onStart() {
        super.onStart();
        dispatchStart(mProcessListener);
        dispatch(Lifecycle.Event.ON_START);
    }

    @Override
    public void onResume() {
        super.onResume();
        dispatchResume(mProcessListener);
        dispatch(Lifecycle.Event.ON_RESUME);
    }

    @Override
    public void onPause() {
        super.onPause();
        dispatch(Lifecycle.Event.ON_PAUSE);
    }

    @Override
    public void onStop() {
        super.onStop();
        dispatch(Lifecycle.Event.ON_STOP);
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        dispatch(Lifecycle.Event.ON_DESTROY);
        // just want to be sure that we won't leak reference to an activity
        mProcessListener = null;
    }
```
生命周期事件分发

```
    private void dispatch(@NonNull Lifecycle.Event event) {
        if (Build.VERSION.SDK_INT < 29) {
            // Only dispatch events from ReportFragment on API levels prior
            // to API 29. On API 29+, this is handled by the ActivityLifecycleCallbacks
            // added in ReportFragment.injectIfNeededIn
            dispatch(getActivity(), event);
        }
    }
	
    static void dispatch(@NonNull Activity activity, @NonNull Lifecycle.Event event) {
        if (activity instanceof LifecycleRegistryOwner) {
            ((LifecycleRegistryOwner) activity).getLifecycle().handleLifecycleEvent(event);
            return;
        }

        if (activity instanceof LifecycleOwner) {
            Lifecycle lifecycle = ((LifecycleOwner) activity).getLifecycle();
            if (lifecycle instanceof LifecycleRegistry) {
                ((LifecycleRegistry) lifecycle).handleLifecycleEvent(event);
            }
        }
    }
```

原理：通过`ReportFragment`对`ComponentActivity`的生命周期做监听，并回调。本质是跟`Glide`的做法一样。





参考：


[Android LifeCycle原理分析 - 简书](https://www.jianshu.com/p/aff113e588e6)

[Lifecycle源码解析，让你一次学个够！](https://mp.weixin.qq.com/s/7Z9UmvMCI90EyzFovV_rEw)

[Retrofit结合Lifecycle, 将Http生命周期管理到极致 - 简书](https://www.jianshu.com/p/07fe489a53f2)

[JetPack系列：Lifecycle - 简书](https://www.jianshu.com/p/fa16e6b592a7)
