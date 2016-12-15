---
layout: post
title: "android图层划分(PhoneWindowManager.windowTypeToLayerLw) 图层id"
date: 2014-11-25 15:12:00 
comments: true
categories: []
tags: []
description: "android图层划分(PhoneWindowManager.windowTypeToLayerLw) 图层id"
keywords: 
---


 
 
 
  
   
    public
   
   
    int
   
  
  windowTypeToLayerLw(
  
   
    int
   
  
  type) {
 
 
  
   
    if
   
  
  (type >;= FIRST_APPLICATION_WINDOW&& type <= LAST_APPLICATION_WINDOW) {
 
 
  
   
    return
   
  
  
   2;
  
 
 
  }
 
 
  
   
    switch
   
  
  (type) {
 
 
  
   
    case
   
  
  TYPE_UNIVERSE_BACKGROUND:
 
 
  
   
    return
   
  
  
   1;
  
 
 
  
   
    case
   
  
  TYPE_PRIVATE_PRESENTATION:
 
 
  
   
    return
   
  
  
   2;
  
 
 
  
   
    case
   
  
  TYPE_WALLPAPER:
 
 
  
   //
   
    wallpaper
   
   is at the bottom, though the window manager may move it.
  
 
 
  
   
    return
   
  
  
   2;
  
 
 
  
   
    case
   
  
  TYPE_PHONE:
 
 
  
   
    return
   
  
  
   3;
  
 
 
  
   
    case
   
  
  TYPE_SEARCH_BAR:
 
 
  
   
    return
   
  
  
   4;
  
 
 
  
   
    case
   
  
  TYPE_RECENTS_OVERLAY:
 
 
  
   
    case
   
  
  TYPE_SYSTEM_DIALOG:
 
 
  
   
    return
   
  
  
   5;
  
 
 
  
   
    case
   
  
  TYPE_TOAST:
 
 
  
   // toasts andthe plugged-in battery thing
  
 
 
  
   
    return
   
  
  
   6;
  
 
 
  
   
    case
   
  
  TYPE_PRIORITY_PHONE:
 
 
  
   // SIM errorsand unlock.  Not sure if this reallyshould be in a high layer.
  
 
 
  
   
    return
   
  
  
   7;
  
 
 
  
   
    case
   
  
  TYPE_DREAM:
 
 
  
   // used forDreams (
   
    screensavers
   
   with TYPE_DREAM windows)
  
 
 
  
   
    return
   
  
  
   8;
  
 
 
  
   
    case
   
  
  TYPE_SYSTEM_ALERT:
 
 
  
   // like the ANR/
   
    app
   
   crashed dialogs
  
 
 
  
   
    return
   
  
  
   9;
  
 
 
  
   
    case
   
  
  TYPE_INPUT_METHOD:
 
 
  
   // on-screenkeyboards and other such input method user interfaces go here.
  
 
 
  
   
    return
   
  
  
   10;
  
 
 
  
   
    case
   
  
  TYPE_INPUT_METHOD_DIALOG:
 
 
  
   // on-screenkeyboards and other such input method user interfaces go here.
  
 
 
  
   
    return
   
  
  
   11;
  
 
 
  
   
    case
   
  
  TYPE_KEYGUARD_SCRIM:
 
 
  
   // the safetywindow that shows behind
   
    keyguard
   
   while
   
    keyguard
   
   is starting
  
 
 
  
   
    return
   
  
  
   12;
  
 
 
  
   
    case
   
  
  TYPE_KEYGUARD:
 
 
  
   // the
   
    keyguard
   
   ;nothing on top of these can take focus, since they are
  
 
 
  
   // responsiblefor power management when displayed.
  
 
 
  
   
    return
   
  
  
   13;
  
 
 
  
   
    case
   
  
  TYPE_KEYGUARD_DIALOG:
 
 
  
   
    return
   
  
  
   14;
  
 
 
  
   
    case
   
  
  TYPE_STATUS_BAR_SUB_PANEL:
 
 
  
   
    return
   
  
  
   15;
  
 
 
  
   
    case
   
  
  TYPE_STATUS_BAR:
 
 
  
   
    return
   
  
  
   16;
  
 
 
  
   
    case
   
  
  TYPE_STATUS_BAR_PANEL:
 
 
  
   
    return
   
  
  
   17;
  
 
 
  
   
    case
   
  
  TYPE_VOLUME_OVERLAY:
 
 
  
   // theon-screen volume indicator and controller shown when the user
  
 
 
  
   // changes thedevice volume
  
 
 
  
   
    return
   
  
  
   18;
  
 
 
  
   
    case
   
  
  TYPE_SYSTEM_OVERLAY:
 
 
  
   // theon-screen volume indicator and controller shown when the user
  
 
 
  
   // changes thedevice volume
  
 
 
  
   
    return
   
  
  
   19;
  
 
 
  
   
    case
   
  
  TYPE_NAVIGATION_BAR:
 
 
  
   // thenavigation bar, if available, shows atop most things
  
 
 
  
   
    return
   
  
  
   20;
  
 
 
  
   
    case
   
  
  TYPE_NAVIGATION_BAR_PANEL:
 
 
  
   // some panels(e.g. search) need to show on top of the navigation bar
  
 
 
  
   
    return
   
  
  
   21;
  
 
 
  
   
    case
   
  
  TYPE_SYSTEM_ERROR:
 
 
  
   // system-levelerror dialogs
  
 
 
  
   
    return
   
  
  
   22;
  
 
 
  
   
    case
   
  
  TYPE_MAGNIFICATION_OVERLAY:
 
 
  
   // used tohighlight the magnified portion of a display
  
 
 
  
   
    return
   
  
  
   23;
  
 
 
  
   
    case
   
  
  TYPE_DISPLAY_OVERLAY:
 
 
  
   // used tosimulate secondary display devices
  
 
 
  
   
    return
   
  
  
   24;
  
 
 
  
   
    case
   
  
  TYPE_DRAG:
 
 
  
   // the draglayer: input for drag-and-drop is associated with this window,
  
 
 
  
   // which sitsabove all other focusable windows
  
 
 
  
   
    return
   
  
  
   25;
  
 
 
  
   
    case
   
  
  TYPE_SECURE_SYSTEM_OVERLAY:
 
 
  
   
    return
   
  
  
   26;
  
 
 
  
   
    case
   
  
  TYPE_BOOT_PROGRESS:
 
 
  
   
    return
   
  
  
   27;
  
 
 
  
   
    case
   
  
  TYPE_POINTER:
 
 
  
   // the (mouse)pointer layer
  
 
 
  
   
    return
   
  
  
   28;
  
 
 
  
   
    case
   
  
  TYPE_HIDDEN_NAV_CONSUMER:
 
 
  
   
    return
   
  
  
   29;
  
 
 
  }
 
 
  Log.
  
   e
  
  (
  
   
    TAG
   
  
  ,
  
   "Unknown window type: "
  
  + type);
 
 
  
   
    return
   
  
  
   2;
  
 
 
  }
 
 
 


