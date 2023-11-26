\mainmatter

# 数据可视化{#visu}

> 人都是视觉动物。

## 从一个可以画恐龙的包说起

在闲逛R论坛的时候，偶然发现有人贴了一段如下的代码，这个代码可以画出一个恐龙。

```r
# install.packages("datasauRus")
library("datasauRus")
library(ggplot2)
ggplot(subset(datasaurus_dozen, dataset=='dino'), 
             aes(x, y)) + 
  geom_point()  
```

<img src="06-visualization_files/figure-html/unnamed-chunk-1-1.png" width="672" />

太神奇了吧，居然有人写了个包来画恐龙！但是仔细一想，这事儿可能没有那么简单。事出非常必有妖，谁会闲到辛辛苦苦发布一个R包就为了画个恐龙呢。

果然datasauRus是一个非常有名的数据包，他包括了若干组描述性统计完全一致，但是分布却截然不同的数据集，恰恰说明了可视化的必要性。

详情可见论文，*Same Stats, Different Graphs: Generating Datasets with Varied Appearance and Identical Statistics through Simulated Annealing**


```r
library(datasauRus)
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
datasaurus_dozen %>% 
    dplyr::group_by(dataset) %>% 
    dplyr::summarise(
      mean_x    = mean(x),
      mean_y    = mean(y),
      std_dev_x = sd(x),
      std_dev_y = sd(y),
      corr_x_y  = cor(x, y))
```

```
## # A tibble: 13 × 6
##    dataset    mean_x mean_y std_dev_x std_dev_y corr_x_y
##    <chr>       <dbl>  <dbl>     <dbl>     <dbl>    <dbl>
##  1 away         54.3   47.8      16.8      26.9  -0.0641
##  2 bullseye     54.3   47.8      16.8      26.9  -0.0686
##  3 circle       54.3   47.8      16.8      26.9  -0.0683
##  4 dino         54.3   47.8      16.8      26.9  -0.0645
##  5 dots         54.3   47.8      16.8      26.9  -0.0603
##  6 h_lines      54.3   47.8      16.8      26.9  -0.0617
##  7 high_lines   54.3   47.8      16.8      26.9  -0.0685
##  8 slant_down   54.3   47.8      16.8      26.9  -0.0690
##  9 slant_up     54.3   47.8      16.8      26.9  -0.0686
## 10 star         54.3   47.8      16.8      26.9  -0.0630
## 11 v_lines      54.3   47.8      16.8      26.9  -0.0694
## 12 wide_lines   54.3   47.8      16.8      26.9  -0.0666
## 13 x_shape      54.3   47.8      16.8      26.9  -0.0656
```

## 数据可视化的目的

数据可视化的目的是直观地向读者传递数据中包含的信息。重要的事情说三遍，**传递信息、传递信息、传递信息**！

因此，一个优秀的可视化应该具备以下的特征。

1. 重点突出。要让读者第一眼就能发现你要传达的主要信息，而不是暗戳戳的隐藏起来，等待别人发现。

2. 表意清晰。主要信息的传递要明确清晰，不要产生歧义，让读者自行发挥。充分利用注释、标题等来避免歧义。

3. 信息量大。图形的标题、注释都可以传达信息，在有限的图形内，尽可能传递更多信息。

4. 逻辑完整。数据传递的信息要有逻辑，能够独立阅读。

5. 美观。尽可能增强图形的美学水平，让读者愿意看，看着舒服，愿意分享。

## 图形语法（Grammar of Graphics）

图形语法（Grammar of Graphics，简称GG）是Leland Wilkinson开发的一套用来描述所有统计图表深层特性的语法规则，该语法回答了**什么是统计图表**这一问题，以自底向上的方式组织最基本的元素形成更高级的元素。在GG看来，一张图就是从数据到几何标记对象的图形属性的一组映射，此外图形中还可能包含数据的统计变换，最后绘制在某个特定的坐标系中。此时，科学绘图就会非常接近画油画，绘图的过程就是在一张空白的画布上，一层一层地叠加图形要素。

