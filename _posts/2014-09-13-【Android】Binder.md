---
layout: post
title: 【Android】Binder
date: 2014-09-13 18:57:00
categories: [Android]
tags: []
---
**BINDER_WRITE_READ** sends zero or more Binder operations, then blocks waiting to receive incoming operations and return
 with a result. (This is the same as doing a normal write() followed by a read() on the file descriptor, just a little more efficient.)
**BINDER_SET_WAKEUP_TIME** sets the time at which the next user-space event is scheduled to happen in the calling process.
**BINDER_SET_IDLE_TIMEOUT** sets the time threads will remain idle (waiting for a new incoming transaction) before they time out.
**BINDER_SET_REPLY_TIMEOUT** sets the time threads will block waiting for a reply until they time out.
**BINDER_SET_MAX_THREADS** sets the maximum number of threads that the driver is allowed to create for that process's thread pool.





