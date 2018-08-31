
title: zabbix template 3.2 转换成 3.0
categories:
  - 运维
tags:
  - 运维
  - zabbix
  - hardware

---
修改zabbix3.2的模板为3.0的模板，导入到zabbix里面

1. zabbix share里面有一个简单的hp服务器的ilo4的snmp模板，但支持的版本为3.2，现在系统使用的3.0的版本，由于增加了一些属性，引起无法导入，所以做了一些修改，然后导入到系统里面

2. 原版地址：
[https://share.zabbix.com/cat-server-hardware/hp/hp-ilo4-snmp-agentless-simple-template](https://share.zabbix.com/cat-server-hardware/hp/hp-ilo4-snmp-agentless-simple-template)
对应的github地址：
[https://github.com/wisenetman/Zabbix-Templates](https://github.com/wisenetman/Zabbix-Templates)

3. 修改步骤

修改版本号 `<version>3.2</version>`修改为`<version>3.0</version>`

去掉不支持的标签：
```xml
<httptest/>
<recovery_expression>*</recovery_expression>
<recovery_mode>*</recovery_mode>
<correlation_mode>*</correlation_mode>
<correlation_tag/>
<manual_close>*</manual_close>
<tags/>
```

4. 修改后的版本：
[https://github.com/Yebolin/Zabbix-Templates](https://github.com/Yebolin/Zabbix-Templates)
提交了PR，第一次提交PR，不过作者可能没有继续维护了，很久添加其他新的内容了

5. 修改其他模板，可以参考上面步骤，另外有一个python的zabbix-template-converter的项目，不过我转换后，依然没有导入成功，只能手动修改了