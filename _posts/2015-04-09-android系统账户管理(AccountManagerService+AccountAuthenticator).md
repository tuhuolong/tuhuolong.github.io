---
layout: post
title: android系统账户管理(AccountManagerService+AccountAuthenticator)
date: 2015-04-09 20:33:00
categories: [Android]
tags: []
---
#####`避免多次登录`
####AccountManagerService
- `管理Authenticator`

####AccountAuthenticator(账户认证器)
- `认证`
- `授权(AuthToken)`

	<serviceandroid:name=".AccountAuthenticatorService"android:exported="true" >
	    <intent-filter>
	        <actionandroid:name="android.accounts.AccountAuthenticator" />
	    </intent-filter>
	
	    <meta-dataandroid:name="android.accounts.AccountAuthenticator"android:resource="@xml/authenticator_config" />
	</service>
	public classAccountAuthenticatorextendsAbstractAccountAuthenticator {
	
	}
	<account-authenticator xmlns:android="http://schemas.android.com/apk/res/android"
	    android:accountType="@string/account_type"
	    android:icon="@drawable/account_icon"
	    android:label="@string/account_name"
	    android:smallIcon="@drawable/account_small_icon" />
