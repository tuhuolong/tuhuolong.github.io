---
layout: post
title: "android app 默认的crash处理流程  (FATAL EXCEPTION)"
date: 2016-07-12 20:15:00 
comments: true
categories: []
tags: []
description: "android app 默认的crash处理流程  (FATAL EXCEPTION)"
keywords: 
---


 
  private static class UncaughtHandler implements Thread.UncaughtExceptionHandler {
    public void uncaughtException(Thread t, Throwable e) {
        try {
            // Don't re-enter -- avoid infinite loops if crash-reporting crashes.
            if (mCrashing) return;
            mCrashing = true;

            if (mApplicationObject == null) {
                Clog_e(TAG, "*** FATAL EXCEPTION IN SYSTEM PROCESS: " + t.getName(), e);
            } else {
                StringBuilder message = new StringBuilder();
                message.append("FATAL EXCEPTION: ").append(t.getName()).append("\n");
                final String processName = ActivityThread.currentProcessName();
                if (processName != null) {
                    message.append("Process: ").append(processName).append(", ");
                }
                message.append("PID: ").append(Process.myPid());
                Clog_e(TAG, message.toString(), e);
            }

            // Bring up crash dialog, wait for it to be dismissed
            ActivityManagerNative.getDefault().handleApplicationCrash(
                    mApplicationObject, new ApplicationErrorReport.CrashInfo(e));
        } catch (Throwable t2) {
            try {
                Clog_e(TAG, "Error reporting crash", t2);
            } catch (Throwable t3) {
                // Even Clog_e() fails!  Oh well.
            }
        } finally {
            // Try everything to make sure this process goes away.
            Process.killProcess(Process.myPid());
            System.exit(10);
        }
    }
}
  AMP.handleApplicationCrash
    AMS.handleApplicationCrash
        AMS.findAppProcess
        AMS.handleApplicationCrashInner
            AMS.addErrorToDropBox
            AMS.crashApplication
                AMS.makeAppCrashingLocked
                    AMS.startAppProblemLocked
                    ProcessRecord.stopFreezingAllLocked
                        ActivityRecord.stopFreezingScreenLocked
                            WMS.stopFreezingScreenLocked
                                WMS.stopFreezingDisplayLocked
                    AMS.handleAppCrashLocked
                mUiHandler.sendMessage(SHOW_ERROR_MSG)
  
   
    *** FATAL EXCEPTION IN SYSTEM PROCESS
   
  
  
   
    FATAL EXCEPTION
   
  
 
 
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
 


