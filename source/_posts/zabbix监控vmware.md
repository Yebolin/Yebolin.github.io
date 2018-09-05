title: zabbix监控vmware
categories: 运维
tags:
  - 运维
  - 虚拟化
  - vmware
---

zabbix监控vmware监控，实际环境中使用的是vcenter6.5

1. 系统有vcenter，跟几台esxi6.0/6.5的host，有超额分配内存（一般最多比例130%），而且磁盘使用的是thin，监控不完善的情况下，可能引起内存或者磁盘满的情况下，所以需要加强监控。
主要用于vcetner下面的host的监控，vms的监控，因为基本每个vm下面都单独安装了zabbix agent，就不需要单独监控

虚拟机监控分两个步骤完成。首先，Zabbix 是通过 vmware collector 进程来监控虚拟机。这些进程通过 SOAP 协议从 VMware Web 服务获取必要的信息，对其进行预处理并存储到 Zabbix server 共享内存中。然后，zabbix pollers 通过 zabbix 简单检查 VMware keys 来检索这些数据。

2. 官方模板
首先分析一下官方的模板，是否符合我们的预期
  - Template Virt VMware 模板应用于 VMware vCenter 和 ESX hypervisor 监控。 用于发现集群、hosts、vms。
  - Template Virt VMware Hypervisor 由自动发现使用，用于host。监控项目有cpu内存，存储自动发现
  - Template Virt VMware Guest 由自动发现使用，用于vm

3. 模板没有任何的触发器，需要自己添加

[虚拟机监控zabbix官方文档](https://www.zabbix.com/documentation/3.0/manual/vm_monitoring)

# 实施步骤

1. 修改zabbix_server.conf
```
StartVMwareCollectors=2
VMwareCacheSize=8M
VMwareTimeout=30
```

2. 在vcenter中创建一个全局只读账号

3. 创建一个新的zabbix主机
  - 关联模板 Template Virt VMware（只需要关联这个，有LLD发现hosts跟vms）
  - macros:
	- {$URL} - VMware 服务 (vCenter or ESX hypervisor) SDK URL (https://servername/sdk).
	- {$USERNAME} - VMware 服务用户名
	- {$PASSWORD} - VMware 服务{$ USERNAME}用户密码
	
解决问题：
  1. 完善触发器
    - Free space percentage < 10
    - Overall status <> green(1)
    - Used memory/Total memory > 90%
  2. log没有级别，现在每分钟检查一次，用户登陆就会产生一条登陆的log，引起大量的日志