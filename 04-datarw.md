\mainmatter

# 数据读写 {#rw}

在介绍了R语言编程的基础知识之后，从本章开始，我们将系统介绍数据工程的数据处理流程。回忆一下，完整的数据分析流程至少包括：数据读写、数据清理、数据可视化、统计分析以及数据分析报告等部分。其中，数据是数据工程不同部分之间衔接的接口。因此，数据读写是数据分析的基础。

本章中我们将介绍常用的数据格式以及如何在R中读写不同格式的数据。

## 数据文件类型
数据格式可以分为通用数据格式


### HDF5
HDF5是Hierarchical Data Format(HDF)第5代的简称，起源于高性能计算领域，目前标准由非营利组织The HDF Group^[https://www.hdfgroup.org/solutions/hdf5/]组织开发和维护。其优点在于

- （1）原始表示：数据不必转换成文本，不涉及到转换误差；
- （2）自我描述：数据类型直接写在文件中，可以被自动识别；
- （3）跨语言：支持所有主流语言，有多重查看器

但是其缺点在于并非人类直接可阅读的数据格式，且对ASCII之外的字符支持没有标准，不保证可以准确处理中文。

HDF5由数据集（Dataset）、组（Group）以及元数据（Metadata）组成。数据集用于储存多维数组；组是数据集的容器，并且可以嵌套；元数据则用于描述数据集或者组的特征，例如数据名称，数据类型等。

在R语言中使用`hdf5r`包^[使用Mac操作系统的读者在安装`hdf5r`包的依赖`bit64`包时，可能会遇到报错`error: unknown type name 'uint64_t'`，此时需要将`/usr/local/include`文件夹修改为其他名字，修改名字后再次安装即可。]来读写HDF5数据，这里我们简单介绍一下hdf5r的基本操作。想更深入了解HDF5数据格式的读者可以直接到The HDF Group官网阅读相关文档。想了解更多hdf5r包的读者可以自学其官方教程^[https://cran.r-project.org/web/packages/hdf5r/vignettes/hdf5r.html]

在下面的代码中，我们通过H5File$new命令可以创建一个新的h5文件，通过create_group函数可以创建新的组，然后我们将数据mtcars放入分组，最后再通过close_all()关闭文件。

```r
library(hdf5r)
library(datasets)
file.h5 <- H5File$new("hz.h5", mode = "w")
mtcars.grp <- file.h5$create_group("mtcars")
mtcars.grp[["mtcars"]] <- datasets::mtcars
file.h5$close_all()
```

此时在工作路径中便生成了一个h5文件“hz.h5”，我们可以在命令行中通过python3-tables的vitables命令与hdf5-tools的h5dump命令查看HDF5文件内容。

通过H5File$new亦可以读入h5文件，然后通过names函数查看文件内的分组与数据集，并可以通过[[]]取出取出数据

```r
file.h5 <- H5File$new("hz.h5", mode = "r")
names(file.h5)
```

```
## [1] "mtcars"
```

```r
cars <- file.h5[["mtcars/mtcars"]][]
file.h5$close_all()
```