粗略地，GG拆解的主要图形要素可以分为三大类：几何对象、美术属性与标签注释。

**几何对象**指的是将数据映射到图形之后的几何类型，包括点（point）、线（line）、直方（histgram）、柱（bar）等。

**美术属性**指的是几何对象呈现在图形中的属性，例如颜色（color）、形状（shape）、尺寸（size）、透明度（alpha）等。属性让同一类几何对象传递更加丰富的信息。

**标签注释**包括标题（title）、轴标题（x/y title）、轴标签（xlab/ylab）、文本标签（text label）、注释（note）、图例（legend）等。可以进一步丰富图形传达的信息。

通过图形语法可以实现统计绘图的自动化，满足可复现原则与一次原则。

更多关于GG的内容，可以参考Wilkinson的著作[The Grammar of Graphics](https://link.springer.com/book/10.1007/0-387-28695-0)。

## `ggplot2`

讲GG在R语言中实现的又是我们熟悉的大神Wickham，他设计了`ggplot2`包，`ggplot2`及其家族现在已经成为统制图的最佳工具。

我们看下发表在经济学人杂志的一篇文章中的图片，图片来源<http://www.economist.com/node/21541178>。数据来源，

<img src="./images/visualization/Economist1.png" width="90%" />

这幅图首先在视觉呈现非常美观，然后信息传达非常丰富，几乎所有的图形要素都被利用了起来。可以分类总结如下。

<img src="./images/visualization/Economist2.png" width="90%" />

本节的任务就是逐步学习`ggplot2`的要素，并画出此图。教学材料受到哈佛大学[IQSS](http://tutorials.iq.harvard.edu/)的启发。数据位于：[EconomistData.csv](https://drive.google.com/file/d/1CyCRpPVqLP-_BWjURiV5127rTiSHWuqe/view?usp=drive_link)。

### `ggplot2`几何对象

`ggplot2`的语法是先通过`ggplot`函数指定数据，然后使用`+`叠加后续绘图动作。我们使用





**练习题**

导入EconomistData.csv数据，（数据来源：http://www.transparency.org/content/download/64476/1031428）该数据包括了部分国家的人类发展指数与腐败感知指数。创建一个以 CPI 为x轴，HDI为y轴的散点图。

### `ggplot2`的美术属性

### `ggplot2`辅助

#### 主题

#### 调色板

选择优雅的颜色是高质量数据可视化的保障。如果不是艺术造诣很高的话，选择成熟的配色是最安全的选择。`ggthemes`包封装了多种成熟的主题，`ggsci`则包括了多种杂志的配色方案，我们可以使用`scales`包的show_col函数来查看色号。

例如，我们要查看并使用经济学人杂志的调色板，可以用下面的代码


```r
library(ggthemes)
library(scales)
library(ggsci)
show_col(economist_pal()(9))
```

<img src="06-visualization_files/figure-html/unnamed-chunk-5-1.png" width="672" />

```r
economist_pal()(9)
```

```
## [1] "#6794a7" "#014d64" "#01a2d9" "#7ad2f6" "#00887d" "#76c0c1" "#7c260b"
## [8] "#ee8f71" "#adadad"
```

如果要使用science杂志的配色，可以用下面的代码。

```r
show_col(pal_aaas()(9))
```

<img src="06-visualization_files/figure-html/unnamed-chunk-6-1.png" width="672" />

```r
pal_aaas()(9)
```

```
## [1] "#3B4992FF" "#EE0000FF" "#008B45FF" "#631879FF" "#008280FF" "#BB0021FF"
## [7] "#5F559BFF" "#A20056FF" "#808180FF"
```

当然，艺术造诣和编程能力并不是互斥的技能点。例如，R语言社区资深开发者（`ggplot2`包的作者）、计算认知科学家Danielle Navarro就是一名计算艺术家（computational artist）。下面是她的作品*Dancer*，更多作品可以浏览https://art.djnavarro.net/。

## 案例：DID结果的可视化

## 动图

## 大屏

### 交互式
