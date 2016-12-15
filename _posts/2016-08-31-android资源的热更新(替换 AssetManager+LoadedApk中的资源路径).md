---
layout: post
title: "android资源的热更新(替换 AssetManager+LoadedApk中的资源路径)"
date: 2016-08-31 14:04:00 
comments: true
categories: [android]
tags: [android]
description: "android资源的热更新(替换 AssetManager+LoadedApk中的资源路径)"
keywords: android
---


 
  
   
    设置LoadedApk中的mResDir
   
   
    
     创建AssetManager, 设置资源包路径
    
    
     
      替换AssetManager
     
    
   
  
  
AssetManager newAssetManager = (AssetManager) AssetManager.class.getConstructor(new Class[0])
        .newInstance(new Object[0]);
Method mAddAssetPath = AssetManager.class.getDeclaredMethod("addAssetPath",
        new Class[] {
                String.class
        });
mAddAssetPath.setAccessible(true);
if (((Integer) mAddAssetPath.invoke(newAssetManager, new Object[] {
        externalResourceFile
})).intValue() == 0) {
    throw new IllegalStateException("Could not create new AssetManager");
}

Method mEnsureStringBlocks = AssetManager.class.getDeclaredMethod("ensureStringBlocks",
        new Class[0]);
mEnsureStringBlocks.setAccessible(true);
mEnsureStringBlocks.invoke(newAssetManager, new Object[0]);

if (activities != null)
    for (Activity activity : activities) {
        Resources resources = activity.getResources();
        try {
            Field mAssets = Resources.class.getDeclaredField("mAssets");
            mAssets.setAccessible(true);
            mAssets.set(resources, newAssetManager);
        } catch (Throwable ignore) {
            Field mResourcesImpl = Resources.class.getDeclaredField("mResourcesImpl");
            mResourcesImpl.setAccessible(true);
            Object resourceImpl = mResourcesImpl.get(resources);
            Field implAssets = resourceImpl.getClass().getDeclaredField("mAssets");
            implAssets.setAccessible(true);
            implAssets.set(resourceImpl, newAssetManager);
        }

        Resources.Theme theme = activity.getTheme();
        try {
            try {
                Field ma = Resources.Theme.class.getDeclaredField("mAssets");
                ma.setAccessible(true);
                ma.set(theme, newAssetManager);
            } catch (NoSuchFieldException ignore) {
                Field themeField = Resources.Theme.class.getDeclaredField("mThemeImpl");
                themeField.setAccessible(true);
                Object impl = themeField.get(theme);
                Field ma = impl.getClass().getDeclaredField("mAssets");
                ma.setAccessible(true);
                ma.set(impl, newAssetManager);
            }

            Field mt = ContextThemeWrapper.class.getDeclaredField("mTheme");
            mt.setAccessible(true);
            mt.set(activity, null);
            Method mtm = ContextThemeWrapper.class.getDeclaredMethod("initializeTheme",
                    new Class[0]);
            mtm.setAccessible(true);
            mtm.invoke(activity, new Object[0]);

            Method mCreateTheme = AssetManager.class.getDeclaredMethod("createTheme",
                    new Class[0]);
            mCreateTheme.setAccessible(true);
            Object internalTheme = mCreateTheme.invoke(newAssetManager, new Object[0]);
            Field mTheme = Resources.Theme.class.getDeclaredField("mTheme");
            mTheme.setAccessible(true);
            mTheme.set(theme, internalTheme);
        } catch (Throwable e) {
            Log.e("InstantRun",
                    "Failed to update existing theme for activity " + activity, e);
        }

        pruneResourceCaches(resources);
    }

Collection references;
if (Build.VERSION.SDK_INT >;= 19) {
    Class resourcesManagerClass = Class.forName("android.app.ResourcesManager");
    Method mGetInstance = resourcesManagerClass.getDeclaredMethod("getInstance",
            new Class[0]);
    mGetInstance.setAccessible(true);
    Object resourcesManager = mGetInstance.invoke(null, new Object[0]);
    try {
        Field fMActiveResources = resourcesManagerClass
                .getDeclaredField("mActiveResources");
        fMActiveResources.setAccessible(true);

        ArrayMap arrayMap = (ArrayMap) fMActiveResources.get(resourcesManager);

        references = arrayMap.values();
    } catch (NoSuchFieldException ignore) {
        Field mResourceReferences = resourcesManagerClass
                .getDeclaredField("mResourceReferences");
        mResourceReferences.setAccessible(true);

        Collection references = (Collection) mResourceReferences.get(resourcesManager);
    }
} else {
    Class activityThread = Class.forName("android.app.ActivityThread");
    Field fMActiveResources = activityThread.getDeclaredField("mActiveResources");
    fMActiveResources.setAccessible(true);
    Object thread = getActivityThread(context, activityThread);

    HashMap map = (HashMap) fMActiveResources.get(thread);

    references = map.values();
}

for (WeakReference wr : references) {
    Resources resources = (Resources) wr.get();
    if (resources != null) {
        try {
            Field mAssets = Resources.class.getDeclaredField("mAssets");
            mAssets.setAccessible(true);
            mAssets.set(resources, newAssetManager);
        } catch (Throwable ignore) {
            Field mResourcesImpl = Resources.class.getDeclaredField("mResourcesImpl");
            mResourcesImpl.setAccessible(true);
            Object resourceImpl = mResourcesImpl.get(resources);
            Field implAssets = resourceImpl.getClass().getDeclaredField("mAssets");
            implAssets.setAccessible(true);
            implAssets.set(resourceImpl, newAssetManager);
        }

        resources.updateConfiguration(resources.getConfiguration(),
                resources.getDisplayMetrics());
    }
}


 
 
  $(function () {
                $('pre.prettyprint code').each(function () {
                    var lines = $(this).text().split('\n').length;
                    var $numbering = $('').addClass('pre-numbering').hide();
                    $(this).addClass('has-numbering').parent().append($numbering);
                    for (i = 1; i ').text(i));
                    };
                    $numbering.fadeIn(1700);
                });
            });
 


