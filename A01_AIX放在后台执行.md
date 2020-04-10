AIX上使用nohup -p PID 的方式解决了忽略后台信号，注销用户后继续后台执行的问题。
之前没成功的原因是scp 在AIX上执行会生成2个进程：
一个scp命令进程，一个ssh进程，所以用 nohup -p PID  这两个进程都要分别执行一次。
```wiki
具体操作步骤如下：
1.前台执行scp命令
scp -P 40022 orcl_20200409_1037254151_s18120_p1 zyadmin@172.16.35.141:/alidata/dump/

2.输入ssh密码
xxxxx

3.暂停当前进程，将前台进程加入jobs
ctrl+z

4.将scp进程后台启动
bg %1

5.查看scp进程PID
ps -ef|grep scp

6.忽略scp和ssh后台进程的PID挂断(SIGHUP)信号
nohup -p 26214434
nohup -p 26213217

7.退出当前登录
exit
```
