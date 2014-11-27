### 服务端配置方法
参考这个[链接](https://help.ubuntu.com/community/L2TPServer)

#### 备注
* `/etc/ipsec.conf`中的`left`字段填写AWS的私有ip。
* 鉴于ubuntu上的l2tp客户端l2tp-ipsec-vpn和network-manager-l2tp-gnome都没有填写l2tp密钥的地方。服务端的`/etc/xl2tpd/l2tp-secrets`不配置。
* 链接时，若客户端`/var/log/auth.log`中出现` we require peer to have ID 'x.x.x.x', but peer declares 'y.y.y.y'`，
在`/etc/ipsec.conf`中`left`字段后面加入`leftid=x.x.x.x`（x.x.x.x是public ip）即可。

### 目前进展
1. 在搜索aliexpress的那台服务器上尝试失败。
2. 在我自己的EC2上配置成功。
3. 通过AMI将我自己的EC2移植到VPC中，成功。可能是系统环境的原因导致aliexpress的那台服务器上尝试失败。

#### 分析
比较成功与失败的服务端，发现`/var/log/auth.log`中的以下内容存在出入。

##### 失败
```
Jul 16 09:41:18 ip-10-0-1-91 pluto[2075]: packet from 116.231.204.107:500: received Vendor ID payload [RFC 3947] method set to=115
Jul 16 09:41:18 ip-10-0-1-91 pluto[2075]: packet from 116.231.204.107:500: received Vendor ID payload [draft-ietf-ipsec-nat-t-ike-02] meth=107, but already using method 115
Jul 16 09:41:18 ip-10-0-1-91 pluto[2075]: packet from 116.231.204.107:500: received Vendor ID payload [draft-ietf-ipsec-nat-t-ike-02_n] meth=106, but already using method 115
...
Jul 16 09:41:18 ip-10-0-1-91 pluto[2075]: "L2TP-PSK-NAT"[5] 116.231.204.107 #8: NAT-Traversal: Result using draft-ietf-ipsec-nat-t-ike (MacOS X): both are NATed
...
Jul 16 09:41:19 ip-10-0-1-91 pluto[2075]: "L2TP-PSK-NAT"[6] 116.231.204.107 #9: responding to Quick Mode proposal {msgid:09f66ecf}
Jul 16 09:41:19 ip-10-0-1-91 pluto[2075]: "L2TP-PSK-NAT"[6] 116.231.204.107 #9:     us: 10.0.1.91<10.0.1.91>[54.191.127.175]:17/1701
Jul 16 09:41:19 ip-10-0-1-91 pluto[2075]: "L2TP-PSK-NAT"[6] 116.231.204.107 #9:   them: 116.231.204.107[192.168.11.54]:17/57494
```

##### 成功
```
Jul 16 09:06:51 ip-10-0-1-91 pluto[2075]: packet from 116.231.204.107:500: received Vendor ID payload [RFC 3947] method set to=109
Jul 16 09:06:51 ip-10-0-1-91 pluto[2075]: packet from 116.231.204.107:500: received Vendor ID payload [draft-ietf-ipsec-nat-t-ike-02] meth=107, but already using method 109
Jul 16 09:06:51 ip-10-0-1-91 pluto[2075]: packet from 116.231.204.107:500: received Vendor ID payload [draft-ietf-ipsec-nat-t-ike-02_n] meth=106, but already using method 109
...
Jul 16 09:06:55 ip-10-0-1-91 pluto[2075]: "L2TP-PSK-NAT"[1] 116.231.204.107 #2: NAT-Traversal: Result using RFC 3947 (NAT-Traversal): both are NATed
...
Jul 16 09:07:06 ip-10-0-1-91 pluto[2075]: "L2TP-PSK-NAT"[2] 116.231.204.107 #3: responding to Quick Mode proposal {msgid:75ae6488}
Jul 16 09:07:06 ip-10-0-1-91 pluto[2075]: "L2TP-PSK-NAT"[2] 116.231.204.107 #3:     us: 10.0.1.91<10.0.1.91>[54.191.127.175,+S=C]:17/1701
Jul 16 09:07:06 ip-10-0-1-91 pluto[2075]: "L2TP-PSK-NAT"[2] 116.231.204.107 #3:   them: 116.231.204.107[192.168.11.110,+S=C]:17/0===192.168.11.110/32
```

### 剩余问题
* 我的ubuntu不能链接到配置成功的vpn服务器。
服务端`/var/log/auth.log`日志显示如下，判断是客户端的问题：
```
Jul 16 03:58:05 ip-172-31-18-52 pluto[23983]: "L2TP-PSK-NAT"[3] 116.231.204.107 #3: ERROR: netlink XFRM_MSG_DELPOLICY response for flow eroute_connection delete included errno 2: No such file or directory
```

* Jimmy的Mac也不能链接vpn。服务端日志看起来很正常。本地日志没有查看。
