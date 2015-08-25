1. 数据访问 立即数、寄存器、内存
2. 数据传送 movl、movsbl、movzbl
3. 基本操作 add、sub、imull、and、xor、or、shr、shl、sar、sal、pushl、popl
  1. 64位操作 idiv： ％eax ％ebx 被除数 -> ％eax 商 ％ebx 余数；imull： ％eax -> ％eax 低位 ％ebx 高位
  2. pushl： subl 4, ％esp; movl data, (%esp); pushl： movl (%esp), data; add 4, %esp;
4. 标志位 CF、SF、ZF、OF
  1. CF：进位，无符号溢出标志
  2. SF：符号位
  3. ZF：零位
  4. OF：溢出位，有符号溢出标志
5. 比较操作 cmp、set、jmp 判断、循环
  1. cmp 做减法，但不写入结果
  2. set、jmp 根据标志位判断比较结果
  3. leal 计算地址和简单的代数
  4. switch 通过跳转表实现
  5. 条件传送 cmovl 分别将两种情况的结果都求出，然后根据test返回不同的结果，更符合现代cpu的流水线运算特性，但适用情况非常少
6. 过程 ％ebp、％esp、call、leave、ret
  1. 将参数逆序压入栈
  1. call 将返回地址（调用者下一条指令所在的地址）压入运行栈
  2. ％ebp 压入运行栈
  3. ％ebp 存入栈顶地址
  4. ％esp 开辟栈空间
  5. ％ebp 取回存入的 ％ebp
  6. ％esp 回到call之前的位置
  7. 从返回地址开始执行
7. 数组
