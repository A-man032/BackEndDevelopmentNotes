[TOC]

# Neo4j

## 1. 概述

Neo4j是一个高性能**NoSQL图形数据库**，将结构化数据存储在网络（图）上而不是表中。它是**面向网络的数据库**，是一个嵌入式的、基于磁盘的、具备完全的事务特性的Java持久化引擎。也可被看作是一个高性能的图引擎。

Neo4j提供了大规模可扩展性，在一台机器上可以处理数十亿节点/关系/属性的图，可以扩展到多台机器并行运行。

相对于关系数据库来说，图数据库善于处理大量复杂、互连接、低结构化的数据，这些数据变化迅速，需要频繁的查询。在关系数据库中，这些查询会导致大量的表连接，因此会产生性能上的问题。

Neo4j重点解决了拥有大量连接的传统RDBMS（关系数据库管理系统，Relational Database Management System）在查询时出现的性能衰退问题。通过围绕图进行数据建模，Neo4j会以相同的速度遍历节点与边，其遍历速度与构成图的数据量没有任何关系。此外，Neo4j还提供了非常快的图算法、推荐系统和OLAP风格的分析，而这一切在目前的RDBMS系统中都是无法实现的。

### 1.1 图数据库和关系数据库的对比

对于**多对多查询**：

- 关系数据库需要设置**中间表**，通过中间表实现多对多查询。 对于多层多对多关系，在SQL语句中出现多个`join`语句，使得关系数据库性能急剧下降。因为`join`语句是全表查询，根据相关的外键，对另一张表进行全表遍历来查询数据。（一条SQL语句中最好不要超过3个`join`语句）
- 多层多对多关系类似于复杂的网络。现实应用场景中有许多多对多关系，因此产生出了图数据库。图数据库中没有表之间通过外键关联的概念。所有的数据都存在一个空间中，数据用节点来保存，数据之间的关系用边来表示，边不限数量和方向。
- 图数据库适合多对多复杂关系的存储。应用场景：人际关系、交通网络等。

## 2.[ Neo4j 教程](https://www.w3cschool.cn/neo4j/)

Neo4j是一个世界领先的开源图形数据库。 它是由Neo技术使用Java语言完全开发的。

Neo4j是基于JVM的开源**NoSQL图数据库**，源代码托管在GitHub上。

相关网站：

