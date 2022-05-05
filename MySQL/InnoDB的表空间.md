# InnoDB的表空间

表空间是一个抽象概念：

- 对于系统表空间来说，对应着文件系统中一个或多个实际文件。
- 对于每个独立表空间来说，对于着文件系统中一个名为“表名.ibd”的实际文件。

> 表空间可以看作是被切分为很多页的池子，当想为某个页插入一条记录时，就从池子中捞出一个对应的页把数据写进去。

## 页面类型

InnoDB是以**页**为单位管理存储空间的。

聚簇索引和其他二级索引都是以B+树的形式保存到表空间中，B+树的节点就是数据页。这个数据页的类型是`FIL_PAGE_INDEX`。

常用的页面类型：

|          Name           | Hexadecimal (十六进制) |       Description       |
| :---------------------: | :--------------------: | :---------------------: |
|   FIL_PAGE_ALLOCATED    |         0x0000         |   最新分配，还未使用    |
|    FIL_PAGE_UNDO_LOG    |         0x0002         |       undo日志页        |
|     FIL_PAGE_INODE      |         0x0003         |      存储段的信息       |
| FIL_PAGE_IBUF_FREE_LIST |         0x0004         |  Change Buffer空闲列表  |
|  FIL_PAGE_IBUF_BITMAP   |         0x0005         | Change Buffer的一些属性 |
|    FIL_PAGE_TYPE_SYS    |         0x0006         |    存储一些系统数据     |
|  FIL_PAGE_TYPE_TRX_SYS  |         0x0007         |      事务系统数据       |
|  FIL_PAGE_TYPE_FSP_HDR  |         0x0008         |     表空间头部信息      |
|   FIL_PAGE_TYPE_XDES    |         0x0009         |    存储区的一些属性     |
|   FIL_PAGE_TYPE_BLOB    |         0x000A         |         溢出页          |
|     FIL_PAGE_INDEX      |         0x45BF         | 索引页（也就是数据页）  |

## 页面通用部分



