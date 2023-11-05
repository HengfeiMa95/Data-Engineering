\mainmatter

# 命令行{#cml}

> 上下无常，刚柔相易，不可为典要，唯变所适。--《周易》


命令行对于初学者而言就像是一个黑色的魔法界面，但实际上命令行才是计算机系统的母语。当我们使用命令行的时候，我们可以清楚的看到程序的输入、输出与执行过程，所以最接近数据工程的“透明原则”。与此同时，命令行脚本可以形成非常灵活的轻量级工具，可以把不同任务连接起来，是“天然胶水”。因此，熟练掌握命令行界面对于数据处理至关重要。

在命令行里，我们需要给计算机以非常精准的指令，这对我们的逻辑思考能力是一个质的改变。在日常生活中，“我以为我知道”和“我确定真的知道”是两个完全不一样的概念，尤其对于经济学家而言，很多时候我们经常处于前者的状态而不自知。在命令行中，我们被迫要达到第二种状态。

命令行普遍存在于几乎所有的操作系统，包括但不限于GNU/Linux，Unix，macOS，Windows WSL，以及各种超级计算机。掌握一套命令行工具，可以让我们在各类系统上穿梭自如。

我们不可能通过一章的篇幅帮助读者成为玩转命令行的专家，因此，本章的目的在于介绍最基本的命令行语法，希望借助本章的内容，为读者在未来研究与工作中使用和学习命令行工具留下合格的学习接口。

## 命令的基本概念
操作系统是一个内核（kernel），是内部与硬件交互的，外部是人机交互的部分，叫做壳（shell），是人类到操作系统的桥梁。shell有两种，命令行（Command Line Interface, CLI）和图形（Graphical User Interface, GUI）。最典型的命令行shell是GNU Bourne-Again Shell（bash）。使用man bash可以阅读bash的在线文档。

### 常用命令

命令有五种类型：可执行程序、脚本、Shell内建命令、Shell函数还有别名。使用`type`命令可以查看其他命令的类型。


```bash
type echo
type cat
```

```
## echo is a shell builtin
## cat is /bin/cat
```

特别地，命令行命令并不需要特别记忆，我们只需要了解常用命令，对于经常使用的命令，在使用的过程中很快就可以记下来

|命令|功能|
|------|---------------|
|`ls`|查看目录内的文件|
|`cd`|改变路径|
|`rm`|删除文件或文件夹|
|`git`|版本控制相关的命令|
|`less`|以分页方式查看文件内容|
|`man`|查看命令文档|
|`info`|GNU的命令详细介绍文件|
|`cat`|将多个文件连接起来输出|
|`pwd`|打印当前路径|
|`seq`|生成等差数列|
|`grep`|筛选符合条件的行|
|`wc`|计数|
|`echo`|打印字符到输出|
|`find`|查找文件|
|`touch`|新建文件/改变文件修改时间|
|`mkdir`|新建文件夹|
|`tee`|显示程序的输出并将其复制到一个文件中|
|`bc`|计算器|
|`uname`|查看系统基本信息|
|`hostname`|用户信息|
|`id`|用户id信息|
|`date`|当前日期时间|
|`uptime`|机器运行时间|
|`dmesg`|显示内核环缓冲区信息|
|`head`|文件头部若干行|
|`tail`|文件尾部若干行|
|`cut`|按列剪切输出|
|`paste`|合并文件的列，`cut`的反函数|
|`find`|查找路径下的文件|
|`file`|识别文件类型|
|`chmod`|修改文件权限|
|`sed`|文件流编辑器，macOS需要使用`gsed`|

### 标准输入与输出

执行一个shell命令行时通常会自动打开三个标准文件，即标准输入文件（stdin），通常对应终端的键盘；标准输出文件（stdout）和标准错误输出文件（stderr），这两个文件都对应终端的屏幕。进程将从标准输入文件中得到输入数据，将正常输出数据输出到标准输出文件，而将错误信息送到标准错误文件中。命令行逐行读入输入文件。

例如，`echo`命令的功能是将参数中的字符串直接送到标准输出。


```bash
echo "I am using the command line"
```

```
## I am using the command line
```

### 管道与进程管理

*管道*可将前一个程序的标准输出和后一个程序的标准输入连接起来，从而将多个程序无线串联起来。默认情况下，键盘是标准输入的端口，屏幕是标准输出的端口。


```bash
echo "I am using the command line" | wc -c
```

```
##       28
```

