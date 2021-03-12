---
layout: post
title:  "插件化 - getResources 源码"
date:   2021-03-09 16:18:00 +0800
categories: Android
tags: Plugin
author: pepe
description: 『 getResources 源码 』
---

AppCompatActivity:
```
	@Override
    public Resources getResources() {
        if (mResources == null && VectorEnabledTintResources.shouldBeUsed()) {
            mResources = new VectorEnabledTintResources(this, super.getResources());
        }
        return mResources == null ? super.getResources() : mResources;
    }
```

ContextThemeWrapper:
```
	@Override
    public Resources getResources() {
        return getResourcesInternal();
    }
```

Context:
```
    public abstract Resources getResources();
```

ContextImpl:
```
    @Override
    public Resources getResources() {
        return mResources;
    }
	
    private ContextImpl(ContextImpl container, ActivityThread mainThread,
            LoadedApk packageInfo, IBinder activityToken, UserHandle user, boolean restricted,
            Display display, Configuration overrideConfiguration, int createDisplayWithId) {
        mOuterContext = this;

        mMainThread = mainThread;
        mActivityToken = activityToken;
        mRestricted = restricted;

        if (user == null) {
            user = Process.myUserHandle();
        }
        mUser = user;

        mPackageInfo = packageInfo;
        mResourcesManager = ResourcesManager.getInstance();

        final int displayId = (createDisplayWithId != Display.INVALID_DISPLAY)
                ? createDisplayWithId
                : (display != null) ? display.getDisplayId() : Display.DEFAULT_DISPLAY;

        CompatibilityInfo compatInfo = null;
        if (container != null) {
            compatInfo = container.getDisplayAdjustments(displayId).getCompatibilityInfo();
        }
        if (compatInfo == null) {
            compatInfo = (displayId == Display.DEFAULT_DISPLAY)
                    ? packageInfo.getCompatibilityInfo()
                    : CompatibilityInfo.DEFAULT_COMPATIBILITY_INFO;
        }
        mDisplayAdjustments.setCompatibilityInfo(compatInfo);
        mDisplayAdjustments.setConfiguration(overrideConfiguration);

        mDisplay = (createDisplayWithId == Display.INVALID_DISPLAY) ? display
                : ResourcesManager.getInstance().getAdjustedDisplay(displayId, mDisplayAdjustments);

        Resources resources = packageInfo.getResources(mainThread);
        if (resources != null) {
            if (displayId != Display.DEFAULT_DISPLAY
                    || overrideConfiguration != null
                    || (compatInfo != null && compatInfo.applicationScale
                            != resources.getCompatibilityInfo().applicationScale)) {
				// 重点
                resources = mResourcesManager.getTopLevelResources(packageInfo.getResDir(),
                        packageInfo.getSplitResDirs(), packageInfo.getOverlayDirs(),
                        packageInfo.getApplicationInfo().sharedLibraryFiles, displayId,
                        overrideConfiguration, compatInfo);
            }
        }
		
        mResources = resources;

        if (container != null) {
            mBasePackageName = container.mBasePackageName;
            mOpPackageName = container.mOpPackageName;
        } else {
            mBasePackageName = packageInfo.mPackageName;
            ApplicationInfo ainfo = packageInfo.getApplicationInfo();
            if (ainfo.uid == Process.SYSTEM_UID && ainfo.uid != Process.myUid()) {
                // Special case: system components allow themselves to be loaded in to other
                // processes.  For purposes of app ops, we must then consider the context as
                // belonging to the package of this process, not the system itself, otherwise
                // the package+uid verifications in app ops will fail.
                mOpPackageName = ActivityThread.currentPackageName();
            } else {
                mOpPackageName = mBasePackageName;
            }
        }

        mContentResolver = new ApplicationContentResolver(this, mainThread, user);
    }
```

