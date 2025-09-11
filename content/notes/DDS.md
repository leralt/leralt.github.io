DDS是数据分发服务（Data Distribution Service）是分布式实时通信中间件协议．
## DDS发布订阅模型DCPS
讲具体协议之前，我们先熟悉下几个专有名词：
- ==Domain==：代表一个通信平面，由Domain ID唯一标识，只有在同一个域内的通信实体才可以通信；如果考虑车内通信，可以只划分1个Domain，也可以按照交互规则或其他规则，定义多个Domain；
- Domain Participant：代表域内通信的应用程序的本地成员身份，简单来说，就是说明同一数据域内的通信成员；
- Topic：是数据的抽象概念，由TopicName标识，关联相应数据的数据类型(DataType)，如果把车内所涉及的所有Topic集合在一起，这样就形成一个虚拟的全局数据空间“Global Data Space”，进一步弱化了节点的概念，所以域参与者已经不是节点的概念了；
- DataWriter：数据写入者，类似缓存，把需要发布的主题数据从应用层写入到DataWriter中；
- DataReader：数据读取者，同样可以理解为一种缓存，从订阅者得到主题数据，随之传给应用层；
- Publisher：发布者，发布主题数据，至少与1个DataWriter关联，通过调用DataWriter的相关函数将数据发出去；
- Subscriber：订阅者，订阅主题数据，至少与1个DataReader关联。当数据到达时，应用程序可能忙于执行其他操作或应用程序只是等待该消息时，这样就会存在两种情况，同步访问和异步通知。

用户只关心自己想要的数据，数据通过Topic进行标识，这样发布者根据主题发布数据，订阅者根据自己感兴趣的主题订阅数据。这便是DDS的核心，以数据为中心的发布-订阅模型DCPS（Data-Centric Publish-Subscribe）
![[Pasted image 20221011113601.png]] ^1220cd