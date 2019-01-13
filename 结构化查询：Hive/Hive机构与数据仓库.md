## 数据仓库

数据仓库是一个：**面向主题的（Subject Oriented）、集成的（Integrated）、相对稳定的（Non-Volatile）、反映历史变化（Time Variant）**的数据集合。用于支持管理层决策。

## 传统数据仓库

![传统数据库](https://github.com/jiaoqiyuan/163-bigdate-note/raw/master/%E7%BB%93%E6%9E%84%E5%8C%96%E6%9F%A5%E8%AF%A2%EF%BC%9AHive/img/%E4%BC%A0%E7%BB%9F%E6%95%B0%E6%8D%AE%E4%BB%93%E5%BA%93.png)

早期数据仓库建立在数据库之上，依托中央数据库，由于中央数据库运行于单机上，存储、计算和扩展能力都受到严重制约，已经不满足现在数据处理的需求，数据达到PB级时，传统数据库基本无法获得很好的性能。

![Hive数据仓库](https://github.com/jiaoqiyuan/163-bigdate-note/raw/master/%E7%BB%93%E6%9E%84%E5%8C%96%E6%9F%A5%E8%AF%A2%EF%BC%9AHive/img/Hive%E6%95%B0%E6%8D%AE%E4%BB%93%E5%BA%93.png)

Hive构建的数据仓库建立在Hadoop之上，将从数据源收集的数据会存放在HDFS后，Hive将HDFS上的数据映射为数据库，并以MapReduce任务的形式进行计算，为接下来的数据报表和分析提供服务。Hive可以满足大部分数据分析的需求，且拓展能力很强。

## Hive架构（3+1）

![Hive架构](https://github.com/jiaoqiyuan/163-bigdate-note/raw/master/%E7%BB%93%E6%9E%84%E5%8C%96%E6%9F%A5%E8%AF%A2%EF%BC%9AHive/img/Hive%E6%9E%B6%E6%9E%84.png)

- Hive组成模块：

    - 用户接口模块（User Interface）：用于实现对Hive的访问

        - CLI：命令行方式

        - JDBC：提供java编程接口

        - HWI（HiveWebUI）：网页形式提供访问接口

    - 元数据模块（MetaStore）：是一个数据库，可以是Hive自带数据库也可是外部数据库，常用MySQL。Hive的数据存储格式可以由用户自己定义，元数据记录了数据存储的格式，对照元数据信息，Hive才能将HDFS上的数据映射为有意义的数据表。

    - 驱动模块（Drivers）：驱动模块包括编译器、优化器、执行器等，负责将SQL语句转化成MapReduce任务。

- 关联模块

    - Hadoop：Hive并不能独立完成数据处理工作，它将SQL转化成MapReduce任务后交给Hadoop处理。

## Hive运行流程

![Hive运行流程](https://github.com/jiaoqiyuan/163-bigdate-note/raw/master/%E7%BB%93%E6%9E%84%E5%8C%96%E6%9F%A5%E8%AF%A2%EF%BC%9AHive/img/Hive%E8%BF%90%E8%A1%8C%E6%B5%81%E7%A8%8B.png)

1. Hive通过用户接口获得用户提交的查询任务，将其交给Driver模块。

2. Driver模块利用解析器解析查询语句获得用户任务。

3. 编译器根据用户任务到MetaStore查询元数据信息。

4. 编译器获得云信息后对任务进行编译，生成逻辑查询计划。

5. 之后编译器将逻辑查询计划转为物理执行计划

6. Driver模块最后将物理计划转交给Hadooop执行。

7. Hadoop将执行结果返回给Hive，Hive通过用户接口返回数据结果。