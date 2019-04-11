# Q&A for PA2019 NUAA

NUAA PA2019常见问题解答（持续更新，欢迎加星）

------

# 读前必看

1. 提问以前务必翻看翻看本问答文档，对于文档中已经给出的问题，助教均不再回复，敬请谅解！
2. 有偿（`1分钱QQ红包`）征集遇到的问题的解决办法
   想分享你遇到问题的解决办法？没问题你可以把想说的话，想提醒大家的事情，还包括代码框架本身的 bug 等等`直接以 Pull Requests 的形式`提交到本仓库的 `Master` 分支，遇到有人提交我会上去检查一下后合并的。另外这个问答的issue功能关掉了，以免有人使用不文明用语或发答案。
3. 请参与编写 Q&A 的同学严格按照[中文文案排版指北](https://github.com/sparanoid/chinese-copywriting-guidelines)排版，以减少维护工作量，谢谢！
4. 注意这里不是共享答案的地方，每周检查报告的时候是有查重的，请同学们珍惜自己的代码……因此遇到这种提交不予合并，技术性难题大家可以一起讨论。温馨提醒：每次提交的报告都会**查重**，请不要把自己的代码分享给别人，遇到**雷同的按讲义[课程设计提交要求](https://www.jinhangdev.cn/ics/text/others/submit-requirement.html#关于学术诚信)处理，敬请知晓！**
5. 新添加的问题将放在最前面，以免大家看不到以为没更新，想看历史比较久远的问题往后面看


# 关于手册勘误（很重要）

请务必看讲义的 `i386 手册勘误`部分，否则**实现指令出错将对后面的进程产生巨大负面影响，甚至导致无法继续往后做**，请务必留意！另外，有部分勘误是**直接**在 `i386 手册的 PDF 文件中`用`批注`形式标注出来的，**若你的PDF阅读器并不能看到这些*标记和批注*，请使用以下推荐的PDF阅读器**：[Adobe Reader](https://adobe-reader.en.softonic.com/)或[Adobe Acrobat](https://get.adobe.com/cn/reader/otherversions/)

---

# PA1

1. 有人问到 `si 8` 步以上就会出现 `HIT GOOD TRAP` 是否正常，请注意看讲义，现阶段 `eip`到 `0x100026` 就证明已经成功结束运行了，若想让程序继续执行下去或者想知道为什么停在这里，请努力做到PA2。测试用例 `si 10`, `si 15` 等只是为了测试你在给予参数 10 或 15 时能否正常打印出运行的指令，实际上运行到 `HIT GOOD TRAP` 即表明用户程序已运行完毕，若要重新运行请退出后重新进入。

2. 关于讲义上的寄存器顺序和框架中排列的寄存器顺序不一样的问题：不影响任何功能使用，随意采取哪个顺序都行，但是千万不能动 `reg.h` 里面的寄存器结构体的名字，以及结构体内已经给定义好的寄存器的名字，也不要给使用的结构体/共同体取名字，根据讲义提示使用匿名共同体/结构体。

3. 安安分分写代码，讲义让在哪里写就在哪里写，让改哪里改哪里，**不要随意动框架代码（除非此刻你深刻了解你自己在做什么）**。

4. 有同学问忘了在 `PA1` 分支下做怎么办，本次的解决方法是回头是岸，马上转去 `PA1` 分支做就行了，这次 `PA0` 分支没有任何代码，查重时不会搞混，放心做就行，下次 `PA2` 的时候别忘了。

5. 关于扫描内存命令，想知道自己做的对不对很简单，把用户程序用`objdump工具`反汇编一下，然后对比反汇编代码就知道是否正确了。从经验的角度建议大家把每个4字节十六进制数在同一行把单字节序列形式也显示出来，**今后请务必注意大小端问题**。参考输出：

   （不需要一样，下面的写法与讲义不同，都可以理解为读一个四字节，然后用两种方法打印出来）

   ```
   (nemu) x 4 0x100000
   Address    Big-Endian ... Little-Endian
   0x00100000 0x000000b8 ... b8 00 00 00
   0x00100004 0x0000bb00 ... 00 bb 00 00
   0x00100008 0x00b90000 ... 00 00 b9 00
   0x0010000c 0xba000000 ... 00 00 00 ba
   ```

   当然只是条建议，方便今后看程序的反汇编，当然这条只是经验建议，可以不理睬。

   还有一个快速鉴别自己是否做对的方法，首先用命令

   ```
   (nemu) x 10 0x100000
   ```

   得到NEMU地址0x100000起始的若干字节内存信息，然后使用命令

   ```
   (nemu) si 5
   ```

   运行测试程序，观察执行的反汇编序列与`x`扫描内存命令得到的结果是否一致。若一致，则实现正常。

6.关于负数的处理，不强制必须怎么处理，只要看起来没有重大问题就行（特殊处理成负数或者下溢出都接受），如 `(nemu) p 1-2`，这条命令输出`-1`或者`0xffffffff`都算对。只要保证除了负数这个加分项外其他运算都正常实现就行，负数写了的话自行规定怎么处理，就能拿加分了。不要太离谱就行，如p 1-2你输出0x1234567那肯定怎么想不太对。

7.缓冲区要溢出了怎么处理：加长缓冲区长度就行了，或者直接assert(0)报错

8.token类型的编码：只要不冲突就行

# PA0

1. 安装虚拟机时遇到选择安装后黑屏，很可能是镜像文件本身的问题，请重新到 Debian 官网或者镜像点重新下载安装包镜像重新安装。

2. 有同学在 `nuaa.portal` 网络环境下发现无法用 DHCP 来配置网络，其原因是 nuaa.portal 不支持桥接，而拨号上网可以。解决方法是将虚拟机网卡 1 设置为 `NAT模式`，网卡 2 设置为设置为`仅主机网络(host-only)`，之后重新用 DHCP 来配置网络即可上网。**（注意 SSH 需要使用 host-only 网卡的地址）**

3. 如果遇到虚拟机无法启动等问题，多半是如下原因，请逐一排查：(1)电脑 BIOS CPU 虚拟化未开启；(2)电脑中 Hyper-v 等其他虚拟机占用了虚拟化资源；(3)没有用管理员身份运行虚拟机。
