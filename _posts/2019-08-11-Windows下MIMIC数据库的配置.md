---
title: Windows10操作系统下MIMIC数据库的配置
date: 2019-08-11 18:21:29 +0800
categories: [踩坑总结, 环境配置]
tags: [tips]
---
<h3>MIMIC数据库介绍</h3>  

MIMIC 是一个重症医学数据库，全称是Medical Information Mart for Intensive Care。2003年，在NIH的资助下，来自贝斯以色列女执事医疗中心(Beth Israel Deaconess Medical Center)、麻省理工(MIT)、牛津大学和麻省总医院(MGH)的急诊科医生、重症科医生、计算机科学专家等共同建立的一个数据库。该数据库在建立之初的名字为Multiparameter Intelligent Monitoring in Intensive Care II，简写为MIMIC II。

2016年9月，MIMIC II 数据库升级为MIMIC III，并改名为Medical Information Mart for Intensive Care，简写仍然是MIMIC[2]。由于MIMIC III 数据库包含的样本量大于MIMIC II，且下载和安装本地数据库时无需安装虚拟机，所以人们一般倾向于用最新的MIMIC III 开展研究，本文所介绍的内容也主要是基于MIMIC III，MIMIC II 的安装和使用可以参阅既往的文献。

***

<h3>准备</h3>  

1. **硬盘空间的准备**  
    压缩文档约10G，解压缩之后大概需要占用40-50G的空间。最终数据库程序加导入的数据大约占用60G空间，如果再计划后续使用eICU的数据库，那么请起码预留200-300G空间的一个分区。  
      
2. **数据准备**  
    MIMIC和eICU数据表，csv或csv.gz格式（后者为压缩格式，占地较小）。

3. **创建数据表脚本文件**  
    [Github: mimic-code-master.zip]，将buildmimic/postgres/下面的所有.sql文件保存在某位置，最好比较好找的地方，比如C:\mimic（本文后面都将按此地址举例）。

    下载地址：https://github.com/MIT-LCP/mimic-code/tree/master/buildmimic/postgres

4. **PostgreSQL/7-zip**
    官网下载。

5. **7-zip环境变量配置**
    因为我们的目的是自动调用命令行批量解压缩数据原始文件，所以需要系统知道我们的7-zip安装到了哪里。这就要告诉操作系统具体的路径，也就是要修改windows系统的PATH。  

    5.1 右键点击我的电脑；
    5.2 选择属性，在高级系统设置-高级里点击最下面的“环境变量”；
    5.3 在弹出的对话框里下方的“系统变量”里找到Path，选中，并点击下方的“编辑”；
    5.4 在弹出的对话框点击右侧的“新建”，输入7-zip的安装地址，默认的是C:\Program Files\7-zip，如果你安装到了其他的地方请修改成为相应的地址。
    5.5 当设置完成之后，点击开始菜单-运行，输入“cmd”打开命令行。在里面输入7z，如果提示“找不到7z”，那就证明PATH没有设置好。如果像下图一样，出来一大堆内容，那就表示7-zip已经准备好了。

***

<h3>本地数据库搭建</h3>  

1. **创建数据库**
    从开始菜单PostgreSQL目录里找到SQL Shell（psql），会打开一个命令行的界面。一路enter直到需要输入密码的地方，输入安装时设定的密码（默认是postgres）。

    ```
    DROP DATABASE IF EXISTS mimic;
    CREATE DATABASE mimic OWNER postgres;
    ```

2. **连接到数据库**
    ```
    \c mimic;
    ```

3. **创建表**
    ```
    CREATE SCHEMA mimiciii;
    set search_path to mimiciii;
    //每次连接psql数据库都需要设置
    \i D:/work/mimic-code/buildmimic/postgres/postgres_create_tables.sql
    ```
    地址为准备时mimic-code的路径。

4. **导入数据**
    ```
    \set ON_ERROR_STOP 1
    \set mimic_data_dir 'D:/mimic/v1_4'
    \i D:/work/mimic-code/buildmimic/postgres/postgres_load_data_7zip.sql
    ```

    地址是csv/csv.gv文件的路径。如果看到如下输出：  

    ```
    COPY 58976
    COPY 34499
    COPY 7567
    ```

    如果你能看到上图里最下面这几行字，停止在COPY 7567，那么基本上就成功了，后面做的就是需要等待。这个等待的时间取决于你的电脑配置，主要看是否有固态硬盘，可能是2-3个小时，也可能是十几个小时，也可能是一整天。最后大功告成，可以重新看到熟悉的mimic=#以及闪动的光标。

5. **建立索引**
  
    ```
    \i D:/work/mimic-code/buildmimic/postgres/postgres_add_indexes.sql
    ```

    建立索引也是要耗费一些时间的。

6. **测试数据库完整性**

    ```
    select
    icustay_id, intime, outtime
    from icustays
    limit 10;
    ```

    输入上述数据库查询代码，可以看到十行数据。MIMIC也提供了一个完整的数据检查脚本，可以运行下面的语句来测试，但也是要耗费一定时间。  

    ```
    \i D:/work/mimic-code/buildmimic/postgres/postgres_checks.sql
    ```

    最后会出来一张有26行记录的预期数据量与实际导入数据量的比较，如果最后一行row_count_check都是PASSED，那么就证明一切都没有问题了。

***

**参考：**
1. https://mimic.physionet.org/tutorials/install-mimic-locally-windows/
2. https://cloud.tencent.com/developer/news/160756







