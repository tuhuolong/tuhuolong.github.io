---
layout: post
title: Eclipse+Maven配置
date: 2013-01-31 19:51:00
categories: [Android]
tags: []
---
下载Maven,配置路径 
M2_HOME:E:\Program Files\apache-maven-3.0.4
PATH:%M2_HOME%\bin;....
MAVEN_OPTS:-Xms256m -Xmx512m


新建仓库文件
E:\Program Files\apache-maven-3.0.4\repo


修改Maven本地仓库 和JDK版本
conf\settings.xml 
<localRepository>E:\Program Files\apache-maven-3.0.4\repo</localRepository>
<profile>
      <id>jdk-1.6</id>
      <activation>
        <jdk>1.6</jdk>
      </activation>
 <properties>       
               <maven.compiler.source>1.6</maven.compiler.source>       
               <maven.compiler.target>1.6</maven.compiler.target>       
               <maven.compiler.compilerVersion>1.6</maven.compiler.compilerVersion>       
      </properties> 
</profile>



安装Eclipse的Maven插件
http://m2eclipse.sonatype.org/sites/m2e

设置(插件的)Maven路径
Eclipse->Window->Preferences->Maven->Installations->"Add" E:\Program Files\apache-maven-3.0.4
Eclipse->Window->Preferences->Maven->User Settings->User Settings->选择E:\Program Files\apache-maven-3.0.4\conf\settings.xml


让Eclipse使用JDK而不是JRE
Eclipse->Window->Preferences->Java->Installed JREs

下载svn http://sourceforge.net/projects/win32svn/?source=dlp，安装
svndiffcmd="E:\\Program Files\\Subversion\\bin\\svn" diff


pom.xml
<android.sdk.path>E:\\android-sdk-windows</android.sdk.path>


项目右键->Configure->Convert to Maven Project