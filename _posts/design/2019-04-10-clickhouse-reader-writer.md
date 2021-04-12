---
layout: page-fullwidth
title: "DataX的Clickhouse读写插件"
subheadline: "安全大数据"
teaser: "DataX 是阿里巴巴集团内被广泛使用的离线数据同步工具/平台，实现包括 MySQL、Oracle、SqlServer、Postgre、HDFS、Hive、ADS、HBase、TableStore(OTS)、MaxCompute(ODPS)、DRDS 等各种异构数据源之间高效的数据同步功能。"
categories:
    - design
# Styling
#
header:
   image_fullwidth: "header_roadmap_2.jpg"
image:
#   thumb: totoro_typography-thumb.jpg
    thumb: icons/tools.png  
    homepage: mediaplayer_js-home.jpg
    caption: 飘
    caption_url: http://candylab.net
mediaplayer: true

---
<!--more-->


<script type="text/javascript">var jd_union_pid="3002832436";var jd_union_euid="";</script><script type="text/javascript" src="//ads-union.jd.com/static/js/union.js"></script>

<div class="row">
<div class="medium-4 medium-push-8 columns" markdown="1">
{% include _sidebar_tags.html %}
<div class="panel radius" markdown="1">
**目录**
{: #toc }
*  TOC
{:toc}
</div>
</div><!-- /.medium-4.columns -->


<div class="medium-8 medium-pull-4 columns" markdown="1">

{% include _improve_content.html %}



作者：一剑飘雪

# ClickHouseReader 插件文档


___



## 1 快速介绍

ClickHouseReader插件实现了从ClickHouse读取数据。在底层实现上，ClickHouseReader通过JDBC连接远程ClickHouse数据库，并执行相应的sql语句将数据从ClickHouse库中SELECT出来。

**不同于其他关系型数据库，ClickHouseReader不支持FetchSize.（截止ClickHouse-jdbc版本0.1.48为止）**

## 2 实现原理

简而言之，ClickHouseReader通过JDBC连接器连接到远程的ClickHouse数据库，并根据用户配置的信息生成查询SELECT SQL语句，然后发送到远程ClickHouse数据库，并将该SQL执行返回结果使用DataX自定义的数据类型拼装为抽象的数据集，并传递给下游Writer处理。

对于用户配置Table、Column、Where的信息，ClickHouseReader将其拼接为SQL语句发送到ClickHouse数据库；对于用户配置querySql信息，ClickHouseReader直接将其发送到ClickHouse数据库。


## 3 功能说明

### 3.1 配置样例

* 配置一个从ClickHouse数据库同步抽取数据到本地的作业:

```
{
    "job": {
        "setting": {
            "speed": {
                 "channel": 3
            },
            "errorLimit": {
                "record": 0,
                "percentage": 0.02
            }
        },
        "content": [
            {
                "reader": {
                    "name": "clickhousereader",
                    "parameter": {
                        "username": "root",
                        "password": "root",
                        "column": [
                            "id",
                            "name"
                        ],
                        "splitPk": "db_id",
                        "connection": [
                            {
                                "table": [
                                    "table"
                                ],
                                "jdbcUrl": [
     "jdbc:clickhouse://127.0.0.1:8123/default"
                                ]
                            }
                        ]
                    }
                },
               "writer": {
                    "name": "streamwriter",
                    "parameter": {
                        "print":true
                    }
                }
            }
        ]
    }
}

```

* 配置一个自定义SQL的数据库同步任务到本地内容的作业：

```
{
    "job": {
        "setting": {
            "speed": {
                 "channel":1
            }
        },
        "content": [
            {
                "reader": {
                    "name": "clickhousereader",
                    "parameter": {
                        "username": "root",
                        "password": "root",
                        "connection": [
                            {
                                "querySql": [
                                    "select db_id,on_line_flag from db_info where db_id < 10;"
                                ],
                                "jdbcUrl": [
                                    "jdbc:clickhouse://127.0.0.1:8123/default"
                                ]
                            }
                        ]
                    }
                },
                "writer": {
                    "name": "streamwriter",
                    "parameter": {
                        "print": false,
                        "encoding": "UTF-8"
                    }
                }
            }
        ]
    }
}
```


### 3.2 参数说明

* **jdbcUrl**

    * 描述：描述的是到对端数据库的JDBC连接信息，使用JSON的数组描述，并支持一个库填写多个连接地址。之所以使用JSON数组描述连接信息，是因为阿里集团内部支持多个IP探测，如果配置了多个，ClickHouseReader可以依次探测ip的可连接性，直到选择一个合法的IP。如果全部连接失败，ClickHouseReader报错。 注意，jdbcUrl必须包含在connection配置单元中。对于阿里集团外部使用情况，JSON数组填写一个JDBC连接即可。

        jdbcUrl按照ClickHouse官方规范，并可以填写连接附件控制信息。具体请参看[ClickHouse官方文档](https://github.com/yandex/clickhouse-jdbc)。

    * 必选：是 <br />

    * 默认值：无 <br />

* **username**

    * 描述：数据源的用户名 <br />

    * 必选：是 <br />

    * 默认值：无 <br />

* **password**

    * 描述：数据源指定用户名的密码 <br />

    * 必选：是 <br />

    * 默认值：无 <br />

* **table**

    * 描述：所选取的需要同步的表。使用JSON的数组描述，因此支持多张表同时抽取。当配置为多张表时，用户自己需保证多张表是同一schema结构，ClickHouseReader不予检查表是否同一逻辑表。注意，table必须包含在connection配置单元中。<br />

    * 必选：是 <br />

    * 默认值：无 <br />

* **column**

    * 描述：所配置的表中需要同步的列名集合，使用JSON的数组描述字段信息。用户使用\*代表默认使用所有列配置，例如['\*']。

      支持列裁剪，即列可以挑选部分列进行导出。

      支持列换序，即列可以不按照表schema信息进行导出。

      支持常量配置，用户需要按照ClickHouse SQL语法格式:
      ["id", "\`table\`", "1", "'bazhen.csy'", "null", "toDate(trade_date) + 1", "2.3" , "true"]
      id为普通列名，\`table\`为包含保留在的列名，1为整形数字常量，'bazhen.csy'为字符串常量，null为空指针，toDate(trade_date) + 1为表达式，2.3为浮点数，true为布尔值。

    * 必选：是 <br />

    * 默认值：无 <br />

* **splitPk**

    * 描述：ClickHouseReader进行数据抽取时，如果指定splitPk，表示用户希望使用splitPk代表的字段进行数据分片，DataX因此会启动并发任务进行数据同步，这样可以大大提供数据同步的效能。

      推荐splitPk用户使用表主键，因为表主键通常情况下比较均匀，因此切分出来的分片也不容易出现数据热点。

      目前splitPk仅支持整形、字符串数据切分，`不支持浮点、日期等其他类型`。如果用户指定其他非支持类型，ClickHouseReader将报错！

      如果splitPk不填写，包括不提供splitPk或者splitPk值为空，DataX视作使用单通道同步该表数据。

    * 必选：否 <br />

    * 默认值：空 <br />

* **where**

    * 描述：筛选条件，ClickHouseReader根据指定的column、table、where条件拼接SQL，并根据这个SQL进行数据抽取。在实际业务场景中，往往会选择当天的数据进行同步，可以将where条件指定为gmt_create > $bizdate 。注意：不可以将where条件指定为limit 10，limit不是SQL的合法where子句。<br />

          where条件可以有效地进行业务增量同步。如果不填写where语句，包括不提供where的key或者value，DataX均视作同步全量数据。

    * 必选：否 <br />

    * 默认值：无 <br />

* **querySql**

    * 描述：在有些业务场景下，where这一配置项不足以描述所筛选的条件，用户可以通过该配置型来自定义筛选SQL。当用户配置了这一项之后，DataX系统就会忽略table，column这些配置型，直接使用这个配置项的内容对数据进行筛选，例如需要进行多表join后同步数据，使用select a,b from table_a join table_b on table_a.id = table_b.id <br />

     `当用户配置querySql时，ClickHouseReader直接忽略table、column、where条件的配置`，querySql优先级大于table、column、where选项。

    * 必选：否 <br />

    * 默认值：无 <br />


### 3.3 类型转换

目前ClickHouseReader支持大部分ClickHouse类型，但也存在部分个别类型没有支持的情况，请注意检查你的类型。

下面列出ClickHouseReader针对ClickHouse类型转换列表:


| DataX 内部类型| ClickHouse 数据类型    |
| -------- | -----  |
| Long     |Uint8,Uint16,Uint32,Uint64,Int8,Int16,Int32,Int64,Enum8,Enum16|
| Double   |Float32,Float64,Decimal|
| String   |String,FixedString(N)|
| Date     |Date, Datetime |
| Boolean  |UInt8 类型，取值限制为 0 或 1  |
| Bytes    |String|



请注意:

* `除上述罗列字段类型外，其他类型均不支持，如Array、Nested等`。

## 4 性能报告

### 4.1 环境准备

#### 4.1.1 数据特征
建表语句：

```sql
CREATE TABLE `t_trade_record` (
    `id` INT ( 11 ) NOT NULL AUTO_INCREMENT,
    `trade_no` BIGINT ( 20 ) NOT NULL,
    `order_no` BIGINT ( 20 ),
    `pair_id` VARCHAR ( 128 ),
    `belonger` VARCHAR ( 128 ),
    `login_name` VARCHAR ( 128 ),
    `belonger_type` VARCHAR ( 32 ),
    `trade_date` date,
    `trade_time` TIMESTAMP(0),
    `bs_flag` VARCHAR ( 8 ),
    `price` DECIMAL ( 16, 8 ),
    `quantity` INT ( 11 ),
    `income_asset_code` VARCHAR ( 128 ),
    `income_fee` DECIMAL ( 16, 8 ),
    `update_time` TIMESTAMP(0) NULL,
    `insert_time` TIMESTAMP(0) NULL,
    PRIMARY KEY ( `id` ) USING BTREE,
    UNIQUE INDEX `index_trade_no` ( `trade_no` ) USING BTREE 
) ENGINE = INNODB CHARACTER SET = utf8;
```

插入记录类似于：

```
INSERT INTO `t_match_record`(`id`, `trade_no`, `order_no`, `pair_id`, `belonger`, `login_name`, `belonger_type`, `trade_date`, `trade_time`, `bs_flag`, `price`, `quantity`, `income_asset_code`, `income_fee`, `update_time`, `insert_time`) VALUES (141135300, 116615441, 115754819, 'ETH-USDT', '2357246974', '131****4807', '0', '2019-04-21', '2019-04-21 00:34:19', 'B', 113.02000000, 0, 'C10001', 0.00001110, '2018-12-21 00:35:00', '2018-12-21 00:35:00');
INSERT INTO `t_match_record`(`id`, `trade_no`, `order_no`, `pair_id`, `belonger`, `login_name`, `belonger_type`, `trade_date`, `trade_time`, `bs_flag`, `price`, `quantity`, `income_asset_code`, `income_fee`, `update_time`, `insert_time`) VALUES (141135299, 116615440, 115754793, 'ETH-USDT', '2357246974', '131****4807', '0', '2019-04-21', '2019-04-21 00:34:19', 'S', 113.02000000, 0, 'C10002', 0.00037297, '2018-12-21 00:35:00', '2018-12-21 00:35:00');
```

#### 4.1.2 机器参数

* 执行DataX的机器参数为:
    1. cpu: 4核 Intel(R) Core(TM) i5-8600 CPU @ 3.10GHz
    2. mem: 4GB
    3. net: 千兆双网卡
    4. disc: DataX 数据不落磁盘，不统计此项

* ClickHouse数据库机器参数为:
    虚拟机配置如下
    1. cpu: 2物理2逻辑  Intel(R) Core(TM) i5-8600 CPU @ 3.10GHz
    2. mem: 2G
    3. net: 千兆双网卡

#### 4.1.3 DataX jvm 参数

    -Xms1024m -Xmx1024m -XX:+HeapDumpOnOutOfMemoryError


### 4.2 测试报告

#### 4.2.1 单表测试报告


| 通道数| 是否按照主键切分| DataX速度(Rec/s)|DataX流量(MB/s)| DataX机器网卡进入流量(MB/s)|DataX机器运行负载|DB网卡流出流量(MB/s)|DB运行负载|
|--------|--------| --------|--------|--------|--------|--------|--------|
|1| 是 | 192299 | 21.82 | 36| 0.6 | 38| 0.6 |
|2| 是 | 461519 | 52.37 | 92| 0.75| 94| 0.72 |
|4| 是 | 480749 | 54.55 | 96| 0.9 | 99| 0.92 |

说明：

1. 这里的单表，主键类型为 bigint(20),范围为：1231425-116615530，从主键范围划分看，数据分布均匀。
2. 对单表如果没有安装主键切分，那么配置通道个数不会提升速度，效果与1个通道一样。
3. 由于机器性能限制，达到2通道时，CPU已到100%，故4通道时，速度并没有增长

#### 4.2.2 分表测试报告

##### 应机器原因，暂时没有做测试

## 5 约束限制

### 5.1 一致性约束

ClickHouse在数据存储划分中属于RDBMS系统，对外可以提供强一致性数据查询接口。例如当一次同步任务启动运行过程中，当该库存在其他数据写入方写入数据时，ClickHouseReader完全不会获取到写入更新数据，这是由于数据库本身的快照特性决定的。关于数据库快照特性，请参看[MVCC Wikipedia](https://en.wikipedia.org/wiki/Multiversion_concurrency_control)

上述是在ClickHouseReader单线程模型下数据同步一致性的特性，由于ClickHouseReader可以根据用户配置信息使用了并发数据抽取，因此不能严格保证数据一致性：当ClickHouseReader根据splitPk进行数据切分后，会先后启动多个并发任务完成数据同步。由于多个并发任务相互之间不属于同一个读事务，同时多个并发任务存在时间间隔。因此这份数据并不是`完整的`、`一致的`数据快照信息。

针对多线程的一致性快照需求，在技术上目前无法实现，只能从工程角度解决，工程化的方式存在取舍，我们提供几个解决思路给用户，用户可以自行选择：

1. 使用单线程同步，即不再进行数据切片。缺点是速度比较慢，但是能够很好保证一致性。

2. 关闭其他数据写入方，保证当前数据为静态数据，例如，锁表、关闭备库同步等等。缺点是可能影响在线业务。

### 5.2 增量数据同步

ClickHouseReader使用JDBC SELECT语句完成数据抽取工作，因此可以使用SELECT...WHERE...进行增量数据抽取，方式有多种：

* 数据库在线应用写入数据库时，填充modify字段为更改时间戳，包括新增、更新、删除(逻辑删)。对于这类应用，ClickHouseReader只需要WHERE条件跟上一同步阶段时间戳即可。
* 对于新增流水型数据，ClickHouseReader可以WHERE条件后跟上一阶段最大自增ID即可。

对于业务上无字段区分新增、修改数据情况，ClickHouseReader也无法进行增量数据同步，只能同步全量数据。

### 5.3 Sql安全性

ClickHouseReader提供querySql语句交给用户自己实现SELECT抽取语句，ClickHouseReader本身对querySql不做任何安全性校验。这块交由DataX用户方自己保证。






# DataX ClickHouseWriter


---

## 1 快速介绍

数据导入clickhousewriter的插件

## 2 实现原理

使用clickhousewriter的官方jdbc接口， 批量把从reader读入的数据写入ClickHouse

## 3 功能说明

### 3.1 配置样例

#### job.json

```
{
  "job": {
    "setting": {
        "speed": {
            "channel": 2
        }
    },
    "content": [
      {
        "reader": {
          ...
        },
        "writer": {
            "name": "clickhousewriter",
            "parameter": {
                "username": "default",
                "password": "zifei123",
                "column":["belonger","belonger_type","bs_flag","id","income_asset_code","income_fee","insert_time","logName","order_no","pair_id","price","quantity","trade_date","trade_no","trade_time","update_time"],
                "connection": [
                    {
                        "jdbcUrl": "jdbc:clickhouse://127.0.0.1:8123/default",
                        "table":["XXX"]
                    }
                ]
            }
        }
      }
    ]
  }
}
```

#### 3.2 参数说明

* jdbcUrl
 * 描述：ClickHouse的连接地址，目前支持多数据源并行导入，支持随机负载均衡,其格式为:jdbc:clickhouse://ip1:8123,ip2:8123/database
 * 必选：是
 * 默认值：无

* username
 * 描述：导入数据源的用户名
 * 必选：是
 * 默认值：空

* password
 * 描述：导入数据源的密码
 * 必选：是
 * 默认值：空

* batchSize
 * 描述：每次批量数据的条数
 * 必选：否
 * 默认值：2048

* trySize
 * 描述：失败后重试的次数
 * 必选：否
 * 默认值：30

* column
 * 描述：elasticsearch所支持的字段类型，样例中包含了全部
 * 必选：是



## 4 性能报告

### 4.1 环境准备

* 总数据量 1.2亿条数据, 每条1.2kb
* 1个replica,单机模式

#### 4.1.1 数据特征

建表语句：

```sql
CREATE TABLE default.t_match_record (
       id                      UInt32,
       trade_no                UInt32,
       order_no                UInt32,
       pair_id                 String,
       belonger                String,
       login_name              String,
       belonger_type           String,
       trade_date              Date,
       trade_time              DateTime,
       bs_flag                 String,
       price                   Decimal64(8),
       quantity                UInt32,
       income_asset_code       String,
       income_fee              Decimal64(8),
       insert_time             DateTime,
       update_time             DateTime
)Engine=MergeTree(trade_date,(belonger,pair_id,trade_no,trade_date),8192);
```

插入记录类似于：

```
INSERT INTO `default`.`t_match_record`(`id`, `trade_no`, `order_no`, `pair_id`, `belonger`, `login_name`, `belonger_type`, `trade_date`, `trade_time`, `bs_flag`, `price`, `quantity`, `income_asset_code`, `income_fee`, `update_time`, `insert_time`) VALUES (141135300, 116615441, 115754819, 'ETH-USDT', '2357246974', '131****4807', '0', '2019-04-21', '2019-04-21 00:34:19', 'B', 113.02000000, 0, 'C10001', 0.00001110, '2018-12-21 00:35:00', '2018-12-21 00:35:00');
INSERT INTO `default`.`t_match_record`(`id`, `trade_no`, `order_no`, `pair_id`, `belonger`, `login_name`, `belonger_type`, `trade_date`, `trade_time`, `bs_flag`, `price`, `quantity`, `income_asset_code`, `income_fee`, `update_time`, `insert_time`) VALUES (141135299, 116615440, 115754793, 'ETH-USDT', '2357246974', '131****4807', '0', '2019-04-21', '2019-04-21 00:34:19', 'S', 113.02000000, 0, 'C10002', 0.00037297, '2018-12-21 00:35:00', '2018-12-21 00:35:00');

```

#### 4.1.2 机器参数
虚拟机配置如下
1. cpu: 2物理2逻辑  Intel(R) Core(TM) i5-8600 CPU @ 3.10GHz
2. mem: 2G
3. net: 千兆双网卡

#### 4.1.3 DataX jvm 参数

-Xms1024m -Xmx1024m -XX:+HeapDumpOnOutOfMemoryError

### 4.2 测试报告

| 通道数|  批量提交行数| DataX速度(Rec/s)|DataX流量(MB/s)|
|--------|--------| --------|--------|
| 1| 8192| 115379| 13.09|
| 2| 2048| 144224| 16.37|
| 2| 4096| 151815| 17.23|
| 2| 8192| 162506| 18.44|
| 4| 2048| 151815| 17.23|
| 4| 4096| 172208| 19.54|
| 4| 8192| 202420| 22.97|


### 4.3 测试总结

* 最好的结果是1-2通道，每次传8192（对当前笔者测试坏境配置而言，瓶颈在CPU上），如果单条数据很大， 请适当减少批量数，防止oom
* 通过升级硬件，单机写入300K/S不是问题，甚至500K/S，而且ClickHouse也是分布式的，多设置几个分片就可以水平扩展，此时还可以并行写入
* 当通道为4，批量提交为8192时，笔者测试机器已压榨到极限：物理机CPU100%，磁盘占用100%，网卡流量峰值为360Mbps。

### 4.4 导入建议
* 数据应该以尽量大的batch进行写入，如每次写入100,000行，根据机器性能，尝试增加通道数
* 数据最好跟ClickHouse分区Key分组排序，这样有更好的插入性能

项目地址：[DataX](https://github.com/kuangye098/DataX)