*重定向*通过`<`号将标准输入重定向到文件，`>`将标准输出重定向到文件


```bash
seq 100 > s100
```

*自由管道*命令`mkfifo`可以定义命名管道，后续命令中可以把命名管道作为输出和输入使用。下列代码中，第一行生成命名管道`filter`，第二行使用`tee`将`seq`和`grep`的结果存入名字为`filter`的管道中，第三行调出管道，并计算其中包含8的字符。


```bash
mkfifo filter.pipe # 生成新的命名管道
seq 100|grep 7| tee filter.pipe| wc -c &
grep 8 < filter.pipe
```

```
## mkfifo: filter.pipe: File exists
##       56
## 78
## 87
```

上述命令中，我们使用了`&`来将命令挂起（即后台运行）。使用`jobs`命令可以查看后台运行的进程，`fg`命令可以将后台进程拉至前台，`bg`命令可以将进程挂起到后台，`kill`加进程号可以杀死任务。

`awk`与`sed`命令的学习放在正则表达式之后，正则表达式的例子要单独拿出来分析。


```python
print("Hello Python!")
```

```
## Hello Python!
```
**快捷键**

ctrl+c
ctrl+d 结束文件

### 别名

### 脚本

把命令储存在拓展名为`.sh`的文件中，就形成了脚本文件。`bash`命令可以运行`Shell`脚本。


```bash
echo "seq 100 | paste -s -d + -| bc" > add_up_100.sh
bash add_up_100.sh
```

```
## 5050
```
在第一行填加脚本解释器 `#!/bin/bash`，可以告知脚本执行时候要使用`/bin/bash`
解释执行。此处，`#!`是约定好的关键字，当它在脚本文件的第一行时，内核会寻找它后面的解释器来执行脚本。

使用`chmod +x`赋予脚本文件可执行权限之后，就可以直接运行脚本文件。


```bash
gsed '1i#!/bin/bash' -i add_up_100.sh # gsed是mac里面的GNU sed
cat add_up_100.sh
chmod +x add_up_100.sh
./add_up_100.sh
```

```
## #!/bin/bash
## seq 100 | paste -s -d + -| bc
## 5050
```

### 传参

Bash脚本可以在执行时传递参数，参数以空格分隔，在脚本中可以通过`$n`来调用第n个参数，`$0`表示调用脚本名称。例如，脚本`args.sh`命令如下，


```bash
#!/bin/bash
echo "Bash 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```


```bash
./args.sh a1 b2 c3
```

```
## Shell 传递参数实例！
## 执行的文件名：./args.sh
## 第一个参数为：a1
## 第二个参数为：b2
## 第三个参数为：c3
```
另外，脚本中还有一些特殊的参数调用方式。

|参数调用|作用|
|:--------|:------------------|
|`$#`|传递到脚本的参数个数|
|`$*`|以单字符串显示所有参数|
|`$$`|脚本运行的当前进程ID号|
|`$@`|逐个显示所有参数|
|`$?`|显示最后命令的退出状态，0表示没有错误|

## 变量与数据结构

Bash脚本中以`var=value`的形式定义变量。由于Bash中约定空格的作用是区分参数，因此在变量赋值的等号前后不能有空格。如果需要把空格作为字符串处理，则需要引号。Bash中约定一切都是字符串，因此需要以`${var}`的方式来调用变量。Bash中单引号和双引号的区别在于，单引号中`$`会被当做字符串处理，而双引号中`$`被当做变量取值符号。


```bash
a="hello world"
echo ${a}
b=1
echo "$b =   1"
echo '$b    = 1'
```

```
## hello world
## 1 =   1
## $b    = 1
```

`$()`可以把命令运行的标准输出复制给变量，按行分隔的列表转换为空格分隔。`()`的含义其实是把命令的结果变成一个字符串。`${#}`可以返回变量字符个数。


```bash
n_list=$(seq 10)
echo ${n_list}
echo ${#n_list}
```

```
## 1 2 3 4 5 6 7 8 9 10
## 20
```

### 数组

`bash`数组类似于`python`里面的list，可以储存任意元素，有序，用`+=`符号添加元素，数组下标从零开始编号。


```bash
stdnt=()
stdnt+=(张三 李四 王五)
echo ${stdnt}
echo ${stdnt[@]}
echo ${stdnt[2]}
echo ${#stdnt[@]}
```

```
## 张三
## 张三 李四 王五
## 王五
## 3
```

### 字典

`bash`中也有字典结构，又叫关联数组（associative array）。  


