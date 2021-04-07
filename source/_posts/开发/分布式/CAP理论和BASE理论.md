---
title: CAP理论和BASE理论
tags:
  - 分布式
categories:
  - 开发
  - 基础知识
  - 分布式
abbrlink: 7ae49b47
date: 2021-04-06 15:56:26
---


说起分布式肯定绕不开两个理论：CAP理论和BASE理论。限于水平有限，本篇文章仅作记录用。



<!-- more -->



# CAP理论
C即一致性，A即可用性，P即分区容错性。
CAP理论指出了三者中只能同时做到其中的两个，无法同时满足三个要求。
CAP理论是针对分布式数据库环境提出的，因此P属性是必须的。如果选择了A，那么分区间的数据可能会不一致；如果选择了C，则为了保持数据一致性而导致服务不可用。



## 拓展

Spring cloud中的eureka 属于AP，zookeeper属于CP。



# BASE理论
BA（基本可用，Basically Available）、S（软状态，Soft-state）、C（最终一致性，Eventually Consistent）
BASE理论是由CAP理论发展而来，是一致性和可用性权衡的结果，即牺牲一致性来满足系统的可用性，然后在后续的过程中达到最终一致。