- [Neo4j官网](https://neo4j.com/)
- [Neo4j源代码](https://github.com/neo4j)
- [Neo4j开发实例](https://github.com/neo4j-examples)
- [Neo4j中文社区](http://neo4j.com.cn/)

**Neo4j**：

- 开源
- 无Schema（图表）
- 没有SQL
- 图形数据库（GDBMS）

**图形数据库**：

- 以图形结构的形式存储数据的数据库。
- 以**节点、关系、和属性**的形式存储应用程序的数据。RDBMS以表的行、列的形式存储数据。

**热门图形数据库**：

Neo4j是一个流行的图数据库。 其他图形数据库是Oracle NoSQL数据库，OrientDB，HypherGraphDB，GraphBase，InfiniteGraph，AllegroGraph。

**图形**：

- 图形是一组节点和连接这些节点的关系。节点和关系包含表示数据的属性。属性是用于表示数据的键值对。

**Neo4j服务器容量**：

| S.No | Neo4j的构建基块 |  容量   |
| :--: | :-------------: | :-----: |
|  1   |      节点       | 约350亿 |
|  2   |      关系       | 约350亿 |
|  3   |      标签       | 约275亿 |

### 2.1 目前需要图形数据库

使用 RDBMS 数据库来存储更多连接的数据，那么它们不能提供用于遍历大量数据的适当性能。 在这些情况下，Graph Database 提高了应用程序性能。

如今，大多数社交网络应用程序（如Facebook，Google +，LinkedIn，Twitter，Yammer 等）和视频托管应用程序（如 Google YouTube，Flickr，Yahoo Video等）都在使用更多连接的数据。

当应用程序中包含大量结构化、半结构化、非结构化的连接数据时，RDBMS数据库中表示非结构化连接数据非常不容易。因为在**RDBMS数据库中存储大量连接的数据，检索和遍历都是非常困难和缓慢的**。图形数据库更适合表示和存储大量连接的数据。

**图形数据库将每个配置文件数据作为节点存储在内部，通过关系与相邻节点连接。**

**结构化数据、非结构化数据、半结构化数据：**

- **结构化数据**：由二维表结构来逻辑表达和实现的数据，严格遵循数据格式与长度规范，主要通过**关系数据库**进行存储和管理。**特点**：数据以**行**为单位，一行数据表示一个实体信息，每一行数据的属性是相同的。
- **非结构化数据**：数据结构不规则或不完整，没有预定义的数据模型，二维逻辑表不便于表现的数据。包括所有格式的**办公文档、文本、图片、HTML、各类报表、图像和音视频信息**等。
- **半结构化数据**：结构化数据的一种形式，虽然不符合关系型数据库或其他数据表的形式关联起来的数据模型结构，但包含相关标记，用来分隔语义元素以及对记录和字段进行分层。常见的半结构数据：XML和JSON。

### 2.2 Neo4j特点和优势

**特点：**

- 图由顶点（Vertex）、边（Edge）和属性（Property）组成
- Neo4j创建的图是用顶点和边构建的一个**有向图**
- 声明式查询语言：**Cypher**
- 通过使用**Apache Lucene**支持索引
- 支持UNIQUE约束
- 包含一个用于执行CQL（Cypher查询语言）命令的UI：Neo4j数据浏览器
- 支持完整的**ACID**（原子性、隔离性、一致性、持久性）事务
- 采用原生图形库与本地GPE（图形处理引擎）
- **查询的数据导出到JSON和XLS格式**
- 提供了REST API，可以被任何编程语言（如Java，Spring，Scala等）访问
- 提供了可以通过任何UI MVC框架（如Node JS）访问的Java脚本
- 支持两种Java API：Cypher API和Native Java API来开发Java应用程序

**优点：**

- 容易表示连接的数据
- 检索、遍历、导航更多的连接数据非常容易和快速
- 易于表示半结构化数据
- Neo4j CQL查询语言命令容易学习
- 使用简单而强大的数据模型
- 不需要复杂的连接来检索连接的、相关的数据，因为它容易检索它的相邻节点或关系细节没有连接或索引

**缺点：**

- AS的Neo4j 2.1.3最新版本，它具有支持节点数，关系和属性的限制
- 不支持Sharding

什么是[**Sharding**](https://www.cnblogs.com/zhwl/p/3640710.html)？

- Sharding 是把数据库Scale Out（横向扩展）到多个物理节点上的一种有效的方式。Shard这个词的意思是“碎片”。如果将一个数据库当作一块大玻璃，将这块玻璃打碎，那么每一小块都称为数据库的碎片（DatabaseShard）。将整个数据库打碎的过程就叫做sharding（分片）。
- 形式上，Sharding是将大数据库分布到多个物理节点上的一个分区方案。每个分区包含数据库的某一部分，称为一个Shard。且分区方式是任意的，不局限于传统的水平分区和垂直分区。
- 一个Shard可以包含多个表的内容甚至可以包含多个数据库实例中的内容。
- 每个Shard被放置在一个数据库服务器上，一个数据库服务器可以处理一个或多个Shard的数据。系统中需要由服务器进行查询路由转发，负责将查询转发到包含该查询所访问数据的Shard或Shards节点上去执行。

**Sharding和分区的区别：**

1. 扩展方式不同。Sharding属于scale out（横向扩展），分区属于scale up（纵向扩展）。
   - Scale out：横向扩展架构的升级通常以**节点**为单位。每个节点拥有容量、处理能力、I/O带宽。一个节点被添加到存储系统，系统中的三种资源将同时升级。**容量增长和性能扩展是同时进行的**。
   - Scale up：纵向扩展是利用现有的存储系统，通过不断增加存储容量来满足数据增长的需求。**这种方式只增加了容量，带宽和计算能力并没有相应的增长。所以，整个存储系统很快会达到性能瓶颈，需要继续扩展。**
   - 两个形象比喻：
     - 传统火车和动车。传统的存储Scale-up架构的存储就好像传统的火车一样，当后面的磁盘越挂越多的时候，控制器性能以及背板带宽却不能相应提升，因此传统存储在磁盘容量扩容到一定程度时候，往往性能下降。集群存储就好像新一代的“动车组”火车一样，当火车车厢增加的时候，前面的火车头动力也随之增加，因此不会发生性能瓶颈。
     - 当你只有六七条鱼的时候， 一个小型鱼缸就够了;可是过一段时间新生了三十多条小鱼，这个小缸显然不够大了。如果用Scale up解决方案，那么你就需要去买一个大缸，把所有沙、水草、布景、加热棒、温度计都从小缸里拿出来，重新布置到大缸。这个工程可不简单哦，不是十分钟八分钟能搞得定的，尤其水草，纠在一起很难分开(不过这跟迁移数据的工程复杂度比起来实在是毛毛雨啦，不值一提)。用Scale out方案，就相当于是你在这个小缸旁边接了一个同样的小缸，两个缸联通。鱼可以自动分散到两个缸，你也就省掉了上面提到的那一系列挪沙、水草、布景等的折腾了。
   - [系统扩展方式scale up和scale out](https://blog.csdn.net/truong/article/details/73056934)
2. 目的不同。分区的目的是为了将一个查询进行并行处理，这样所有节点能并行处理一个查询；Sharding是让每个节点尽量处理不同的查询。
3. 应用场景：分区适用于传统的企业应用，尤其OLAP（联机分析处理）的应用，基本上每个查询都需要访问大部分的数据；Sharding适用于云Web应用，特征是有大量的用户和查询，但每个查询访问到的元组非常少，Sharding可以将负载分散到多个物理节点上。
4. 可用性：对于分布式数据库基本上每个查询都需要所有节点的参与，如果某些节点down掉后，系统会大受影响；Sharding处理的应用一般只涉及到少数几个节点，故可用性上Sharding要好一些。
5. 分割粒度：分区一般只针对一个数据库内部进行分割；Sharding可以以数据库为粒度进行分割。

### 2.3 Neo4j 数据模型

Neo4j图数据库遵循**属性图模型**来存储和管理数据。

图形是一组节点和连接这些节点的关系。 图形以属性的形式将数据存储在节点和关系中。 属性是用于表示数据的键值对。

**属性图模型规则：**

- 表示节点，关系和属性中的数据
- 节点和关系都包含属性
- 关系连接节点
- 属性是**键值对**
- 节点用圆圈表示，关系用方向键表示
- 关系具有方向：单向和双向
- 每个关系包含“开始节点”或“从节点”和“到节点”或“结束节点”

**属性图数据模型中，关系应该是定向的。如果我们尝试创建没有方向的关系，那么它将抛出一个错误消息。Neo4j中关系也是具有方向性的，如果尝试创建没有方向的关系，Neo4j会抛出一个错误消息。**

Neo4j图数据库将其所有数据存储在节点和关系中。我们不需要任何额外的RRBMS数据库或无SQL数据库来存储Neo4j数据库数据。它**以图形的形式存储其数据的本机格式**。**Neo4j使用本机GPE（图形处理引擎）引擎来使用它的本机图存储格式。**

## 3. Lucene

**Neo4j和ES（分布式搜索引擎）都是基于Lucene的。**

- Lucene是apache下的一个开源的全文检索引擎工具包。它不是一个完整的全文检索引擎，而是一个全文检索引擎的架构。
- Lucene提供了完整的查询引擎和索引引擎，部分文本分析引擎。
- Lucene的目的是为软件开发人员提供一个简单易用的工具包，以方便的在目标系统中实现全文检索的功能，或是以此为基础建立一个完整的全文检索引擎。
- Lucene是一套用于全文检索和搜寻的开源程式库，由Java开发

**数据分类：**

- **结构化数据**：具有固定格式或有限长度的数据。结构化数据查询方法：
  - **数据库搜索：**使用sql语句进行查询，而且能很快的得到查询结果。
- **非结构化数据**：指不定长或无固定格式的数据，如邮件、word文档等磁盘上的文件。非结构化数据查询方法：
  - **顺序扫描法**：如果要查找内容包含某一个字符串的文件，就是一个文档一个文档地查看。对于每个文档，从头扫描到尾，如果此文档中包含此字符串，则此文档为我们要找的文件，接着查看下一个文件，直到扫描完全部的文件。
  - **全文检索**：将非结构化数据中的一部分信息提取处理，重新组织，使其变得有一定结构，然后对有一定结构的数据进行搜索，从而达到搜索相对较快的目的。这些从非结构化数据中提取出的然后重新组织的信息。称之为**索引**。

**全文检索（Full-text Search）：**

- 先“分词”创建索引，再对索引进行搜索的过程叫全文检索
  - 分词：将一段文字分成一个个单词
  - 虽然创建索引的过程非常耗时，但索引一旦创建就可以多次使用。全文检索主要处理的是查询，所以耗时间创建索引是值得的。
- 应用场景：对于数据量大、数据结构不固定的数据可采用全文检索方式搜索，比如百度、Google等搜索引擎、论坛站内搜索、电商网站站内搜索等。

### [Lucene实现全文检索的流程](https://www.cnblogs.com/xiaobai1226/p/7652093.html)

![Lucene索引和搜索流程](.\png\Lucene索引和搜索流程.png)

- 绿色表示索引过程，对要搜索的原始内容进行索引构建一个索引库，索引过程包括：
  - 确定原始内容（即要搜索的内容）
  - 采集文档
  - 创建文档
  - 分析文档
  - 索引文档

- 红色表示搜索过程，从索引库中搜索内容，搜索过程包括：
  - 用户通过搜索界面
  - 创建查询
  - 执行搜索，从索引库搜索
  - 渲染搜索结果

1. **创建索引**

   对文档索引的过程，将用户要搜索的文档内容进行索引，索引存储在索引库中。

   1.1 **获得原始文档**

   ​	原始文档是指要索引和搜索的内容。原始内容包括互联网上的网页、数据库中的数据、磁盘上的文件等。

   ​	从互联网、数据库、文件系统等获取需要搜索的原始信息的过程是**信息采集**，常用的技术是**爬虫**。

   1.2 **创建文档对象**

   ​	索引前先将原始内容创建成文档（Document），文档中包括一个一个的域（Field），域中存储内容。

   ​	可将一个文件当成一个Document，Document中包括一些Field。

   ![Document](.\png\Document.png)

   注意：

   ​	（1）每个Document可以有多个Field

   ​	（2）不同的Document可以有不同的Field

   ​	（3）同一个Document可以有相同的Field（域名和域值都相同）

   ​	（4）每个文档都有一个唯一的编号，文档ID

   1.3 **分析文档**

   ​	将原始内容创建为包含域（Field）的文档（Document），需要再对域中的内容进行分析。分析的过程是对原始文档提取单词，将字母转为小写，去除标点符号，去除停用词等，生成最终的语汇单元，可将词汇单元理解为一个个的单词。

   > 原始文档内容：
   >
   > Lucene is a Java full-text search engine. 
   >
   > 分析后得到的语汇单元：
   >
   > lucene、java、full、search、engine

   ​	每个单词叫一个Term，不同的域中拆分出来的相同的单词是不同的Term。Term中包含两部分，一部分是文档的域名，一部分是单词的内容。例如：文件名中包含apache和文件内容中包含的apache是不同的term。

   1.4 **创建索引**

   ​	对所有文档分析得出的语汇单元进行索引，索引的目的是为了搜索，最终要实现**只搜索被索引的语汇单元从而找到Document**。

   ​	索引库如下图所示：

   ![索引](.\png\索引.png)

   ​	创建和使用索引流程，如下图所示：

   ![索引流程](.\png\索引流程.png)

   注意：

   ​	（1）是对语汇单元创建索引，通过词语找文档，这种索引结构叫**倒排索引结构**。

   ​	（2）传统方法正排索引是根据文件找到该文件的内容，再文件内容中匹配搜索关键字。正排索引在大数据量的情景下，搜索慢。

   ​	（3）倒排索引结构根据内容（词语）找文档，包括索引和文档两部分，索引即词汇表，规模较小，文档集合较大。结构如下图所示：

   ![倒排索引png](.\png\倒排索引.png)

2. **查询索引**

   查询搜索也是搜索的过程。即用户输入关键字，从索引中进行搜索的过程。根据关键字搜索索引，根据索引找到对应的文档，从而找到要搜索的内容。

   2.1 **用户查询接口**

   ​	全文检索系统提供用户搜索的界面供用户提交搜索的关键字，搜索完成展示搜索结果。

   ​	Lucene不提供制作用户搜索界面的功能，需要根据自己的需求开发搜索界面。

   2.2 **创建查询**

   ​	用户输入查询关键字执行搜索之前需要先创建一个查询对象，查询对象中可以指定查询要搜索的Field文档域、查询关键字等，查询对象中可以指定查询要搜索的Field文档域、查询关键字等，查询对象会生成具体的查询语法。

   2.3 **执行查询**

   ​	根据查询语法在倒排索引词典表中分别找出对应搜索词的索引，从而找到索引所链接的文档链表。

   > 搜索语法为`“fileName:lucene”`表示搜索出`fileName`域中包含`Lucene`的文档。
   >
   > 搜索过程就是在索引上查找域为`fileName`，并且关键字为`Lucene`的`term`，并根据`term`找到文档id列表。

   ​	可通过两种方法创建查询对象：

    1. 使用Lucene提供Query子类

       - Query是一个抽象类，lucene提供了很多查询对象，比如TermQuery项精确查询，NumericRangeQuery数字范围查询等。

       - ```java
         Query query = new TermQuery(new Term("name", "lucene"));
         ```

   2. 使用QueryParse解析查询表达式

      -  QueryParse会将用户输入的查询表达式解析成Query对象实例。

      - ```java
        QueryParser queryParser = new QueryParser("name", new IKAnalyzer());
        Query query = queryParser.parse("name:lucene");
        ```

   ​    **中文分词器：**

   ​	Lucene自带的中文分词器

   - StandardAnalyzer：（标准分词器，也是前面例子中使用的分词器）
     - 单字分词
     - 如：“我爱中国”，分成 “我”、“爱”、“中”、“国”。
   - CJKAnalyzer
     - 二分法分词：按两个字进行切分
     - 如：“我是中国人”，分成 “我是”、“是中”、“中国”“国人”。
   - SmartChineseAnalyzer
     - 对中文支持较好，但扩展性差，扩展词库，禁用词库和同义词库等不好处理

   ​    第三方中文分词器

   - IK-analyzer

3. **删除索引**

   - 删除全部索引（将索引目录的索引信息全部删除，无法恢复）
   - 指定查询条件删除

4. **修改索引库**

   - 更新的原理是先删除再添加

## 4. Neo4j CQL