```bash
declare -A stdnt # 声明 cargo 是字典
stdnt[age]=23
stdnt[gender]=male
echo ${stdnt[age]}
echo ${!stdnt[@]} # 取字典key
echo ${stdnt[@]} # 取字典值
```

```
## 23
## gender age
## male 23
```

## 运算

`$((expr))`用于计算表达式`expr`的结果，例如，


```bash
echo $((1+2))
echo $((1%2)) #取余数
echo $((4/2)) #整除
```

```
## 3
## 1
## 2
```

## 控制流程

### 选择结构

`bash`的选择结构如下：


```bash
if [ 3 -gt 2 ]; then
  echo "3>2"
else
  echo "3<=2"
fi
```

```
## 3>2
```

上述代码中，`[...]`是一个程序（更准确地说，`[`是命令，`]`是这个命令的参数），正式的命令为test，即`test 3 -gt 2`。由于`bash`中没有逻辑值，程序执行成功就是`TRUE`，执行失败则对应`FALSE`，因此真假判断的依据来执行程序的返回值，命令执行成功时返回值为`0`，执行失败返回`非0`。这里正好与`Python`相反。`$?`或`${?}`变量值可以得到前一条命令的返回值。


```bash
type [
test 3 -gt 2
echo $?
```

```
## [ is a shell builtin
## 0
```
`test`其他内建的判断参数：

|参数|含义|
|:------------------|:--------|
|`[ INTEGER1 -eq INTEGER2 ]` |相等|
|`[ INTEGER1 -ge INTEGER2 ]` |大于等于|
|`[ INTEGER1 -lt INTEGER2 ]` |小于|
|`[ INTEGER1 -ne INTEGER2 ]` |不等|
|`[ ! EXPRESSION ]` |取否|
|`[ EXPRESSION1 -a EXPRESSION2 ]` |取和|
|`[ EXPRESSION1 -o EXPRESSION2 ]` |取或|
|`-e file`|file文件是否存在|
|`-d dir`|dir路径是否存在|
|`-s file`|file文件是否存在且非空|

使用`man [`查看更多参数。

上述代码可以用一种简洁的方式来写：


```bash
[[ 3 -gt 2 ]] && echo "3>2" || echo "3<=2"
```

```
## 3>2
```

下面的脚本可以用于判断第一个参数的路径是否存在，若不存在则生成相应的路径。


```bash
#!/bin/bash
if [ ! -d $1 ]
then
        mkdir $1
fi
```

### 循环

#### for循环

以计算Fabonacci数列为例，`for`循环的结构如下，


```bash
x=1
y=1
for i in $(seq 10); do
  s=$(($x+$y))
  x=${y}
  y=${s}
done
echo ${y}
```

```
## 144
```

再举一个例子，把一个行数很多的文件拆分拆分成每一万行一组，可以使用下面的脚本。第一个参数和第二个参数为循环起止数，第三个参数为被拆分文件，第四个参数为储存文件路径与文件名称前缀。


```bash
#!/bin/bash
for a in $(seq $1 $2)
do
        gsed -n $((${a}*10000+1)),$(((${a}+1)*10000))p $3 > $4${a}.txt
        echo ${a}
done
```


#### while循环

上述代码可以改成如下的`while`循环。


```bash
x=1
y=1
n=0
while(($n<10)); do
  s=$(($x+$y))
  x=${y}
  y=${s}
  let n++
done
echo ${y}
```

```
## 144
```

特别地，`while`循环不需要迭代器，只要条件判断为真，就会循环。因此可以构造一个流，结合`read line`命令将文件中内容传输给`while`循环。`read line`的作用是读入管道的标准输出。

下面的代码先生成一个包括四个中国专利公开号的文件，然后将文件逐行读入到`while`循环中，最后通过`curl`命令从谷歌专利中下载该专利的文本内容。


```bash
if [ -s patent.txt ];then
  rm patent.txt
fi

echo 'CN102572430A
CN102414031A
CN104327221B
CN104735795B' > patent.txt

cat patent.txt|while read line; do
   curl -ks https://patents.google.com/patent/${line} > ${line}.html
done

head CN104735795B.html
```

```
## <!doctype html>
## <html lang="en">
##   <head>
##     <title>CN104735795B - 一种时分系统的任务调度方法及装置 
##         - Google Patents</title>
## 
##     <meta name="viewport" content="width=device-width, initial-scale=1">
##     <meta charset="UTF-8">
##     <meta name="referrer" content="origin-when-crossorigin">
##     <link rel="canonical" href="https://patents.google.com/patent/CN104735795B/zh">
```
#### until循环

