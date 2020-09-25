---
layout: post
title:  "Jetpack - ViewModel"
date:   2020-09-24 16:24:00 +0800
categories: Android
tags: Jetpack
author: pepe
description: 『 ViewModel 』
---

官方文档：

[ViewModel 概览  Android Developers](https://developer.android.google.cn/topic/libraries/architecture/viewmodel)

痛点：

* 如果系统销毁或重新创建界面控制器，则存储在其中的任何临时性界面相关数据都会丢失。对于简单的数据，Activity 可以使用 `onSaveInstanceState()`方法从`onCreate()`中的捆绑包恢复其数据，但此方法仅适合可以序列化再反序列化的少量数据，而不适合数量可能较大的数据，如用户列表或位图。

* 界面控制器经常需要进行异步调用，这些调用可能需要一些时间才能返回结果。界面控制器需要管理这些调用，并确保系统在其销毁后清理这些调用以避免潜在的内存泄露。此项管理需要大量的维护工作，并且在因配置更改而重新创建对象的情况下，会造成资源的浪费，因为对象可能需要重新发出已经发出过的调用。
	
* `Activity`和`Fragment`承载过多，代码膨胀。需要将从数据库或网络加载数据的任务分离出去。

### **ViewModel：**

> 架构组件为界面控制器提供了`ViewModel`辅助程序类，该类负责为界面准备数据。在配置更改期间会自动保留`ViewModel`对象，以便它们存储的数据立即可供下一个`Activity`或`Fragment`实例使用。例如，如果您需要在应用中显示用户列表，请确保将获取和保留该用户列表的责任分配给 `ViewModel`，而不是`Activity`或`Fragment`.

`ViewModel`保存数据的原理：


先从`ViewModel`的构造开始：
```
	model = new ViewModelProvider(this).get(TestViewModel.class);
	
	public ViewModelProvider(@NonNull ViewModelStoreOwner owner) {
        this(owner.getViewModelStore(), owner instanceof HasDefaultViewModelProviderFactory
                ? ((HasDefaultViewModelProviderFactory) owner).getDefaultViewModelProviderFactory()
                : NewInstanceFactory.getInstance());
    }
	
	public ViewModelProvider(@NonNull ViewModelStore store, @NonNull Factory factory) {
	
		// ComponentActivity 并没有实现 HasDefaultViewModelProviderFactory
		// 所以，mFactory = NewInstanceFactory.getInstance()
		// 里面通过反射实例化 ViewModel: modelClass.newInstance();
		
        mFactory = factory;
        mViewModelStore = store;
    }
```

重点来看看`owner.getViewModelStore()`

```
	@NonNull
    @Override
    public ViewModelStore getViewModelStore() {
        if (getApplication() == null) {
            throw new IllegalStateException("Your activity is not yet attached to the "
                    + "Application instance. You can't request ViewModel before onCreate call.");
        }
        if (mViewModelStore == null) {
            NonConfigurationInstances nc =
                    (NonConfigurationInstances) getLastNonConfigurationInstance();
			// 第一次进入时，nc 和 mViewModelStore 都为 null，new 一个
            if (nc != null) {
                // Restore the ViewModelStore from NonConfigurationInstances
                mViewModelStore = nc.viewModelStore;
            }
            if (mViewModelStore == null) {
                mViewModelStore = new ViewModelStore();
            }
        }
        return mViewModelStore;
    }
	
	// NonConfigurationInstances 是 ComponentActivity 的静态内部类
    static final class NonConfigurationInstances {
        Object custom;
        ViewModelStore viewModelStore;
    }
```

追踪`getLastNonConfigurationInstance()`

```
	@Nullable
    public Object getLastNonConfigurationInstance() {
		// mLastNonConfigurationInstances 是 Activity 的静态内部类
        return mLastNonConfigurationInstances != null
                ? mLastNonConfigurationInstances.activity : null;
    }
```

`mLastNonConfigurationInstances.activity` 保存的是 一个`ComponentActivity`的内部类`NonConfigurationInstances`

那么，它是什么时候赋值的呢？

```
	// 方法名：保留非配置实例
    NonConfigurationInstances retainNonConfigurationInstances() {
        Object activity = onRetainNonConfigurationInstance();
        HashMap<String, Object> children = onRetainNonConfigurationChildInstances();
        FragmentManagerNonConfig fragments = mFragments.retainNestedNonConfig();

        // We're already stopped but we've been asked to retain.
        // Our fragments are taken care of but we need to mark the loaders for retention.
        // In order to do this correctly we need to restart the loaders first before
        // handing them off to the next activity.
        mFragments.doLoaderStart();
        mFragments.doLoaderStop(true);
        ArrayMap<String, LoaderManager> loaders = mFragments.retainLoaderNonConfig();

        if (activity == null && children == null && fragments == null && loaders == null
                && mVoiceInteractor == null) {
            return null;
        }

        NonConfigurationInstances nci = new NonConfigurationInstances();
        nci.activity = activity;
        nci.children = children;
        nci.fragments = fragments;
        nci.loaders = loaders;
        if (mVoiceInteractor != null) {
            mVoiceInteractor.retainInstance();
            nci.voiceInteractor = mVoiceInteractor;
        }
        return nci;
    }
	
	// 复制的对象从哪儿来的呢？
	// 追踪 onRetainNonConfigurationInstance()
	@Override
    @Nullable
    public final Object onRetainNonConfigurationInstance() {
        Object custom = onRetainCustomNonConfigurationInstance();
		
		// 保存的其实就是 一开始 new 的 mViewModelStore
		
        ViewModelStore viewModelStore = mViewModelStore;
        if (viewModelStore == null) {
            // No one called getViewModelStore(), so see if there was an existing
            // ViewModelStore from our last NonConfigurationInstance
            NonConfigurationInstances nc =
                    (NonConfigurationInstances) getLastNonConfigurationInstance();
            if (nc != null) {
                viewModelStore = nc.viewModelStore;
            }
        }

        if (viewModelStore == null && custom == null) {
            return null;
        }

        NonConfigurationInstances nci = new NonConfigurationInstances();
        nci.custom = custom;
        nci.viewModelStore = viewModelStore;
        return nci;
    }
```

`ViewModel`保存在`ViewModelStore`里面，`ViewModelStore`保存在`NonConfigurationInstances`里面，那么`NonConfigurationInstances`又是通过什么保存的呢？

继续跟踪`retainNonConfigurationInstances()`的调用
	
```
	// ActivityThread 中
	// 在`Activity`销毁时，会调用`retainNonConfigurationInstances()`
	
	/** Core implementation of activity destroy call. */
    ActivityClientRecord performDestroyActivity(IBinder token, boolean finishing,
            int configChanges, boolean getNonConfigInstance, String reason) {
        ActivityClientRecord r = mActivities.get(token);
        Class<? extends Activity> activityClass = null;
        if (localLOGV) Slog.v(TAG, "Performing finish of " + r);
        if (r != null) {
            activityClass = r.activity.getClass();
            r.activity.mConfigChangeFlags |= configChanges;
            if (finishing) {
                r.activity.mFinished = true;
            }

            performPauseActivityIfNeeded(r, "destroy");

            if (!r.stopped) {
                callActivityOnStop(r, false /* saveState */, "destroy");
            }
            if (getNonConfigInstance) {
                try {
				
					// NonConfigurationInstances 作为返回值，保存在了 ActivityClientRecord 里面
					
                    r.lastNonConfigurationInstances
                            = r.activity.retainNonConfigurationInstances();
                } catch (Exception e) {
                    if (!mInstrumentation.onException(r.activity, e)) {
                        throw new RuntimeException(
                                "Unable to retain activity "
                                + r.intent.getComponent().toShortString()
                                + ": " + e.toString(), e);
                    }
                }
            }
            try {
                r.activity.mCalled = false;
                mInstrumentation.callActivityOnDestroy(r.activity);
                if (!r.activity.mCalled) {
                    throw new SuperNotCalledException(
                        "Activity " + safeToComponentShortString(r.intent) +
                        " did not call through to super.onDestroy()");
                }
                if (r.window != null) {
                    r.window.closeAllPanels();
                }
            } catch (SuperNotCalledException e) {
                throw e;
            } catch (Exception e) {
                if (!mInstrumentation.onException(r.activity, e)) {
                    throw new RuntimeException(
                            "Unable to destroy activity " + safeToComponentShortString(r.intent)
                            + ": " + e.toString(), e);
                }
            }
            r.setState(ON_DESTROY);
        }
        mActivities.remove(token);
        StrictMode.decrementExpectedActivityCount(activityClass);
        return r;
    }
```

那么`ActivityClientRecord`，又是什么时候保存的呢？

追踪`lastNonConfigurationInstances`的调用

```
    private Activity performLaunchActivity(ActivityClientRecord r, Intent customIntent) {
       
		...
	   
		// Activity 初始化的时候，使用到了 lastNonConfigurationInstances
		// 那么这里的 ActivityClientRecord 从哪儿来的呢？
        appContext.setOuterContext(activity);
                activity.attach(appContext, this, getInstrumentation(), r.token,
                        r.ident, app, r.intent, r.activityInfo, title, r.parent,
                        r.embeddedID, r.lastNonConfigurationInstances, config,
                        r.referrer, r.voiceInteractor, window, r.configCallback);

		...
			
        return activity;
    }
	
	@Override
    public Activity handleLaunchActivity(ActivityClientRecord r,
            PendingTransactionActions pendingActions, Intent customIntent) {
        
		...

        final Activity a = performLaunchActivity(r, customIntent);

        ...

        return a;
    }
	
	private void handleRelaunchActivityInner(ActivityClientRecord r, int configChanges,
            List<ResultInfo> pendingResults, List<ReferrerIntent> pendingIntents,
            PendingTransactionActions pendingActions, boolean startsNotResumed,
            Configuration overrideConfig, String reason) {
        // Preserve last used intent, it may be set from Activity#setIntent().
       
		...
		
        handleLaunchActivity(r, pendingActions, customIntent);
    }
	
	 @Override
    public void handleRelaunchActivity(ActivityClientRecord tmp,
            PendingTransactionActions pendingActions) {
        // If we are getting ready to gc after going to the background, well
        // we are back active so skip it.
        unscheduleGcIdler();
        mSomeActivitiesChanged = true;

        Configuration changedConfig = null;
        int configChanges = 0;

        // First: make sure we have the most recent configuration and most
        // recent version of the activity, or skip it if some previous call
        // had taken a more recent version.
        synchronized (mResourcesManager) {
            int N = mRelaunchingActivities.size();
            IBinder token = tmp.token;
            tmp = null;
            for (int i=0; i<N; i++) {
                ActivityClientRecord r = mRelaunchingActivities.get(i);
                if (r.token == token) {
                    tmp = r;
                    configChanges |= tmp.pendingConfigChanges;
                    mRelaunchingActivities.remove(i);
                    i--;
                    N--;
                }
            }
        }

        ...

		// 通过一个 ArrayMap，Binder 为 key，保存的 ActivityClientRecord
		
        ActivityClientRecord r = mActivities.get(tmp.token);
        if (DEBUG_CONFIGURATION) Slog.v(TAG, "Handling relaunch of " + r);
        if (r == null) {
            return;
        }

        r.activity.mConfigChangeFlags |= configChanges;
        r.mPreserveWindow = tmp.mPreserveWindow;
        r.activity.mChangingConfigurations = true;

       

        handleRelaunchActivityInner(r, configChanges, tmp.pendingResults, tmp.pendingIntents,
                pendingActions, tmp.startsNotResumed, tmp.overrideConfig, "handleRelaunchActivity");

        if (pendingActions != null) {
            // Only report a successful relaunch to WindowManager.
            pendingActions.setReportRelaunchToWindowManager(true);
        }
    }
	
	 final ArrayMap<IBinder, ActivityClientRecord> mActivities = new ArrayMap<>();

```

至此，分析完毕。`destory`的时候，保存`ViewModelStore`到`NonConfigurationInstances`里面，`NonConfigurationInstances`保存到`ActivityClientRecord`里面。
`reLaunch`的时候，从`mActivities`里面恢复`ActivityClientRecord`，然后传给`activity`的`attach()`。

```
	// Activity 
	final void attach(Context context, ActivityThread aThread,
            Instrumentation instr, IBinder token, int ident,
            Application application, Intent intent, ActivityInfo info,
            CharSequence title, Activity parent, String id,
            NonConfigurationInstances lastNonConfigurationInstances,
            Configuration config, String referrer, IVoiceInteractor voiceInteractor,
            Window window, ActivityConfigCallback activityConfigCallback) {
        attachBaseContext(context);

        mFragments.attachHost(null /*parent*/);

        mWindow = new PhoneWindow(this, window, activityConfigCallback);
        mWindow.setWindowControllerCallback(this);
        mWindow.setCallback(this);
        mWindow.setOnWindowDismissedCallback(this);
        mWindow.getLayoutInflater().setPrivateFactory(this);
        if (info.softInputMode != WindowManager.LayoutParams.SOFT_INPUT_STATE_UNSPECIFIED) {
            mWindow.setSoftInputMode(info.softInputMode);
        }
        if (info.uiOptions != 0) {
            mWindow.setUiOptions(info.uiOptions);
        }
        mUiThread = Thread.currentThread();

        mMainThread = aThread;
        mInstrumentation = instr;
        mToken = token;
        mIdent = ident;
        mApplication = application;
        mIntent = intent;
        mReferrer = referrer;
        mComponent = intent.getComponent();
        mActivityInfo = info;
        mTitle = title;
        mParent = parent;
        mEmbeddedID = id;
		
		// 恢复 lastNonConfigurationInstances
        mLastNonConfigurationInstances = lastNonConfigurationInstances;
		
        if (voiceInteractor != null) {
            if (lastNonConfigurationInstances != null) {
                mVoiceInteractor = lastNonConfigurationInstances.voiceInteractor;
            } else {
                mVoiceInteractor = new VoiceInteractor(voiceInteractor, this, this,
                        Looper.myLooper());
            }
        }

        mWindow.setWindowManager(
                (WindowManager)context.getSystemService(Context.WINDOW_SERVICE),
                mToken, mComponent.flattenToString(),
                (info.flags & ActivityInfo.FLAG_HARDWARE_ACCELERATED) != 0);
        if (mParent != null) {
            mWindow.setContainer(mParent.getWindow());
        }
        mWindowManager = mWindow.getWindowManager();
        mCurrentConfig = config;

        mWindow.setColorMode(info.colorMode);

        setAutofillCompatibilityEnabled(application.isAutofillCompatibilityEnabled());
        enableAutofillCompatibilityIfNeeded();
    }
```


参考：

[从源码看 Jetpack（6）-ViewModel源码详解](https://juejin.im/post/6873356946896846856)

[Jetpack使用（四）ViewModel核心原理 - 简书](https://www.jianshu.com/p/00799899fe7b)


