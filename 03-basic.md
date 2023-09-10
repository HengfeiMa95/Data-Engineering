\mainmatter

# R编程基础 {#basic}

## R语言简介
今天写完这部分

### R语言是

## R与Rtudio的安装

R vs Py
http://ucanalytics.com/blogs/r-vs-python-comparison-and-awsome-books-free-pdfs-to-learn-them/

## R对文件和路径的相关操作

在R语言内部可以文件与路径进行一系列操作，这些函数都在base包中。

### 路径操作

#### 查看与修改路径
工作路径（working directory）指的是R在系统做工作的位置，R会默认在工作路径中读入写出数据。初学者往往会忽略工作路径的设置，从而给代码和数据管理带来制造混乱，我们建议每个R项目都使用独立的工作路径。下面的组命令可以对工作路径进行查看与修改。


```r
# 赋值一个工作路径变量，该路径应该是已经存在的，如果不存在需要使用下接中的dir.create函数创建
mypath = "~/myRworkingdir" 
# 为了确保路径存在，可以先查看其是否存在
file.exists(mypath)   
# 查看当前的工作路径
getwd()  
# 设定mypath为工作路径
setwd(mypath)
# 查看当前目录的子路径
list.dirs()             
# 查看当前目录的子目录和文件
dir()   
# 查看特定路径的子目录和文件
dir(mypath)  
# 列出目录下包括隐藏文件在内的所有的目录和文件
dir(mypath,all.files=TRUE)
# 只列出以字母R开头的子目录或文件
dir(mypath,pattern='^R')
# 查看当前目录权限
file.info(".")         
```

#### 创建路径
在R中可以创建新的路径，特别是可以递归的创建一个多级路径。


```r
# 在当前目录下，新建一个路径
dir.create("newdir")
# 递归创建一个3级子路径newdir1/newdir2/newdir3，使用recursive参数，直接创建会出错
dir.create(path="newdir1/newdir2/newdir3",recursive = TRUE)  
```


### 文件操作

R也可以对路径内的文件直接操作

#### 查看文件
R可以查看路径内文件的是否存在以及完整信息


```r
# 检查文件是否存在
file.exists("myfile.txt")  
# 查看文件完整信息
file.info("myfile.txt")
# 查看文件访问权限
file.access("myfile.txt",0)
#判断是文件还是目录。-d 是目录返回ture，-f是文件会ture
file_test("-d", "myfile.txt")
```

#### 创建、合并、复制文件
在R中也可以创建新的文件，并将内容写入该文件


```r
# 创建一个空文件 output.txt
file.create("output.txt")
# 把相关的内容写入output.txt件中，若没有这个文件则创建文件并写入内容 
cat("output \n", file = "output.txt") 
# 合并文件,把文件output.txt的内容合并到myfile.txt
file.append("myfile.txt", "output.txt")
#把文件myfile.txt复制到文件output.txt
file.copy("myfile.txt", "output.txt")    
```

#### 重命名文件与路径
使用file.rename函数可以重名工作路径或文件

```r
# 将文件file重名名为newfile
file.rename("myfile.txt","mynewfile.txt")
# 将路径newdir重名名为newdir_rename
file.rename("newdir", "newdir_rename")    
```

#### 删除文件

有两个函数可以使用file.remove和unlink，其中unlink函数还可以删除路径


```r
# 删除前面创建的文件
file.remove("mynewfile.txt", "myfile.txt") 
# 删除文件
unlink("output.txt")
# 删除路径
unlink("newdir", "newdir_rename")      
```

### 特殊路径
与R相关的有一些特殊路径，在debug和配置中会用到，此处列出。

- `R.home()`可以查看R软件的相关路径

- `.Library`可以找到R核心包的路径

- `.Library.site`可以查看R核心包的目录和root用户安装包目录

- `.libPaths()`可以查看R所有包的存放目录

- `system.file()`可以查看指定包所在的目录


## 代码风格

相信很多读者和我一样，每次回看以前写的代码就像看当年的QQ签名，浓浓的中二感。不过，我们还是得硬着头皮看，尤其是在代码第一次调试通之后，一定要再次完善代码。代码的作用本就是为了替代重复劳动，所以以后大概率还是会使用这段代码，如果代码写的一团糟，过一段时间之后，不仅别人可能看不懂，连自己也可能看不懂，有需要花大量时间去学习自己写的代码。更重要的是，代码的可读性也是可复现原则的内在要求。

本节中，我们总结了几条经验来探讨如何写出一段兼具简洁性、可读性与效率的代码，

- 代码要对机器友好，更要对人类友好。人类的时间是宝贵的，因此对人类友好（以代码的可读性为优化方向）的优先级高于对机器友好（以机器运算效率为优化方向）；

- 不吝啬写注释。注释至少能保证一段时间之后，自己能看懂代码；

- 不要使用没意义的变量名/函数名（例如a,temp），但同时要善于使用缩写，避免变量名/函数名无限长；对不同数据，不要使用同一个变量名

- 一整段代码的结构组成为：加载包-数据读入-定义函数-数据变形-运算-输出；

- 为了简化代码，重复操作函数化，数据变形使用pipeline，代码中使用缩进与换行。当然，函数中不要使用全局数据；

- 向量化与并行运算，尽量使用服务器而不是自己的笔记本，用机器时间换人类时间的同时优化机器时间；

- 数据结构是程序设计的第一步；

- 及时打印计算过程的信息，避免等待焦虑，便于debug；

- 运算符号前后加空格等方式加强代码的结构性；

- 不断询问自己有没有更好的方式，优化永无止尽。

此外，Hadley Wickham在他的著作中*The tidyverse style guide*中介绍了丰富的细节来帮助读者规范代码风格，同时使用`styler`和`lintr`可以快速检查与规范现有代码，Rstudio也为我们提供了自动提示代码风格的功能（如下图所示）。这些都可以帮助我们更好的规范代码。




# 函数

## 函数模块化

当代码中使用的函数非常冗长时，不仅会影响代码的结构与可读性，更会给代码调试工作带来额外的负担。此时，可以将函数模块化，然后单独储存在`.R`文件中，随后可通过`source`函数在主代码中调用。

例如，我们将对处理组与对照组在变量`y`层面进行加权T检验的代码定义为函数`wttest`。将其储存为`wttest.R`文件。

```r
library(weights)
wttest <- function(data, y){
    wtd.t.test(x = data[data$treat == 1,][y] %>% pull(1),
               y = data[data$treat == 0,][y] %>% pull(1),
               weight = data$weights[data$treat == 1],
               weighty = data$weights[data$treat == 0],
               samedata = FALSE)
}
```

在主代码中，可以通过`source`函数调用`wttest.R`中的`wttest`函数。

```r
source("wttest.R")
```
