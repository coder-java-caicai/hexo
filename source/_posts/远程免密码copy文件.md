layout: scp
title: 远程免密码copy文件
date: 2016-04-28 17:13:48
tags: linux
---
1.先安装expect
yum -y install expect
2.脚本内容:
cat auto_svn.sh
#!/bin/bash
passwd='123456'
/usr/bin/expect <<-EOF
set time 30
spawn ssh -p18330 root@192.168.10.22
expect {
"*yes/no" { send "yes＼r"; exp_continue }
"*password:" { send "$passwd＼r" }
}
expect "*#"
send "cd /home/trunk＼r"
expect "*#"
send "svn up＼r"
expect "*#"
send "exit＼r"
interact
expect eof
EOF


expect的简单用法及举例
#!/usr/bin/expect -f
set password 123456
#download
spawn scp root@192.168.1.218:/root/a.wmv /home/yangyz/
set timeout 300 
expect "root@192.168.1.218's password:"
set timeout 300 
send "$passwordr"
set timeout 300 
send "exitr"
expect eof

