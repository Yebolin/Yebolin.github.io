title: 在esxi主机上通过hponcfg重置ilo密码
categories:
  - 运维
tabs:
  - 运维
  - hardware

---

想重置hp远程管理口ilo的密码，一般情况下重启服务器就可以重置密码，但如果不想重启服务器，可以使用hponcfg工具来重置密码。
该方法只适用于安装了esxi的hp服务器。其他版本系统的可以参考安装相关工具来解决。

1. 打开esxi的ssh服务
	具体步骤不做详细描述，大概步骤参考:
  - c#客户端: 配置->软件->安全配置文件->服务->属性->启动ssh服务
  - webUI: 配置->系统->服务->ssh启动

2. ssh到esxi主机上
使用hp定制版本的esxi，都带有这个软件，没有的话需要去官网下载
```bash
cd /opt/hp/tools/
cat >pw.xml<<EOF
<RIBCL VERSION="2.0">
<LOGIN USER_LOGIN="Administrator" PASSWORD="unknown">
<USER_INFO MODE="write">
<MOD_USER USER_LOGIN="Administrator">
<PASSWORD value="password"/>
</MOD_USER>
</USER_INFO>
</LOGIN>
</RIBCL>
EOF
./hponcfg -f pw.xml
``` 

看到如下结果表示成功
> HP Lights-Out Online Configuration utility

> Version 4.0-13 (c) Hewlett-Packard Company, 2011
Firmware Revision = 2.10 Device type = iLO 4 Driver name = hpilo
iLO IP Address: 172.\*.\*.\* 
Script succeeded 

3. 关闭ssh服务
出于安全考虑，重置密码后关闭服务。
