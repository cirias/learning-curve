#### 文件路径
File system 中的相对路径是相对于所执行的文件的，例如：
```bash
# 相对的是app.js所在的路径
$ node app.js
```
但是这样对于模块中要调用相对路径就会很不方便。
此时可以使用全局变量`__dirname`，其实它并不是完全的全局变量，只是模块的全局变量，保存着模块的绝对路径。
同样`__filename`保存的是模块入口文件的绝对路径。

## Async
* Do not remove element from array in async method. 