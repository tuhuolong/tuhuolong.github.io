---
layout: post
title: "android系统账户管理(AccountManagerService+AccountAuthenticator)"
date: 2015-04-09 20:33:00 
comments: true
categories: []
tags: []
description: "android系统账户管理(AccountManagerService+AccountAuthenticator)"
keywords: 
---


 
  
   
    避免多次登录
   
  
  
   AccountManagerService
  
  
   
    
     管理Authenticator
    
   
  
  
   AccountAuthenticator(账户认证器)
  
  
   
    
     认证
    
   
   
    
     授权(AuthToken)
    
   
  
  <service
    android:name=".AccountAuthenticatorService"
    android:exported="true" >;
    <intent-filter>;
        <action android:name="android.accounts.AccountAuthenticator" />;
    </intent-filter>;

    <meta-data
        android:name="android.accounts.AccountAuthenticator"
        android:resource="@xml/authenticator_config" />;
</service>;
  public class AccountAuthenticator extends AbstractAccountAuthenticator {

}
  <account-authenticator xmlns:android="http://schemas.android.com/apk/res/android"
    android:accountType="@string/account_type"
    android:icon="@drawable/account_icon"
    android:label="@string/account_name"
    android:smallIcon="@drawable/account_small_icon" />;
 
 
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
 