ResourcesManager，直接是用 AssetManager 构建的:
```
    Resources getTopLevelResources(String resDir, String[] splitResDirs,
            String[] overlayDirs, String[] libDirs, int displayId,
            Configuration overrideConfiguration, CompatibilityInfo compatInfo) {
        final float scale = compatInfo.applicationScale;
        Configuration overrideConfigCopy = (overrideConfiguration != null)
                ? new Configuration(overrideConfiguration) : null;
        ResourcesKey key = new ResourcesKey(resDir, displayId, overrideConfigCopy, scale);
        Resources r;
        synchronized (this) {
            // Resources is app scale dependent.
            if (DEBUG) Slog.w(TAG, "getTopLevelResources: " + resDir + " / " + scale);

            WeakReference<Resources> wr = mActiveResources.get(key);
            r = wr != null ? wr.get() : null;
            //if (r != null) Log.i(TAG, "isUpToDate " + resDir + ": " + r.getAssets().isUpToDate());
            if (r != null && r.getAssets().isUpToDate()) {
                if (DEBUG) Slog.w(TAG, "Returning cached resources " + r + " " + resDir
                        + ": appScale=" + r.getCompatibilityInfo().applicationScale
                        + " key=" + key + " overrideConfig=" + overrideConfiguration);
                return r;
            }
        }

        //if (r != null) {
        //    Log.w(TAG, "Throwing away out-of-date resources!!!! "
        //            + r + " " + resDir);
        //}

		// 重点
        AssetManager assets = new AssetManager();
        // resDir can be null if the 'android' package is creating a new Resources object.
        // This is fine, since each AssetManager automatically loads the 'android' package
        // already.
        if (resDir != null) {
			// 重点
            if (assets.addAssetPath(resDir) == 0) {
                return null;
            }
        }

        if (splitResDirs != null) {
            for (String splitResDir : splitResDirs) {
                if (assets.addAssetPath(splitResDir) == 0) {
                    return null;
                }
            }
        }

        if (overlayDirs != null) {
            for (String idmapPath : overlayDirs) {
                assets.addOverlayPath(idmapPath);
            }
        }

        if (libDirs != null) {
            for (String libDir : libDirs) {
                if (libDir.endsWith(".apk")) {
                    // Avoid opening files we know do not have resources,
                    // like code-only .jar files.
                    if (assets.addAssetPath(libDir) == 0) {
                        Log.w(TAG, "Asset path '" + libDir +
                                "' does not exist or contains no resources.");
                    }
                }
            }
        }

        //Log.i(TAG, "Resource: key=" + key + ", display metrics=" + metrics);
        DisplayMetrics dm = getDisplayMetricsLocked(displayId);
        Configuration config;
        final boolean isDefaultDisplay = (displayId == Display.DEFAULT_DISPLAY);
        final boolean hasOverrideConfig = key.hasOverrideConfiguration();
        if (!isDefaultDisplay || hasOverrideConfig) {
            config = new Configuration(getConfiguration());
            if (!isDefaultDisplay) {
                applyNonDefaultDisplayMetricsToConfigurationLocked(dm, config);
            }
            if (hasOverrideConfig) {
                config.updateFrom(key.mOverrideConfiguration);
                if (DEBUG) Slog.v(TAG, "Applied overrideConfig=" + key.mOverrideConfiguration);
            }
        } else {
            config = getConfiguration();
        }
		// 重点
        r = new Resources(assets, dm, config, compatInfo);
        if (DEBUG) Slog.i(TAG, "Created app resources " + resDir + " " + r + ": "
                + r.getConfiguration() + " appScale=" + r.getCompatibilityInfo().applicationScale);

        synchronized (this) {
            WeakReference<Resources> wr = mActiveResources.get(key);
            Resources existing = wr != null ? wr.get() : null;
            if (existing != null && existing.getAssets().isUpToDate()) {
                // Someone else already created the resources while we were
                // unlocked; go ahead and use theirs.
                r.getAssets().close();
                return existing;
            }

            // XXX need to remove entries when weak references go away
            mActiveResources.put(key, new WeakReference<>(r));
            if (DEBUG) Slog.v(TAG, "mActiveResources.size()=" + mActiveResources.size());
            return r;
        }
    }
```

资源加载的总结：所有的资源加载通过Resource  -> 构建对象是直接new的对象  -> AssetManager  其实是Resource的核心实例 -> 最终是通过AssetManager获取

AssetManager 实例化：

```
AssetManager asset = new AssetManager();
assets.addAssetPath(resDir)// 可以是一个zip的路径
```

















