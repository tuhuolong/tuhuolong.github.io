---
layout: post
title: "Windows Shell命令"
date: 2011-03-02 10:26:00 
comments: true
categories: [windows]
tags: [windows]
description: "Windows Shell命令"
keywords: windows
---


 
  一、Windows下Shell命令概述
 
 
  
   Windows的Shell命令又是Windows的CMD命令。而cmd命令又是原来MS-DOS系统保留下来。
  
  
   Shell来源路径与配置
  
  
   对应的命令资源配置在windows的环境变量中：
  
  
   Windows Shell命令是基于配置好的Path环境变量，对Shell命令在Path路径中依次从前至后搜寻到对应命名的可执行入口。
  
  
   也就是可以自行编写一些Shell程序，C、Java、Perl等等，然后配置到环境变量中，就可以有自己的私有Shell命令了。大多数默认的
Shell命令对应目标程序打多在“C:/Windows/”及“C:/Windows/System32”目录下。而基于Windows的一些应用程序
（如Mysql）都会向Windows的Path中配置添加自己的目录。
  
  
   Shell命令的执行方式
  
  
   
    一般的是通过调出CMD控制台执行
   
   
    对于较熟悉的可以自行编写bat批处理Shell命令，然后保存为 .bat 后缀格式文件。
   
   
    以Win7为例，可以通过Win键调出search 窗口，快捷执行，但不能全部使用
   
   
    同时可以通过Win+R调出运行窗口来执行
   
  
 
 
  二、常用Shell程序命令大全
 
 
  
   下面分类列举那些调出Shell命令列表，通过使用对应命令可以大大节约时间：
  
  
   Windows常用工具类
  
  
   
    
     
      
       
        运行程序
       
      
      
       
        运行命令
       
      
     
     
      
       记事本
      
      
       notepad
      
     
     
      
       计算器
      
      
       calc
      
     
     
      
       画图
      
      
       mspaint
      
     
     
      
       写字板
      
      
       write
      
     
     
      
       Windows放大镜
      
      
       magnify
      
     
     
      
       辅助工具管理器
      
      
       utilman
      
     
     
      
       Telnet客户端
      
      
       telnet
      
     
     
      
       任务管理器
      
      
       taskmgr
      
     
     
      
       关闭Windows
      
      
       shutdown
      
     
     
      
       扫描仪与相机
      
      
       sticpl.cpl
      
     
     
      
       远程桌面
      
      
       mstsc
      
     
     
      
       可移动存储
      
      
       ntmsmgr.msc
      
     
     
      
       注册表编辑器
      
      
       regedit
      
     
     
      
       TrueType造字程序
      
      
       eudcedit
      
     
     
      
       打印机文件夹
      
      
       printers
      
     
     
      
       打印机和传真
      
      
       control printers
      
     
     
      
       控制面板
      
      
       control
      
     
     
      
       屏幕键盘
      
      
       osk
      
     
     
      
       网络连接
      
      
       ncpa.cpl 或control netconnections
      
     
     
      
       从Windows注销
      
      
       logoff
      
     
     
      
       字体文件夹
      
      
       fonts 或control fonts
      
     
     
      
       磁盘管理
      
      
       diskmgmt.msc
      
     
     
      
       快速查找
      
      
       findfast.cpl
      
     
     
      
       磁盘碎片整理程序
      
      
       dfrg.msc
      
     
     
      
       磁盘检查工具
      
      
       chkdsk
      
     
     
      
       磁盘清理工具
      
      
       cleanmgr
      
     
     
      
       命令行提示符
      
      
       cmd
      
     
     
      
       剪贴簿查看器
      
      
       clipbrd
      
     
     
      
       辅助功能选项
      
      
       access.cpl
      
     
     
      
       添加硬件向导
      
      
       hdwwiz.cpl
      
     
     
      
       添加或删除程序
      
      
       appwiz.cpl
      
     
    
   
  
  
   常用管理配置工具
  
  
   
    
     
      
       设备管理器
      
      
       devmgmt.msc
      
     
     
      
       Direct X控制面板(如果已经安装)
      
      
       directx.cpl
      
     
     
      
       Direct X诊断工具
      
      
       dxdiag
      
     
     
      
       磁盘分区管理器
      
      
       diskpart
      
     
     
      
       显示属性
      
      
       desk.cpl 或 control desktop
      
     
     
      
       文件夹选项
      
      
       control folders
      
     
     
      
       显示属性的外观选项卡
      
      
       control color
      
     
     
      
       文件签名验证
      
      
       sigverif
      
     
     
      
       IP配置实用程序
      
      
       ipconfig
      
     
     
      
       键盘属性
      
      
       control keyboard
      
     
     
      
       鼠标属性
      
      
       main.cpl 或 control mouse
      
     
     
      
       Internet属性
      
      
       inetcpl.cpl
      
     
     
      
       密码属性
      
      
       assword.cpl
      
     
     
      
       电话与调制解调器选项
      
      
       telephon.cpl
      
     
     
      
       电源选项属性
      
      
       powercfg.cpl
      
     
     
      
       性能
      
      
       perfmon
      
     
     
      
       区域和语言选项
      
      
       intl.cpl
      
     
     
      
       系统属性
      
      
       sysdm.cpl
      
     
     
      
       用户帐户管理
      
      
       nusrmgr.cpl
      
     
     
      
       Windows防火墙
      
      
       firewall.cpl
      
     
     
      
       运行Windows更新
      
      
       wupdmgr
      
     
     
      
       系统配置实用程序
      
      
       msconfig
      
     
     
      
       声音和音频设备属性
      
      
       mmsys.cpl
      
     
     
      
       共享文件夹
      
      
       fsmgmt.msc
      
     
     
      
       服务
      
      
       services.msc
      
     
     
      
       Windows安全中心
      
      
       wscui.cpl
      
     
     
      
       任务计划
      
      
       control schedtasks
      
     
     
      
       游戏控制
      
      
       joy.cpl
      
     
     
      
       字符映射表
      
      
       charmap
      
     
     
      
       证书管理控制台
      
      
       certmgr.msc
      
     
    
   
  
  
   一些高级配置管理
  
  
   
    
     
      
       Windows管理体系结构
      
      
       wmimgmt.msc
      
     
     
      
       Windows系统安全工具
      
      
       syskey
      
     
     
      
       系统配置编辑器
      
      
       sysedit
      
     
     
      
       策略的结果集
      
      
       rsop.msc
      
     
     
      
       可移动存储
      
      
       ntmsmgr.msc
      
     
     
      
       可移动存储操作请求
      
      
       ntmsoprq.msc
      
     
     
      
       ODBC数据源管理器
      
      
       odbccp32.cpl
      
     
     
      
       本地安全设置
      
      
       secpol.msc
      
     
     
      
       本地用户和组
      
      
       lusrmgr.msc
      
     
     
      
       索引服务
      
      
       ciadv.msc
      
     
     
      
       组策略编辑器
      
      
       gpedit.msc
      
     
     
      
       事件查看器
      
      
       eventvwr.msc
      
     
     
      
       Driver Verifier Manager
      
      
       verifier
      
     
     
      
       Dr. Watson系统诊断工具
      
      
       drwtsn32
      
     
    
   
  
 
 
  三、一些Shell程序命令详解
 
 
  下面围绕第二章节以及更多高级shell操作命令进行详细描述：(陆续根据使用补充)
 
 
  网络配置Ipconfig命令
 
 
  
   默认Ipconfig为显示当前网络基本连接信息
  
  
   
    显示连接配置    ipconfig /all
   
   
    显示DNS缓存内容    ipconfig /displaydns
   
   
    去除DNS缓存内容    ipconfig /flushdns
   
   
    释放全部(或指定)适配器的由DHCP分配的动态IP地址)    ipconfig /release
   
   
    为全部适配器重新分配IP地址    ipconfig /renew
   
   
    刷新DHCP并重新注册DNS   ipconfig /registerdns
   
   
    显示DHCP Class ID   ipconfig /showclassid
   
   
    修改DHCP Class ID    ipconfig /setclassid
   
  
 
 
  关机操作shutdown命令
 
 
  
   Shutdown使您能够一次关闭或重新启动一台本地或远程计算机：
  
  
   语法:：
   
   shutdown [/i | /l | /s | /r | /a | /p | /h | /e] [/f]
 [/m //computername/] [/t XXX] [/d [p:]XX:YY/c"Comment"]
  
  
   参数：
  
  
   
    /i : 显示“远程关机对话框”。/i 选项必须是键入的第一个参数，之后的所有参数都将被忽略。
   
   
    /l : 立即注销当前用户，没有超时期限。不能将 /l 与 /m //computername/ 或 /t 一起使用。
   
   
    /s : 关闭计算机。
   
   
    /r : 关机后重新启动计算机。
   
   
    /a : 取消关机操作（仅在超时期限内有效）。a 参数仅可以与 /m //computername/ 一起使用。
   
   
    /p : 仅关闭本地计算机（而不是远程计算机），没有超时期或警告。/p 只能和 /d 一起使用。如果您的计算机不支持关闭电源功能，使用 
/p 将关闭系统但不会切断您的计算机电源。
   
   
    /h : 使本地计算机处于休眠状态（如果已启用休眠）。仅可将 h 与 /f 一起使用。
   
   
    /e : 允许在目标计算机上记录意外关机原因。
   
   
    /f : 强制关闭正在运行的应用程序而不提前警告用户。
   
   
    /t：设置经过过长时间进行当次操作，定时功能，时间单位为秒
   
  
  
   举例：
  
  
   
    立即关机：shutdown -s -t 0
   
   
    30秒后重启：shutdown -r -t 30
   
   
    取消关机：shutdown -a
   
  
 
 
  网络连接查看命令netstat
 
 
  
   
    netstat -a 查看开启了哪些端口,常用netstat -an
   
   
    netstat -n 查看端口的网络连接情况，常用netstat -an
   
   
    netstat -v 查看正在进行的工作
   
   
    netstat -p 协议名 例：netstat -p tcq/ip 查看某协议使用情况（查看tcp/ip协议使用情况）
   
   
    netstat -s 查看正在使用的所有协议使用情况
   
  
 
 
  系统网络服务命令net
 
 
  
   
    netstat -a 查看开启了哪些端口,常用netstat -an
   
   
    net use //ip/ipc$ " " /user:" " 建立IPC空链接
   
   
    net use //ip/ipc$ "密码" /user:"用户名" 建立IPC非空链接
   
   
    net use h: //ip/c$ "密码" /user:"用户名" 直接登陆后映射对方C：到本地为H:
   
   
    net use h: //ip/c$ 登陆后映射对方C：到本地为H:
   
   
    net use //ip/ipc$ /del 删除IPC链接
   
   
    net use h: /del 删除映射对方到本地的为H:的映射
   
   
    net user 用户名 密码 /add 建立用户
   
   
    net user guest /active:yes 激活guest用户
   
   
    net user 查看有哪些用户
   
   
    net user 帐户名 查看帐户的属性
   
   
    net localgroup administrators 用户名 /add 
把"用户"添加到管理员中使其具有管理员权限,注意：administrator后加s用复数
   
   
    net start 查看开启了哪些服务
   
   
    net start 服务名 开启服务；(如:net start telnet， net start schedule)
   
   
    net stop 服务名 停止某服务
   
   
    net time //目标ip 查看对方时间
   
   
    net time //目标ip /set 设置本地计算机时间与"目标IP"主机的时间同步,加上参数/yes可取消确认信息
   
   
    net view 查看本地局域网内开启了哪些共享
   
   
    net view //ip 查看对方局域网内开启了哪些共享
   
   
    net config 显示系统网络设置
   
   
    net logoff 断开连接的共享
   
   
    net pause 服务名 暂停某服务
   
   
    net send ip "文本信息" 向对方发信息
   
   
    net ver 局域网内正在使用的网络连接类型和信息
   
   
    net share 查看本地开启的共享
   
   
    net share ipc$ 开启ipc$共享
   
   
    net share ipc$ /del 删除ipc$共享
   
   
    net share c$ /del 删除C：共享
   
   
    net user guest 12345 用guest用户登陆后用将密码改为12345
   
   
    net password 密码 更改系统登陆密码
   
  
 
 
  系统服务命令sc
 
 
  
   SC来控制系统创建服务和管理服务。
  
  
   SC使用这样的语法：
  
  
   
    sc <server>; [command] [service name] 
<option1>; <option2>;…
   
  
  
   下面介绍各种参数。
  
  
   
    server ：可选，可以使用双斜线，如
    
     myserver
    
    ，也可以是
    
     192.168.0.1
    
    来操作远程计算机。如
果在本地计算机上操作就不用添加任何参数。
   
   
    command：下面列出SC可以使用的命令
    
     
      config—-改变一个服务的配置。（长久的）
     
     
      continue–对一个服务送出一个继续控制的要求。
     
     
      control—-对一个服务送出一个控制。
     
     
      create—-创建一个服务。（增加到注册表中）
     
     
      delete—-删除一个服务。（从注册表中删除）
     
     
      EnumDepend–列举服务的从属关系。
     
     
      GetDisplayName–获得一个服务的显示名称。
     
     
      GetKeyName–获得一个服务的服务键名。
     
     
      interrogate–对一个服务送出一个询问控制要求。
     
     
      pause—-对一个服务送出一个暂停控制要求。
     
     
      qc—-询问一个服务的配置。
     
     
      query—-询问一个服务的状态，也可以列举服务的状态类型。
     
     
      start—-启动一个服务。
     
     
      stop—-对一个服务送出一个停止的要求。
     
    
   
   
    service name：在注册表中为service key制定的名称。注意这个名称是不同于显示名称的（这个名称可以用net 
start和服务控制面板看到），而SC是使用服务键名来鉴别服务的。
   
  
  
   
    特别说明
   
   ：
   
    SC create
   
   子
命令
   
   这个命令可以在注册表和服务控制管理数据库建立一个入口。使用语法如下：
  
  
   
    sc <server>; create [service name] [binPath= ] 
<option1>; <option2>;…
   
  
  
   这里的servername，servicename，optionname，optionvalues和上面的一样，这里就option描述，通
常为键值对应：option name = option values 。
  
  
   
    注：键值对应 name= 
value，等号后面应由一个空格，不能省略
   
  
  
   
    option
   
   详细描述如下：
  
  
   
    type=—-own, share, interact, kernel, filesys
    
    关于建立服务的类型，
选项值包括驱动程序使用的类型，默认是share。
   
   
    start=—-boot, sys tem, auto, demand, disabled
    
    关于启动服务的类
型，选项值包括驱动程序使用的类型，默认是demand（手动）。
   
   
    error=—-normal, severe, critical, ignore
    
    当服务在导入失败错误的严重
性，默认是normal。
   
   
    binPath=–(string)
    
    服务二进制文件的路径名，这里没有默认值，这个字符串是必须设置的。
   
   
    group=—-(string)
    
    这个服务属于的组，这个组的列表保存在注册表中的
ServiceGroupOrder下。默认是nothing。
   
   
    tag=—-(string)
    
    如果这个字符串被设置为yes，sc可以从CreateService 
call中得到一个tagId。然而，SC并不显示这个标签，所以使用这个没有多少意义。默认是nothing
   
   
    depend=—-(space separated string)有空格的字符串。
    
    在这个服务启动前必须启动的
服务的名称或者是组。
   
   
    obj=—-(string)
    
    账号运行使用的名称，也可以说是登陆身份。默认是localsys tem
   
   
    Displayname=–(string)
    
    一个为在用户界面程序中鉴别各个服务使用的字符串。
   
   
    password=–(string)
    
    一个密码，如果一个不同于localsys 
tem的账号使用时需要使用这个。
   
   
    Optionvalues
    
    Optionname参数名称的数值列表。参考optionname。当我们输入一个字符
串时，如果输入一个空的引用这意味着一个空的字符串将被导入。
   
  
  
   举例说明：在一台叫做（
   
    myserver
   
   ）的计算机上为一个叫“NewService”的服务建立的一个注册表登记，命令如下：
  
  
   sc
   
    myserver
   
   create MyService binpath= C:/NewServ.exe
  
 


