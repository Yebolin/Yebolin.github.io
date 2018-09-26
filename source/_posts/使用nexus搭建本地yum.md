title: 使用nexus搭建本地yum源
categories: 运维
tags:
  - 运维
  
---

之前搭建的本地yum源，一直都是使用http服务器(nginx或apache)跟createrepo配合一起使用来搭建。但这个方式有几个问题：
  - 服务器端需要全量下载全部的rpm包，其实很多包不会经常使用到
  - 需要写脚本定时维护，有时候遇到版本更新的时候会很慢
  - 不能把多个yum源汇聚管理
  
想把本地yum作为代理方式访问，本地有请求本地，本地没有请求远端缓存到本地，发现nexus可以满足这个需求，而且可以聚合多个yum源（其实本地搭建的也可以做聚合，新版本craterepo是可以支持子文件夹，以前老版本centos6的时候我测试过不可以）
  - 代理方式，不需要全量下载
  - 支持聚合多yum
  - 除了支持yum，还可以支持开放的maven，npm,docker,pypi等其他库

1. 下载
[下载最新版本Linux](https://download.sonatype.com/nexus/3/latest-unix.tar.gz)

2. 安装
```bash
# 确保有安装jdk1.8
java -version

# 创建用户
user add -r -s /sbin/nologin nexus
echo "nexus - nofile 65536" >> /etc/security/limits.conf

# 解压安装
tar -xzvf latest-unix.tar.gz -C /opt
ln -s /opt/nexus* /opt/nexus
chown -R nexus:nexus /opt/nexus/

# 配置成服务
cat <<EOF >/etc/systemd/system/nexus.service
[Unit]
Description=nexus service
After=network.target
  
[Service]
Type=forking
ExecStart=/opt/nexus/bin/nexus start
ExecStop=/opt/nexus/bin/nexus stop
User=nexus
Restart=on-abort
  
[Install]
WantedBy=multi-user.target
EOF

# 启动
sudo systemctl daemon-reload
sudo systemctl enable nexus.service
sudo systemctl start nexus.service
```

3. 配置
登陆webui：http://localhost:8081/  默认密码admin/admin123






[官方介绍页面](https://www.sonatype.com/nexus-repository-oss)
[官方帮助文档](https://help.sonatype.com/repomanager3)