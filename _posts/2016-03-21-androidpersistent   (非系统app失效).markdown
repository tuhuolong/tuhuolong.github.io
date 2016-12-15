---
layout: post
title: "android:persistent   (非系统app失效)"
date: 2016-03-21 15:08:00 
comments: true
categories: [android]
tags: [android]
description: "android:persistent   (非系统app失效)"
keywords: android
---


 
  
   非系统app设置persistent无效
  
  安装时
if ((flags&PARSE_IS_SYSTEM) != 0) {
            if (sa.getBoolean(
                    com.android.internal.R.styleable.AndroidManifestApplication_persistent,
                    false)) {
                ai.flags |= ApplicationInfo.FLAG_PERSISTENT;
            }
        }
  
   
    
   
  
  
   系统启动时会拉起persistent的(系统)app
  
  try {
    List apps = AppGlobals.getPackageManager().
        getPersistentApplications(STOCK_PM_FLAGS);
    if (apps != null) {
        int N = apps.size();
        int i;
        for (i=0; i<N; i++) {
            ApplicationInfo info
                = (ApplicationInfo)apps.get(i);
            if (info != null &&
                    !info.packageName.equals("android")) {
                addAppLocked(info, false, null /* ABI override */);
            }
        }
    }
} catch (RemoteException ex) {
    // pm is in same process, this will never happen.
}

final ProcessRecord addAppLocked(ApplicationInfo info, boolean isolated,
            String abiOverride) {

        if (app.thread == null && mPersistentStartingProcesses.indexOf(app) < 0) {
            mPersistentStartingProcesses.add(app);
            startProcessLocked(app, "added application", app.processName, abiOverride,
                    null /* entryPoint */, null /* entryPointArgs */);
        }

        return app;
    }

  
   
    
   
  
  
   app进程死掉之后重启persistent app
  
  private final boolean cleanUpApplicationRecordLocked(ProcessRecord app,
        boolean restarting, boolean allowRestart, int index) {

    if (!app.persistent || app.isolated) {

    } else if (!app.removed) {
        // This app is persistent, so we need to keep its record around.
        // If it is not already on the pending app list, add it there
        // and start a new process for it.
        if (mPersistentStartingProcesses.indexOf(app) < 0) {
            mPersistentStartingProcesses.add(app);
            restart = true;
        }
    }

    if (restart && !app.isolated) {
        // We have components that still need to be running in the
        // process, so re-launch it.
        if (index < 0) {
            ProcessList.remove(app.pid);
        }
        addProcessNameLocked(app);
        startProcessLocked(app, "restart", app.processName);
        return true;
    } else if (app.pid >; 0 && app.pid != MY_PID) {

    }
    return false;
}
  
   
    
   
  
  
   非系统app标志位失效
  
  if ((flags&PARSE_IS_SYSTEM) != 0) {
    if (sa.getBoolean(
            com.android.internal.R.styleable.AndroidManifestApplication_persistent,
            false)) {
        ai.flags |= ApplicationInfo.FLAG_PERSISTENT;
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
 


