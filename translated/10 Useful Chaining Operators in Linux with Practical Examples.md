示例说明10个Linux中有用打的链接操作符
================================================================================
Linux命令中的链接的意思是，结合几个命令并基于它们之间操作符的行为执行。Linux中的链接命令，有些像你在shell中写[短小的shell脚本]，并直接在终端中执行。链接使得自动处理变得可能。不仅如此，一个无人看管的机器在链接操作符的帮助下如此有条理地运行。

![Linux中的10个链接操作符](http://www.tecmint.com/wp-content/uploads/2013/12/Chaining-Operators-in-Linux.png)
*Linux中的10个链接操作符*

本文旨在常用的**链接操作符**，并作简短的描述和相关的肯定能提高你生产力的例子，除了有时能降低系统负载外还能让你写出简短有意义的代码。

### 1. 和号操作符 (&) ###

‘**&**‘的作用是使命令在后台运行。只要在命令后面跟上一个空格和 ‘**&**。你可以一口气在后台运行多个命令。

在后台运行一个命令:

    tecmint@localhost:~$ ping ­c5 www.tecmint.com &

同时在后台运行两个命令：

    root@localhost:/home/tecmint# apt-get update & apt-get upgrade &

### 2. 分号操作符 (;) ###

分号操作符使你可以一口气运行几个命令，命令顺序执行。

    root@localhost:/home/tecmint# apt-get update ; apt-get upgrade ; mkdir test

创建了一个‘**test**‘文件夹

### 3. 与操作符 (&&) ###

如果第一个命令执行成功(译者注: 原文为“if the execution of first command fails”，译者认为与上下文意思不同)，**与操作符 (&&)**会执行第二个命令，也就是说，第一个命令退出状态是**1**。这个命令在检查最后一个命令的执行状态时很有用。

比如，我想使用**[links command][2]**在终端中访问网站**tecmint.com**，但在这之前我需要检查主机是否**在线**或者**不在线**。

    root@localhost:/home/tecmint# ping -c3 www.tecmint.com && links www.tecmint.com

### 4. 或操作符 (||) ###

**或操作符 (||)**很像编程中的**else**语句。上面的操作符允许你在第一个命令失败的情况下执行第二个命令，也就是说，第一个命令的退出状态是**1**。

举例来说，我想要在非root帐户中执行‘**apt-get update**‘，如果第一个命令失败了，接着会执行第二个命令‘**links www.tecmint.com**‘。

    tecmint@localhost:~$ apt-get update || links tecmint.com

上面的命令中，由于**用户**不允许**更新**系统，这意味着第一个命令的退出状态是’**1**′,因此最后一个命令‘**links tecmint.com**‘会执行。

如果第一个命令成功执行并且退出状态是‘**0**‘呢？很明显的，第二个命令不会执行。

    tecmint@localhost:~$ mkdir test || links tecmint.com

这里，用户在家目录创建了一个‘**test**‘文件夹，这是被允许的。命令成功的执行，退出状态是‘**0**‘,因此，最后的命令不会执行。

### 5. 非操作符 (!) ###

**非操作符 (!)**很像**except**语句。这个命令会执行除了提供的条件外的所有的语句。要理解这点，在你的家目录创建一个目录‘**tecmint**‘，并‘**cd**‘到它这里。

    tecmint@localhost:~$ mkdir tecmint 
    tecmint@localhost:~$ cd tecmint

接下来，在文件夹‘**tecmint**‘下创建不同类型的文件。

    tecmint@localhost:~/tecmint$ touch a.doc b.doc a.pdf b.pdf a.xml b.xml a.html b.html

看一下我们在文件夹‘**tecmint**‘创建的新文件。

    tecmint@localhost:~/tecmint$ ls 

    a.doc  a.html  a.pdf  a.xml  b.doc  b.html  b.pdf  b.xml

用一种聪明的办法马上删除除了 ‘**html**‘之外的所有文件。

    tecmint@localhost:~/tecmint$ rm -r !(*.html)

验证一下上次的执行结果，使用[ls 命令][3]列出可见所有文件。

    tecmint@localhost:~/tecmint$ ls 

    a.html  b.html

### 6. 与 – 或 操作符 (&& – ||) ###

上面的操作符实际上是‘**与**‘和‘**或**‘操作符的组合。它很像‘**if-else**‘语句。

比如，让我们ping **tecmint.com**，如果成功打印‘**已验证**‘，否则打印‘**主机故障**‘。

    tecmint@localhost:~/tecmint$ ping -c3 www.tecmint.com && echo "Verified" || echo "Host Down"

#### 示例输出 ####

    PING www.tecmint.com (212.71.234.61) 56(84) bytes of data. 
    64 bytes from www.tecmint.com (212.71.234.61): icmp_req=1 ttl=55 time=216 ms 
    64 bytes from www.tecmint.com (212.71.234.61): icmp_req=2 ttl=55 time=224 ms 
    64 bytes from www.tecmint.com (212.71.234.61): icmp_req=3 ttl=55 time=226 ms 

    --- www.tecmint.com ping statistics --- 
    3 packets transmitted, 3 received, 0% packet loss, time 2001ms 
    rtt min/avg/max/mdev = 216.960/222.789/226.423/4.199 ms 
    Verified

现在，断开我们现在的网络连接诶，再试一下相同的命令。

    tecmint@localhost:~/tecmint$ ping -c3 www.tecmint.com && echo "verified" || echo "Host Down"

#### 实例输出 ####

    ping: unknown host www.tecmint.com 
    Host Down

### 7. 管道操作符 (|) ###

**PIPE**在第一个命令的输出作为第二个命令的输入时很有用。比如，‘**ls -l**‘的输出通过管道到‘**less**‘，并看一下输出。

    tecmint@localhost:~$ ls -l | less

### 8. 命令合并操作符 {} ###

合并两个或多个命令，第二个命令依赖于第一个命令的执行。

比如，检查一下文件‘**xyz.txt**‘和‘**xyz1.txt**‘是否在**Downloads**目录下，并输出相关的输出。

    tecmint@localhost:~$ [ -f /home/tecmint/Downloads/xyz.txt ] || echo “The file does not exist”

    tecmint@localhost:~$ [ -f /home/tecmint/Downloads/xyz1.txt ] || echo “The file does not exist” 

    “The file does not exist”

### 9. 优先操作符 () ###

这个操作符可以让命令以优先顺序执行。

    Command_x1 &&Command_x2 || Command_x3 && Command_x4.

在上面的伪代码中，如果**Command_x1**执行失败了会怎么样，**Command_x2**, **Command_x3**, **Command_x4**没有一个会自行，对于这种，我们使用**优先操作符**。

    (Command_x1 &&Command_x2) || (Command_x3 && Command_x4)

在上面的伪代码中，如果**Command_x1**执行失败，**Command_x2**同样失败，但是**Command_x3**会继续执行， **Command_x4**会依赖于 **Command_x3**的退出状态。

### 10. 连接符 (\) ###

**连接符 (\)**如它名字所说，被用于连接shell中跨越多行的命令。比如，下面的命令会打开文本文件**test(1).txt**。

    tecmint@localhost:~/Downloads$ nano test\(1\).txt

今天就到这里，我会近日开始另外一个有趣的文章。不要走开，继续关注**Tecmint**。不要忘记在评论栏里提出有价值的反馈。

--------------------------------------------------------------------------------

via: http://www.tecmint.com/chaining-operators-in-linux-with-practical-examples/

译者：[geekpi](https://github.com/geekpi) 校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创翻译，[Linux中国](http://linux.cn/) 荣誉推出

[1]:http://www.tecmint.com/category/bash-shell/
[2]:http://www.tecmint.com/command-line-web-browsers/
[3]:http://www.tecmint.com/15-basic-ls-command-examples-in-linux/