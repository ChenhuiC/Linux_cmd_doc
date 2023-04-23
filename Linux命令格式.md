# Linux命令格式

## 前言
Linux和Git命令中的中括号[]和尖括号<>和竖杠|和省略号...其实都是根据一个规则制定的，这个规则便是[Linux帮助文档的编写规则:命令行界面描述语言Docopt](http://docopt.org/)。所有的Linux或Git的帮助文档的公式都是按照命令行界面描述语言Docopt的语法规则来编写，因此我们了解清楚命令行界面描述语言Docopt就终于能读懂Linux和Git命令的帮助文档了！
## 使用说明
### A. **参数**（位置参数和选项的参数）
参数是命令的操作对象，一般文件、目录、用户和进程等可以作为参数被命令操作，用于指定选项操作的位置路径
    1. 位置参数\<argument>或ARGUMENT
        以“<”开头、以“>”结尾的单词和大写单词被解释为位置参数，如下面的host和port。
        ```bash
        my_program <host> <port>
        ```
    2. 选项的参数 -o argument或--option=argument中的argument    
    紧跟在选项后面的参数，用于指定选项的操作对象
### B. 选项-o或--option
选项是包括一个或者多个字母的代码，前面有一个“-”或“–”连字符，主要用于改变命令执行动作的类型。选项又分为短格式选项（-l）和长格式选项（–all）。短格式选项是英文的简写。
    
选项和参数连用的规则与注意事项：
1. 短选项“-”可以以“堆叠”，意思三个选项-a -b -c可以写成-abc，并且他们作用等价
2. 短选项可以在零~多个空格后指定参数，并且他们作用等价。-a file等价于-afile等价于-a（多个空格）file
3. 短选项堆叠且在一个或多空格后指定参数的歧义性：不带空格指定参数的命令-afile存在歧义。因为无法判断-afile是多个堆叠的短选项-a -f -i -l -e，还是选项-a带有参数file
4. 长选项可以在一个或多个空格或等于符号=后指定参数：如--input=ARG等价于--input ARG等价于--input（多个空格）ARG
5. 短选项和长选项在一个或多空格后指定参数的歧义性：-a file和--input ARG存在歧义。因为无法判断file和ARG分别是短选项-a和长选项--input的选项的参数还是位置参数
6. 针对第3点和第5点提到的歧义性，作为程序员的我们，最好的解决方案是：规范书写命令来避免歧义。（1）书写带参数的短选项命令时，最好写成-a（一个或多个空格）file来避免歧义；（2）书写带参数的长选项命令时，最好写成--input=ARG来避免歧义。
### C. 命令
所有不遵循上述约定(A)和(B)的--options或<arguments>的其他单词都被解释为命令或子命令
### D. [可选元素]
**元素指的是:选项,参数,命令。**  
用方括号“[]”括起来的元素被标记为可选的，意思是:可以有该元素也可以没有该元素。元素是否包含在相同或不同的括号内并不重要，它们完全等价。例如：  

    my_program [command --option <argument>]

与下面的命令是等价的：

    my_program [command --option <argument>]

因此不要误以为my_program [command --option \<argument>]命令只有一下2种可能性：
>错误案例：
my_program [command --option <argument>]的所有可能性：
第一种可能性:my_program command --option argument
第二种可能性:my_program

正确的情况是这3个元素都是分开的
>正确案例:
my_program [command --option <argument>]的所有可能性：
第一种可能性:my_program command --option argument
第二种可能性:my_program
第三种可能性:my_program command
第四种可能性:my_program --option
第五种可能性:my_program argument
第六种可能性:my_program --option argument
第七种可能性:my_program command argument
第八种可能性:my_program command --option

### E. (必选元素)
默认情况下，所有元素都是必需的，如果不包括在括号“[]”中。然而，有时有必要根据需要显式地使用括号“()”标记元素。例如，当您需要对互斥元素进行分组时(参见下一节):

    my_program (--either-this <and-that> | <or-this>)

另一个用例是，当您需要指定如果有一个元素存在，那么就需要另一个元素，您可以这样实现:

    my_program [(<one-argument> <another-argument>)]

在这种情况下，有效的程序调用可以没有参数，也可以有两个参数。
### F. 元素|其他
互斥的元素可以通过竖杠“|”进行分离，如下

    my_program go (--up | --down | --left | --right)


### G. 元素...
使用省略号“…”指定左边的参数(或一组参数)可以重复一次或多次
### H. [options]
“[options]”是一个快捷方式，它允许避免在模式中列出所有选项(从带有描述的选项列表中)。例如:
```bash{.line-numers}
my_program [options] <path>

--all             List everything.
--long            Long output.
--human-readable  Display in human-readable format.
```
与下面的命令是等价的：
```bash{.line-numers}
Usage: my_program [--all --long --human-readable] <path>

--all             List everything.
--long            Long output.
--human-readable  Display in human-readable format.
```
