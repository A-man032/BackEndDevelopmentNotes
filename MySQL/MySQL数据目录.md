# MySQL的数据目录

## 数据库和文件系统的关系

InnoDB、MyISAM这样的存储引擎是把表存储在磁盘上，操作系统使用文件系统管理磁盘。所以像InnoDB、MyISAM这样的存储引擎都是把数据存储在文件系统上。

当我们想读取数据时，这些存储引擎会从文件系统中把数据读出来返回给我们；当我们想写入数据时，这些存储引擎会把这些数据又写回文件系统。

## MySQL数据目录

MySQL服务器程序在启动时，会到文件系统的某个目录下加载一些数据，之后在运行过程中产生的数据也会存储到这个目录下的某些文件，这个目录就是数据目录。

**安装目录和数据目录**：

- 安装目录里存储了许多用来控制客户端程序和服务端程序的命令
- 数据目录里存储了MySQL在运行过程中产生的数据

**查看数据目录路径**：

数据目录对应着系统变量`datadir`

```sql
SHOW VARIABLES LIKE 'datadir';
```

## 数据库在文件系统中的表示

每个数据库都对应数据目录下的一个子目录，或者说是对应一个文件夹。每当我们新建一个数据库时，MySQL会做两件事：

- 在数据目录下创建一个与数据库名同名的子目录（即文件夹）；
- 在与该数据库名同名的子目录下创建一个名为db.opt的文件，这个文件包含了该数据库的一些属性，比如该数据库的字符集和比较规则。

## 表在文件系统中的表示

每个表的信息分为两种：

- 表结构的定义
- 表中的数据

InnoDB、MyISAM都在数据目录下对应的数据库子目录中创建了一个专门用于描述表结构的文件，`表名.frm`。`.frm`的文件是以二进制格式存储的，若直接打开会显示乱码。

### InnoDB是如何存储表数据的？

- InnoDB使用**页**为单位来管理存储空间，默认页的大小为16KB。
- InnoDB中，每个索引都对应着一棵B+树，该B+树的每个节点都是一个数据页。**数据页之间没有必要时物理连续的，因为数据页之间有双向链表来维护这些页的顺序**。
- InnoDB的聚餐索引的叶子节点存储了完整的用户记录。

**表空间（文件空间）**：对应文件系统上一个或多个真实文件（不同表空间对应的文件数量可能不同）。每一个表空间可以被划分为很多个页，表数据就存放在某个表空间下的某些页中。

- **系统表空间**：可对应文件系统上一个或多个实际的文件。在默认情况下，InnoDB会在数据目录下创建一个名为`ibdata1`的文件，大小为12MB。这个文件就是对应的系统表空间在文件系统上的表示，是自扩展文件。
- **独立表空间**：MySQL5.6.6及之后的版本中，InnoDB不再默认把各个表的数据存储到系统表空间中，而是为每个表建立一个独立表空间。在使用独立表空间存储表数据时，会在该表所属数据库对应的子目录下创建一个表示该独立表空间的文件，`表名.ibd`。
  - 启动项`innodb_file_per_table`可以指定使用系统表空间还是独立表空间来存储数据。0表示使用系统表空间，1表示使用独立表空间。只对新建的表起作用，对于已经分配表空间的表并不起作用。
- 其他类型的表空间：如通用表空间、undo表空间等。

### MyISAM是如何存储表数据的？

MyISAM中的索引相当于全部都是二级索引，该存储引擎中的数据和索引是分开存放的。所以在文件系统中也是使用不同的文件来存储数据文件和索引文件的。

MyISAM中没有表空间的定义，表的数据和索引都存放到对应的数据库子目录下。假设test表使用MyISAM存储引擎，那么它所在的数据库目录下会为test表创建以下3个文件：

- test.frm
- test.MYD：表的数据文件，插入的用户记录
- test.MYI：表的索引文件，为该表创建的索引都会放到这个文件中

### 其他文件

以上提到的都是用户自己存储的数据，除此之外，数据目录下还包含了一些确保程序更好运行的额外文件。

- 服务器进程文件：每运行一个MySQL服务器程序，都意味着启动一个进程。MySQL服务器会把自己的进程ID写入这个文件中。
- 服务器日志文件：在服务器运行期间，会产生各种日志，如常规插叙日志、错误日志、二进制日志、redo日志等。
- SSL和RSA证书与密钥文件：主要是为了客户端和服务器安全通信而创建的一些文件。

## 文件系统对数据库的影响

- 数据库名称和表名称不得超过文件系统所允许的最大长度。
- 特殊字符：为避免数据库名和表名出现某些特殊字符而造成文件系统不支持的情况，MySQL会把数据库名和表名中所有除数字和拉丁字母以外的任何字符在文件名中映射成@+编码值的形式，并将其作为文件名。
- 文件长度受文件系统最大长度的限制：对InnoDB来说，每个表的数据都会被存储到与表名同名的.ibd文件中，这些文件会随着表中记录的增加而增大，它们的大小受限于文件系统支持的最大文件大小。

