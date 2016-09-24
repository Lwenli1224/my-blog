# Rabbitmq学习

## 协议

## 名词

- producer：生产者；向消息队列发送消息的进程
- queue：消息队列；是内置在rabbitmq中，它的实质是一个理论上无限大的缓冲区
- consumer：消费者；
- channel：内置对象，用于连接后与rabbitmq进行交互，如创建队列等
- durable：持久存储；当值为true时，服务器重启消息不会丢失
- exchange：交换区；消息不会直接从生产者发送至队列，需要通过交换区进行分发，有四种交换区：
	- direct：通过routing_key分发消息
	- topic：
	- headers：
	- fanout：会广播所有消息至订阅该exchange的队列
- ack：TCP协议中的一个概念，全称（Acknowledgement Number）。用于解决不丢包问题，在rabbitmq里，是确认消息被消费。

## 动作

- declare queue：通过给定名称声明队列，如果队列不存在，则创建。此操作是幂等性的。
- publish： 发送消息，需要exchange和routing key
- message acknowledgment：如果consumer设置需要ack，那么

## 模式

### 发送

1. 连接rabbit server
2. 创建channel
3. 声明队列
4. 向队列发送消息
5. 关闭连接

### 接收

1. 连接rabbit server
2. 创建channel
3. 声明队列
4. 从队列中读取消息
5. 关闭连接


### 工作队列

1. 连接rabbit server
2. 创建channel
3. 声明队列：持久化，均匀分发
4. 发送/消费：noAck，持久化

### 订阅者

1. 连接rabbit server
2. 创建channel
3. 声明交换区，使用fanout模式
4. 声明队列
5. 绑定交换区
6. 消费队列

### 路由

1. 连接rabbit server
2. 创建channel
3. 声明交换区，使用direct模式
4. 声明队列
5. 绑定交换区，使用routing_key
6. 消费队列



