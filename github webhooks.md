### 介绍

GitHub repository 通过webhooks与其他web服务交互。

通过webhooks，可以实现每次push后，线上自动deploy，将自动化进行到底。

设置页面在：`https://github.com/:owner/:repo/settings/hooks`。
点击 Add webhook 开始设置。

### 参数说明
+ Payload URL： 发送这个地址发送 POST 请求。

+ Content type： 数据格式。

+ Secret： 用于HMAC发送内容的密钥。可选。（服务端可通过Node.js的Crypto来验证）

+ 请求内容格式：[样列][lnk-example-delivery]

### 自动部署流程
1. GitHub收到push，于是向Payload URL发送POST请求。
2. 服务端收到请求，验证后，执行部署脚本。

### 服务端验证代码

```javascript
var signature = require('Crypto').createHMAC('sha1', secret).update(req.body).digest('hex');
if (signature != req.header('X-Hub-Signature')) return;
```

### 部署脚本
```bash
#!/bin/bash
# 更新并重启项目，记录日志到deploy.log文件。

data=$(date "+%Y-%m-%d %H:%M:%S")
echo ${data}" Runing deploy script..." >> deploy.log

cd ~/workspace/:repo/ &&
forever stop >> deploy.log &&
git pull origin production >> deploy.log &&
forever start app.js >> deploy.log

data=$(date "+%Y-%m-%d %H:%M:%S")
echo ${data}" Finished deploy script..." >> deploy.log

exit 0
```

[lnk-example-delivery]:https://developer.github.com/webhooks/#example-delivery
