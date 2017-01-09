# DisJob

DisJob 是一个基于Quartz、Netty、ZooKeeper的分布式rpc调度job框架。

#一、项目背景
disjob分布式任务调度概述<br/>
公司现有定时任务（job）有几千个，基于crontab实现分散于很多系统中，有时会发生故障，原因有多种，job执行过程中本身发生故障，系统进程挂 掉，系统所在服务器宕机；对于需要执行几个小时的大型job没有分片功能，或者不能非常方便的分片。因此disjob调度中心是为解决这些问题而设计，初步计 划要实现的功能如下：<br/>
        1、防单点故障：业务系统的单点故障以及调度中心本身的单点故障<br/>
             应用服务器必须要两台及以上才能防单点故障<br/>
        2、job本身发生故障时可通过邮件/短信通知相关人员<br/>
        3、job执行进程僵死时，可通过job管理平台kill进程<br/>
        4、对于无状态任务，可提供并发执行功能，对于有状态任务，严格按照先后顺序执行，且不会重复执行<br/>
        5、提供web管理平台，将所有定时任务集中管理，可手动提交、停止任务、设置任务执行的频率<br/>
             停止任务、设置任务频率均要在下次任务执行时才能生效，也就是说正在执行的任务不能停止，设置的新的执行频率也不会即时生效<br/>
        6、提供任务监控功能，即时生成任务执行进度报告<br/>
             需要提供日志数据，对定时的任务的业务代码有一定侵入<br/>
        7、任务执行发生异常时的异常信息，且可提供重试的配置项<br/>
             需要任务发送异常信息到调度中心或抛出异常<br/>
        8、大型任务的分片功能，假设要在A库处理数百万条数据，可将任务分片到多个系统上同时执行<br/>
             要实现分片功能必须要多台服务器（第一个版本没有实现）<br/>
        你们需要做的：<br/>
        1、基于RPC（远程过程调用或者说远程方法调用）框架提供定时任务接口，框架和基础方法我们提供，方便快捷<br/>
        2、使用我们提供的网络API给调度中心发送日志消息，会一定程度侵入业务代码，只是增加代码，不会改变原有代码的逻辑<br/>
      
  
#二、总体架构
  
  ![](https://github.com/huangyiminghappy/DisJob/blob/master/imgs/%E6%9E%B6%E6%9E%84%E5%9B%BE.png)

  
#三、ZooKeeper数据存储模型

  ![](https://github.com/huangyiminghappy/DisJob/blob/master/imgs/zookeeper%E6%95%B0%E6%8D%AE%E6%A8%A1%E5%9E%8B.png)
  
  ![](https://github.com/huangyiminghappy/DisJob/blob/master/imgs/%E6%95%B0%E6%8D%AE%E6%A8%A1%E5%9E%8B%E5%9B%BE.png)
  
 #四、主要流程图  

  ![](https://github.com/huangyiminghappy/DisJob/blob/master/imgs/%E8%B0%83%E5%BA%A6%E4%B8%AD%E5%BF%83%E6%B5%81%E7%A8%8B%E5%9B%BE.png)