`until`循环与`while`循环在处理方式上刚好相反。一般`while`循环优于 `until`循环。


#### break与continue

`break`终止循环、 `continue`跳入下一轮循环，这一点与`Python`的用法是一致的。此处不做展开。

## 函数

`bash`函数是五种命令之一。通过下面的代码结构可以定义`bash`函数。


```bash
function greet() {
  echo "Hello, $1"
}

greet 大佬
```

```
## Hello, 大佬
```
`bash`函数可以有return关键字指定返回结果，也可以默认将最后一行的运行结果作为返回值。

## bash内建正则表达式

在`bash`里面也内建了正则表达式的匹配，效率远远高于使用`egrep`。

由`[[ EXPR =~ REGEX ]]` 可以判断`EXPR` 是否能被`REGEX`匹配。运算支持扩展正则表达式，匹配结果由`BASH_REMATCH`列表给出。


```bash
seq 1000 | until ! read line
do
  if [[ $line =~ ^23+$ ]]; then
    echo ${line}
    echo ${BASH_REMATCH[0]}
  fi
done
```

```
## 23
## 23
## 233
## 233
```


## 应用案例

在获取谷歌专利文本时，如果把所有输出文件都放在同一个路径当中，将会给文件系统带来沉重的负担，甚至无法方便打开文件夹，更别说后续操作。因此，我们需要把任务设计地更细致。我们设计一个文件储存的架构，每100个文件储存在一个路径中，然后每100个文件夹再组成上一级目录，以此类推。下面的脚本可以实现这个功能。


```bash
#!/bin/bash
if [ ! -d "./description/$2" ]
then
        mkdir ./description/$2
fi

if [ ! -d "./description/$2/$3" ]
then
        mkdir ./description/$2/$3
fi

c=0
cat $1 | while read line
do
        echo "Downloading:${line}"
        if [ ! -d "./description/$2/$3/$((c/100))" ]
        then
                mkdir ./description/$2/$3/$((c/100))
        fi
        curl -k https://patents.google.com/patent/${line} > ./description/$2/$3/$((c/100))/${line}.html
        let c++
done
```


## 通配符
通配符可以按照规则匹配，用于构造简单的匹配模式，批量匹配。`*`匹配任意多个任意字符；`？`匹配一个任意字符。但是通配符可以匹配的空间比较有限，这就需要使用正则表达式。

参考资料：
Data Science at the Command Line

## sed

## awk



## 正则表达式
`egrep`可以利用正则表达式进行匹配，具体的正则表达式详见xx节。

**不同版本的shell可以写一段**

这里记录一下

## R的命令行选项
为了能够使用Make来构建完整的数据自动分析流程，我们就需要R代码能够从命令行解析参数并在R代码中使用。argparse包是受到Python中同名包的启发开发的，其用法与Python中接近，可以让我们在跨语言编程的时候更加轻松。

### Linux/Unix Shebang

在命令行中运行R脚本，可以使用`Rscript example.R`的方式，其中`example.R`是我们希望运行的脚本。在Linux和Unix系统中，有一种更简介的方式，即通过在脚本第一行添加`#! /usr/bin/env Rscript`的方式，来告诉操作系统这段脚本的要调用Rscript来运行。此时只要进入到`example.R`所在的路径中，使用命令`./exmaple.R`便可以直接运行这段代码。类似地，如果是一段Python3脚本的话，我们可以使用``#! /usr/bin/env Python3`

另外，`!#`被称作Shebang，翻译过来就是"井号叹号"，上一次我听到这么草率的命名还是我办公室所在的"新建楼"。

### argparse包实例
argparse包使用起来非常简洁，也没有太多函数。我们其官方教程中的一个实例来介绍如何使用它。

