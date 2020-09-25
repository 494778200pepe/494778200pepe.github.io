---
layout: post
title:  "Jetpack - LiveData"
date:   2020-09-24 16:24:00 +0800
categories: Android
tags: Jetpack
author: pepe
description: 『 LiveData 』
---

官方文档：

[LiveData 概览  Android Developers](https://developer.android.google.cn/topic/libraries/architecture/livedata)


### **LiveData**

> LiveData 是一种可观察的数据存储器类。与常规的可观察类不同，LiveData 具有生命周期感知能力，意指它遵循其他应用组件（如 Activity、Fragment 或 Service）的生命周期。这种感知能力可确保 LiveData 仅更新处于活跃生命周期状态的应用组件观察者。

活跃状态：STARTED 或 RESUMED状态

因为LiveData只会更新处于前台的activty的数据，所以LiveData具有以下

* 优点

	。不会发生内存泄露
	
		观察者会绑定到 Lifecycle 对象，并在其关联的生命周期遭到销毁后进行自我清理。
	
	。不会因 Activity 停止而导致崩溃
		
		如果观察者的生命周期处于非活跃状态（如返回栈中的 Activity），则它不会接收任何 LiveData 事件。
	
	。不再需要手动处理生命周期
	
		界面组件只是观察相关数据，不会停止或恢复观察。LiveData 将自动管理所有这些操作，因为它在观察时可以感知相关的生命周期状态变化。
	
	。数据始终保持最新状态
		
		如果生命周期变为非活跃状态，它会在再次变为活跃状态时接收最新的数据。例如，曾经在后台的 Activity 会在返回前台后立即接收最新的数据。


源码分析：

```
	@MainThread
    public void observe(@NonNull LifecycleOwner owner, @NonNull Observer<? super T> observer) {
		// 断言主线程
        assertMainThread("observe");
		// 如果已经被销毁
        if (owner.getLifecycle().getCurrentState() == DESTROYED) {
            // ignore
            return;
        }
		// 包一层
        LifecycleBoundObserver wrapper = new LifecycleBoundObserver(owner, observer);
		// 是否已经保存在了 mObservers 这个 map 中，如果存在，返回已经保存的那个 LifecycleBoundObserver
        ObserverWrapper existing = mObservers.putIfAbsent(observer, wrapper);
		// 如果已经保存了，那么判断现在的 LifecycleOwner 和保存的 LifecycleOwner 是否是同一个，如果不是同一个，报错
        if (existing != null && !existing.isAttachedTo(owner)) {
            throw new IllegalArgumentException("Cannot add the same observer"
                    + " with different lifecycles");
        }
		// 如果已经保存了，返回
        if (existing != null) {
            return;
        }
		// ComponentActivity 获取 LifecycleRegistry，添加 Observer
        owner.getLifecycle().addObserver(wrapper);
    }
```

本质上，还是在 Lifecycle 里面添加一个回调。

再来看看，发送数据的方法：

```
	// 注意：setValue在主线程使用；postValue在子线程使用
	@MainThread
    protected void setValue(T value) {
		// 确保是主线程
        assertMainThread("setValue");
		// 数据的版本 +1
        mVersion++;
		// 保存数据
        mData = value;
		// 分发数据
        dispatchingValue(null);
    }
	
	void dispatchingValue(@Nullable ObserverWrapper initiator) {
		// 正在分发数据中，也就是在下面的循环中，直接返回，
		// mDispatchInvalidated = true; 循环会多执行一次
        if (mDispatchingValue) {
            mDispatchInvalidated = true;
            return;
        }
        mDispatchingValue = true;
        do {
            mDispatchInvalidated = false;
            if (initiator != null) {
                considerNotify(initiator);
                initiator = null;
            } else {
				// 为 null 时，遍历所有的监听，更新数据
                for (Iterator<Map.Entry<Observer<? super T>, ObserverWrapper>> iterator =
                        mObservers.iteratorWithAdditions(); iterator.hasNext(); ) {
                    considerNotify(iterator.next().getValue());
                    if (mDispatchInvalidated) {
                        break;
                    }
                }
            }
        } while (mDispatchInvalidated);
        mDispatchingValue = false;
    }
	
	// 考虑通知
	private void considerNotify(ObserverWrapper observer) {
		// 一开始observer如果不是活跃状态（STARTED 或 RESUMED状态）直接退出
		// 检查 LiveData 里面是不是有 active 的 Observer，如果没有就 return
        if (!observer.mActive) {
            return;
        }
        // Check latest state b4 dispatch. Maybe it changed state but we didn't get the event yet.
        //
        // we still first check observer.active to keep it as the entrance for events. So even if
        // the observer moved to an active state, if we've not received that event, we better not
        // notify for a more predictable notification order.
		// 检查最新状态以备调度。也许它改变了状态，但我们还没有得到活动。
		// 我们还是先检查一下观察者。主动把它作为活动的入口。所以即使观察者移动到一个活动状态，
		// 如果我们没有收到那个事件，我们最好不要通知以获得更可预测的通知顺序。
		
		
		// 二次检查
		//此处判断主要是为了照顾 LifecycleBoundObserver
        //由于 Lifecycle 有可能状态值 State 已经切换到了非活跃状态，但 LifecycleBoundObserver 还未收到事件通知
        //所以为了避免意外情况，此处主动检查 observer 的活跃状态并判断是否需要更新其活跃状态

        if (!observer.shouldBeActive()) {
			// 如果不是 activie 的状态，那么就去更新 ObserverWrapper 里面的 active 状态
			// 因为不是 activie 的状态，所以传入的参数为 false
            observer.activeStateChanged(false);
            return;
        }
		
		// 如果是 activie 状态，那么检查版本
        if (observer.mLastVersion >= mVersion) {
            return;
        }
		// 更新数据
        observer.mLastVersion = mVersion;
        observer.mObserver.onChanged((T) mData);
    }
	
	@Override
    boolean shouldBeActive() {
		// 判断状态是不是大于等于STARTED，比STARTED大的就是RESUMED
        return mOwner.getLifecycle().getCurrentState().isAtLeast(STARTED);
    }
	
	
```

> 总结：LiveData通过addObserver绑定到Lifecycles上，所以拥有了感应生命周期的能力，而且LiveData只会去更新处于活跃状态的应用组件观察者，如果界面还在后台，LiveData是不会去发送数据的，这样做大大提高了app的性能。





参考：

[Jetpack使用（二）LiveData核心原理 - 简书](https://www.jianshu.com/p/ac6888134fa5)