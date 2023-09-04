##################
计算节点
##################

.. note:: 本章介绍ChinaSRC-P计算节点情况

数据中心原型系统的文件系统包括：

节点类型	节点数量	CPU 详情	超线程	总 CPU 核心	内存	内存/核心	备注
管理节点	1	2 x E5-2640 v3, 2.60 GHz, 8 cores	是	32	64 GB	2 GB/core	单用户限制内存 4 GB
CPU 节点	4	2 x E5-2680 v3, 2.50 GHz, 12 cores	是	48	128 GB	2.67 GB/core	---
GPU 节点	1	2 x E5-2680 v4, 2.40 GHz, 14 cores	是	56	128 GB	2.28 GB/core	NVIDIA Tesla P100 x 2, CUDA 9.0


三个分区。

/home和/ssd分区主要用于用户个人数据和结果数据存储，
/o9000主要用于处理数据的长期存储。
/ibo9000主要用于存储原始观测数据。

目前，ChinaSRC-P系统存储了约2PB左右的数据，
包括MWA、ASKAP和VLBI望远镜的观测数据。

主要数据分组情况：MWA/GLEAM组、 MWA/Pulsar组和ASKAP组。

如果需要使用这些分组里面的数据，或需要上传和存储大量数据，请发邮件到 shaoska@shao.ac.cn  咨询。