ssh是用于远程加密连接计算机Shell的协议。

来自鸟哥的[简单介绍](http://vbird.dic.ksu.edu.tw/linux_server/0310telnetssh_2.php)。

这里记录了一些ssh比较有趣的用法。

### SCOCKS5 Web Proxy

通过Socks5建立Web代理，如果主机在墙外的话，嘿嘿……

```bash
# port is your custom port number(ie.9000).
$ ssh -D port username@hostname
```

在浏览器代理设置中，选择Socks5，主机填写`localhost`，端口填写`port`

### X11 Forwarding

X11 Forwarding就是把服务端运行的X Program通过ssh传送至客户端显示。
在客户端显示并操作服务端运行的GUI程序感觉很不错吧。

#### 步骤

__服务端__

修改配置文件`/etc/ssh/sshd_config`

```bash
...
X11Forwarding yes
...
```

__客户端__

```bash
$ ssh -X username@hostname
```

#### 补充

理论上ssh可以把服务端的任何数据传输到客户端。

所以还可以这样干，将服务端的X使用的端口forward到客户端的某个端口，
然后让客户端的X监听这个端口。

__服务端__

```bash
# The well known TCP ports for X11 are 6000-6063.
# X11 default listen on 6000
$ ssh -R 6010:localhost:6000
```

__客户端__

```bash
nc -l -p 6000 > /tmp/.X11-unix/X0
```
