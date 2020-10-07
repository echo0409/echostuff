---
title: IEEEtran confer LaTex模板食用方式
date: 2019-07-03 10:47:01 +0800
categories: [学习总结, 应用技能]
tags: [tips]
---
一篇学术论文主要包括标题、作者、正文、图表、参考文献等部分。许多基于LaTex的模板，比如IEEEtran journal/confer，给论文写作带来了很大便利，使作者省去了排版的烦恼。。本文将从标题，作者，正文，图片，表格，数学公式，参考文献七个部分来叙述IEEEtran confer模板的食用方式。

***
<h3>标题</h3>  
此模板的标题有以下几种：  

```C
\title{An Acute Kidney Injury...}
//文章标题

\begin{abstract}
...
\end{abstract}
//摘要:abstract
//关键字：IEEEkeywords  

\section{Methods}
//普通大标题

\section*{Acknowledgment}
//致谢

\subsection{Problem Formulation}
//段落标题

\subsubsection{Feature Generation}
//段内标题
//花括号内为标题内容，根据需要使用即可  
```


***
<h3>作者</h3>  
作者说明有多种方式可采用：

```C
//方式一
\author{\IEEEauthorblockN{作者姓名}
\IEEEauthorblockA{所在机构\\
Tianjin, China\\
邮箱}
\and

\IEEEauthorblockN{作者姓名\authorrefmark{1}}   
//说明为通讯作者
\IEEEauthorblockA{所在机构\\
Chengdu,China\\
邮箱}
\and


//方式二
\author{作者1$^{1,2}$, 作者2$^3$, 作者3$^4$\\ 
//上标代表所在机构
$^1$机构1\\
$^2$机构2\\
$^3$机构2\\
$^4$机构4\\
}
```
***
<h3>正文</h3>
正文直接在对应位置粘贴即可。粘贴时不需首行缩进，顶头写，段与段之间空一行即可。  

***
<h3>图片</h3>  
插入图片的操作代码如下：  
```C  
\begin{figure}[htpb]
  \centering    
  //图片居中
  \includegraphics[width=5cm]{fig/process}
  //width指定图片宽度，fig/process为图片路径，fig为与tex文件在同一层次的文件夹
  \caption{Framework of XGBoost-VM}
  //图片标题
  \label{fig:process}
  //图片标签，在文中引用时使用
\end{figure}

//在文中引用图片
\autoref{fig:process}

//需要引入宏包
\usepackage{hyperref}
```



***
<h3>表格</h3>  
LaTex表格操作相对复杂，因此可以使用在线表格生成网站：   

https://www.tablesgenerator.com/  
将得到的代码粘贴至tex文件中即可。引用操作与图片相似。以下为范例：
```C
  \begin{table*}[!t]
    \centering
    \caption{Performance of prediction models}
    \label{table:perfromance compare}
    \begin{tabular}{ccccc}
    \hline
    \multirow{2}{*}{Model} & \multicolumn{2}{c}{AUC}       & \multicolumn{2}{c}{Sensitivity} \\ \cline{2-5} 
                           & AKI(24h)      & AKI(48h)      & AKI(24h)       & AKI(48h)       \\ \hline
    XGBoost-VM             & \textbf{0.80} & \textbf{0.77} & \textbf{0.68}  & \textbf{0.65}  \\
    AB                     & 0.78          & 0.75          & 0.66           & 0.62           \\
    RF                     & 0.73          & 0.75          & 0.51           & 0.59           \\
    NB                     & 0.53          & 0.52          & 0.09           & 0.05           \\
    kNN                    & 0.63          & 0.62          & 0.37           & 0.30           \\
    SVM                    & 0.75          & 0.71          & 0.65           & 0.57           \\ \hline
    \end{tabular}
    \end{table*}  

//{table*}使表格变为单栏
```
***
<h3>数学公式</h3>  
LaTex插入数学公式有行内模式和行间模式两种。前者在正文的行文中插入数学公式，后者独立成行。

```C
//使用宏包
\usepackage{amsmath}

//行内模式示例
$E=mc^2$

//行间模式示例
\begin{equation}
E=mc^2.
\end{equation}

//写入复杂公式时可以使用在线LaTex公式编辑器生成代码贴入

```
***
<h3>参考文献</h3>
参考文献可以使用natbib方便的生成并管理。在tex文件所在目录下新建一个bib文件，使用google学术，搜索到所需文献，生成bibTex格式的引用，贴入bib文件中：
```C
//示例
@article{diesase2012improving,
//diesase2012improving为此文献代号，在文内引用时使用
  title={Improving global outcomes (KDIGO) acute kidney injury work group: KDIGO clinical practice guideline for acute kidney injury},
  author={Diesase, K},
  journal={Kidney Int Suppl},
  volume={2},
  number={1},
  pages={1--138},
  year={2012}
}
```
```C
//引入宏包
\usepackage[numbers,sort&compress]{natbib}
\usepackage{hyperref}

//在最后引入bib文件，生成参考文献
\bibliographystyle{IEEEtran}
\bibliography{script}

//文内引用
\cite{huang2018enhancing,mohamadlou2018prediction, hinson20181emf, kate2016prediction}
```

****
至此，论文的排版基本完成。
****
参考：https://www.bilibili.com/video/av15613797/
