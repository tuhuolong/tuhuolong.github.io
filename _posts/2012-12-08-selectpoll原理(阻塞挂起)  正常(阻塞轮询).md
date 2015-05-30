---
layout: post
title: select/poll原理(阻塞挂起)  正常(阻塞轮询)
date: 2012-12-08 19:41:00
categories: [Linux内核]
tags: []
---
遍历所有的fd, 如果没好(不就绪)，加入(对应fd)等待队列,如果所有fd都没好(不就绪)，挂起线程(schedule_timeout)
 直到就绪唤醒或超时,再次遍历并退出
static int do_poll(unsigned int nfds, struct poll_list *list, struct poll_wqueues *wait,
 long timeout)                    
 {                                                                       
      int count = 0;
      poll_table* pt = &wait->pt;
  
      if (!timeout)
          pt = NULL;
  
      for (;;) {
          struct poll_list *walk;
          set_current_state(TASK_INTERRUPTIBLE);
          walk = list;
          while(walk != NULL) {
              do_pollfd( walk->len, walk->entries, &pt, &count);
              walk = walk->next;
          }
          pt = NULL;
          if (count || !timeout || signal_pending(current))
              break;
          count = wait->error;
          if (count)
              break;
          timeout = schedule_timeout(timeout); /* 让current挂起，不占CPU，timeout到了以后再回来运行current*/
      }
      __set_current_state(TASK_RUNNING);
      return count;
  }


