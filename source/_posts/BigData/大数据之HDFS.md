---
title: HDFS高可用之HA架构
tags:
  - BigData
  - HDFS
  - 大数据
  - 数据仓库
categories:
  - - BigData
---

### 一、NameNode故障导致的问题

1.NameNode机器宕机，导致整个集群不可用，重启NameNode后才可使用，

2.NameNode机器软硬件升级，导致集群不可用

以上两种情况会导致HDFS系统无法使用，为解决以上的问题Hadoop2.0推出了HDFS的高可用（HA）解决方案。



### 二、HA高可用

HDFS的HA由两个NameNode组成，一个是Active状态，另一个Standby。

所有来自客户端的请求由Active状态的NameNode处理，Standby只同步Active状态NameNode的状态，一旦主节点（Active）发生故障就会借助Zookeeper实现自动的主备选举和切换，这样备用主节点就能立即接替故障节点并提供服务，而用户感觉不到明显的系统中断，使得整个系统更加高效可靠。

共享存储系统即JournalNode集群，是实现NameNode高可用的最关键的部分，它保存了Name Node在运行过程中所产生的HDFS元数据，主备NameNode通过JornalNode实现元数据同步，在切换主备时，新的主NameNode在确认元数据完全同步之后才会提供服务，除此之外还需要共享HDFS数据块和DataNode之间的映射关系，DATaNode向主备NameNode上报数据块位置信息。
