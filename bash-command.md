```
jobs -p | xargs kill -9
ps aux | grep atom | awk '{print $2}' | xargs kill -9
```

* bash-command
    * 热键
        * Tab
            * 接在一串命令的第一个字的后面，则为命令补全；
            * 接在一串命令的第二个字以后时，则为『文件补齐』！
        * ctrl + c
            * 终止
        * ctrl + d
            * 键盘输入结束(End Of File, EOF 或 End Of Input)
    * 文件操作
        * cat  由第一行开始显示文件内容
        * tac  从最后一行开始显示，可以看出 tac 是 cat 的倒著写！
        * nl   显示的时候，顺道输出行号！
        * more 一页一页的显示文件内容
        * less 与 more 类似，但是比 more 更好的是，他可以往前翻页！
        * head 只看头几行
        * tail 只看尾巴几行
        * od   以二进位的方式读取文件内容！
        * touch 重置时间
    * 文件操作
        * cp, rm, mv
        * cp 复制
            * 直接复制会改变权限，加 -a 完全复制
        * rm 删除
        * mv 移动或重命名
    * 目录操作
        * ls, cd, pwd, mkdir, rmdir
        * rmdir 删除空目录
    * 搜索
        * which 搜索命令
        * whereis, locate, find
        * whereis 寻找特定文件（-b binary | -m manual | -s source | -u 非以上三种的文件）完整名称
        * locate 根据部分名称搜索，搜索数据库，有时需要先运行 updatedb 更新数据库
        * find 根据时间列出目录内的文件
    * 权限
        * chgrp ：改变文件所属群组
        * chown ：改变文件拥有者
        * chmod ：改变文件的权限, SUID, SGID, SBIT等等的特性
    * tar: xjvf [filename] / cjvf [filename] / tjvf [filename] (xzvf)
