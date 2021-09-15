## FATE-SERVING 2.1 新增特性： 
```text
1.支持多host算法模型在线预测，目前支持的多host算法有：纵向LR、纵向SBT。
2.支持在admin页面以pipeline方式展示模型信息。
3.增加模型同步功能，支持集群内不同serving-server节点之间模型同步复制，便于机器扩缩容。
4.admin页面模型操作/展示功能优化。
5.在admin页面增加cluster一键自检工具，可查看Cluster的服务自检状态，由后台定时对Cluster进行自检，用户也可以主动进行一键自检。
6.增加路由表编辑功能，可通过页面直接编辑当前serving-proxy配置的路由表信息。
7.服务拓扑图优化，展示各机器间的真实映射关系，拓扑图上可选取指定的机器并查看其详情。
8.新增guest和host之间支持http协议。
9.新增一个通用的httpAdaptor用于host获取特征。
```
 
## FATE-SERVING 2.0 支持的特性：
 
```text
1.单笔预测，2.0.*版本guest方与host方将并行计算，从而降低了耗时。    
2.批量预测，2.0.*版本开始引入的新的特性，一次请求可以批量提交一批需要预测的数据，极大地提高了吞吐量。  
3.并行计算，在1.3.*版本中guest方的预测与host方的预测是串行，从2.0版本开始guest方与host方将采用并行预测的方式，各方预测时可以根据特征数量拆分成子任务再并行计算。  
4.可视化的集群操作界面，引入新的组件serving-admin，它将提供集群的可视化操作界面，包括模型的管理、流量的监控、配置的查看、服务的管理等操作。   
5.新的模型持久化/恢复方式，在serving-server重启时1.3.*版本采用了回放推送模型的请求的方式来实例重启时恢复模型 ，2.0.* 版本使用了直接恢复内存数据的方式恢复模型。  
6.java版的SDK，使用该SDK可以使用FATE-SERVING带有的服务治理相关的功能，如服务自动发现、路由等功能。   
7.新的扩展模块，将需要用户自定义开发的部分（如：host方的特征获取adaptor接口开发）放到该模块，从而与核心源码分离。 
8.支持多种缓存方式，FATE-SERVING在1.3.*版本强依赖redis，从2.0.*版本开始，不再强依赖redis。可以选择不使用缓存、使用本地内存缓存、使用redis。   
9.更改内部预测流程，重构了核心代码，去除调了前后处理等组件，使用了统一的异常处理。算法组件不再跟rpc接口紧耦合。  
10.提供命令行工具，能够查询配置、模型信息
```

**兼容性**     

* 2.0.* 版本的host方 fate-serving 兼容 1.3.* 版本的 guest 方fate-serving，反过来1.3.* 版本的host fate-serving 不兼容2.0.*版本的guest。 如果需要升级集群，则需要先升级host方。

* 升级过程中， 建议不要在原有目录部署，原因是有可能因为原有版本的jar包没有替换干净导致jar包冲突 。可以新建另一个目录， 然后将新包部署。部署后并启动后，2.0.* 版本会读取当前用户根目录下.fate目录下的旧模型存储文件加载进内存，并且会生成新的模型文件放到另一目录，具体的目录可以通过配置model.cache.path配置，参见serving-server的配置详解，默认是当前部署目录下。（建议先备份用户目录下 .fate 目录 ，在1.3.* 版本中，所有模型的本地存储文件都放在了该目录下 ）。

* 2.0.* 有部分配置项与1.3.* 不同，升级时参考各模块的配置 。

[点击查看](../../../RELEASE.md)更多历史版本信息