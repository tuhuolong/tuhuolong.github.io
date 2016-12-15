---
layout: post
title: "android CrashHandler"
date: 2016-12-15 16:18:00 
comments: true
categories: []
tags: []
description: "android CrashHandler"
keywords: 
---


 
  Thread.setDefaultUncaughtExceptionHandler(new CrashHandler(getAppContext()));
  public class CrashHandler implements Thread.UncaughtExceptionHandler {

    public static final int MAX_CRASH_LOG_LENGTH = 5000;

    private Context mAppContext;
    private Thread.UncaughtExceptionHandler mDefaultHandler;

    public CrashHandler(Context appContext) {
        mAppContext = appContext;
        mDefaultHandler = Thread.getDefaultUncaughtExceptionHandler();
    }

    @Override
    public void uncaughtException(Thread thread, Throwable ex) {
        if (ex == null) {
            return;
        }
        ex.printStackTrace();
        Writer result = new StringWriter();
        PrintWriter printWriter = new PrintWriter(result);
        ex.printStackTrace(printWriter);
        String callStack = result.toString();
        if (callStack.length() >; MAX_CRASH_LOG_LENGTH) {
            callStack = callStack.substring(0, MAX_CRASH_LOG_LENGTH);
        }
        String exceptionClass = (ex == null) ? "" : ex.getStackTrace()[0].getClassName();
        String exceptionMethod = (ex == null) ? "" : ex.getStackTrace()[0].getMethodName();

        final List<KeyValuePair>; params = new ArrayList<>;();

        final Object syncLock = new Object();

        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                okhttp3.Request okRequest = new okhttp3.Request.Builder()
                        .url(url)
                        .post(KeyValuePairUtil.getRequestBody(params))
                        .build();

                Call call = getOkHttpClient().newCall(okRequest);

                call.enqueue(new Callback() {
                    @Override
                    public void onFailure(Call call, IOException e) {
                        synchronized (syncLock) {
                            syncLock.notify();
                        }
                    }

                    @Override
                    public void onResponse(Call call, Response response) throws IOException {
                        synchronized (syncLock) {
                            syncLock.notify();
                        }
                    }
                });
            }
        };

        if (Looper.myLooper() == null) {
            mUiHandler.post(runnable);
        } else {
            runnable.run();
        }

        synchronized (syncLock) {
            try {
                syncLock.wait();
            } catch (InterruptedException e) {
            }
        }

        if (mDefaultHandler != null) {
            mDefaultHandler.uncaughtException(thread, ex);
        }
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
 