在命令行中的参数可以完整参数(用`--`调用)，与短参数(用`-`调用)，同时每一个参数都有其缺省值，以及对应的帮助文档解释。命令行参数本身有各种灵活的设置方式，感兴趣的读者可以进一步阅读Data Science at the Command Line.^[https://www.datascienceatthecommandline.com/1e/]。详细介绍这部分内容并不是本书的使命。

argparse包分为三个步骤，第一步是使用`ArgumentParser()`创建一个参数解析对象，第二步是使用`add_argument`函数为前面的对象增加参数，第三步是通过`parse_args()`把解析对象赋值并在后续程序中调用。

我们可以先把下面的脚本储存成为`example.R`，这段脚本的作用是输出若干个随机数，具体的随机分布与输出参数数量由命令行参数决定。


```r
#! /usr/bin/env Rscript
library(argparse)

# 创建参数解析对象
parser <- ArgumentParser()

# 设置参数
# 设置第一个参数verbose，缩写为v，其作用是告诉脚本是否打印完整的计算过程，其缺省值为TRUE
parser$add_argument("-v", "--verbose", action="store_true", default=TRUE,
        help="Print extra output [default]")

# 设置第二个参数quietly，缩写为q，其作用是修改verbose参数，当调用改参数时，verbose被修改为FALSE，从而导致不再打印计算过程
parser$add_argument("-q", "--quietly", action="store_false", 
        dest="verbose", help="Print little output")

# 设置第三个参数count，缩写为c，这是一个整数参数，缺省值5，在后续的代码中被用作确定输出随机数的个数
parser$add_argument("-c", "--count", type="integer", default=5, 
        help="Number of random normals to generate [default %(default)s]",
        metavar="number")

# 设置第四个参数generator，无缩写，用于确定调用何种随机分布，缺省值rnorm对应于正态分布
parser$add_argument("--generator", default="rnorm", 
        help = "Function to generate random deviates [default \"%(default)s\"]")

# 设置第五个参数mean，无缩写，浮点数，用于确定正态分布的均值，缺省值为0
parser$add_argument("--mean", default=0, type="double",
        help="Mean if generator == \"rnorm\" [default %(default)s]")

# 设置第五个参数sd，无缩写，浮点数，用于确定正态分布的标准差，缺省值为1
parser$add_argument("--sd", default=1, type="double",
        metavar="standard deviation",
        help="Standard deviation if generator == \"rnorm\" [default %(default)s]")

# 调用解析器，此时args就被赋值为命令行参数输入的相应值
args <- parser$parse_args()

# 根据verbose确定是否打印计算过程
if ( args$verbose ) { 
        write("writing some verbose output to standard error...\n", stderr()) 
}

# 根据其他参数，确定输出随机数的类型与次数
if( args$generator == "rnorm") {
        cat(paste(rnorm(args$count, mean=args$mean, sd=args$sd), collapse="\n"))
} else {
        cat(paste(do.call(args$generator, list(args$count)), collapse="\n"))
}
cat("\n")
```

之后我们需要首先在命令行修改脚本的权限，为其添加可执行权限
```{}
sudo chmod +x example.R
```

然后便可以执行该脚本，例如我们可以查该脚本对应的帮助，

```{}
./example.R --help
```

输出结果为：
<img src="argparser1.png" width="100%" />

或者使用参数输出3个正态分布随机数。
```{}
./example.R --mean=10 --sd=10 --count=3
```
输出结果如下：
<img src="argparser2.png" width="100%" />

也可以输出均匀分布的随机数，并且选择不打印计算过程。
```{}
./example.R --quiet -c 4 --generator="runif"
```
输出结果如下：
<img src="argparser3.png" width="100%" />


*需要单独整理一期shell命令行的内容*

## 关于命令行与IDE

相信很多人和我一样，一开始接触程序设计语言的时候，都会先接触IDE（Integrated Development Environment），例如`Rstudio`和`Pycharm`。每个语言都有一款甚至多款配套的优秀IDE，它们开发了各种效率工具，例如，语法高亮、自动补全、快捷键、错误提醒、编译和调试等，让编程更加丝滑。久而久之，我们也在无形之中产生了对IDE的依赖，一款IDE越是成功，这样的依赖就越强。以至于有一段时间，我离开`Rstudio`就会手足无措，并且曾一度设想，用`Rstudio`统一写作所有程序语言。虽然确实可以这么做，但并不是最佳选择。IDE会成为语言切换的障碍，阻碍进一步修炼。我就听有人说过，因为没有找到`Rstudio`在`python`总的替代物，因此一直使用`R`语言从事`python`更加擅长的工作。

决大多数情形中，我们的工作对象是数据，换句话说就是文件，而命令行天生就是针对文件的语言与环境。在命令行中，我们随手一个小命令就可以对文件进行查看和编辑，效率非常高。命令行作为其他语言的基础环境，可以轻松地连接不同语言的脚本。

回到命令行，才是数据工程最终的归宿。在命令行的黑魔法世界中，更有利于我们从根本上提升生产力，在不远的将来，做到手中无剑，心中有剑，人剑合一。

