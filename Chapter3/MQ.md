# 消息队列

---

## 应用场景

1. 异步处理
2. 应用解耦
3. 流量消峰

## JMS消息模型

1. P2P模型

   * 概念

     > 消息队列(Queue)、发送者(Sender)、接收者(Receiver)。每一个消息都被发送到一个特定的队列，接收者从队列获取消息。队列保留着消息，直到它们被消费或超时。

   * 特点

     > * 每一个消息仅仅有一个消费者(Consumer)(即一旦被消费，消息就不再在消息队列中)
     > * 发送者和接收者之间在时间上没有依赖性，也就是说当发送者发送了消息之后。无论接收者有没有正在执行，它不会影响到消息被发送到队列。
     > * 接收者在成功接受消息之后须要向队列应答成功
     
   * 代码示例

     ```java
     package com.zjs.MQ;
     
     import org.apache.activemq.ActiveMQConnection;
     import org.apache.activemq.ActiveMQConnectionFactory;
     
     import javax.jms.*;
     //生产者
     public class MessageProducer {
         //定义ActivMQ的链接地址
         private static final String ACTIVEMQ_URL="tcp://127.0.0.1:61616";
         //定义发送消息的队列名称
         private static final String Queue_NAME="MyMessage";
     
         public static void main(String[] args) throws JMSException {
             //创建链接工厂
             ActiveMQConnectionFactory activeMQConnectionFactory=new ActiveMQConnectionFactory(ACTIVEMQ_URL);
             //创建链接
             Connection connection=activeMQConnectionFactory.createConnection();
             //打开连接
             connection.start();
               /*4.获取session  (参数1：是否启动事务,
             参数2：消息确认模式[
            AUTO_ACKNOWLEDGE = 1    自动确认
            CLIENT_ACKNOWLEDGE = 2    客户端手动确认   
            DUPS_OK_ACKNOWLEDGE = 3    自动批量确认
            SESSION_TRANSACTED = 0    事务提交并确认
           ])*/
             Session session=connection.createSession(false,Session.AUTO_ACKNOWLEDGE);
             //创建队列目标
             Destination destination=session.createQueue(Queue_NAME);
             //创建一个生产者
             javax.jms.MessageProducer producer=session.createProducer(destination);
             //创建模拟100个消息
             for(int i=1;i<=100;i++)
             {
                 TextMessage message=session.createTextMessage("我发送的message: "+i);
                 //发送消息
                 producer.send(message);
                 System.out.println("我现在发送的消息是:"+message.getText());
             }
             //关闭链接
             connection.close();
     
         }
     }
     
     
     
     package com.zjs.MQ;
     import org.apache.activemq.ActiveMQConnectionFactory;
     
     import javax.jms.*;
     //消费者
     public class MessageConsumer {
         //定义ActivMQ的连接地址
         private static final String ACTIVEMQ_URL = "tcp://127.0.0.1:61616";
         //定义发送消息的队列名称
         private static final String QUEUE_NAME = "MyMessage";
     
         public static void main(String[] args) throws JMSException {
             //创建连接工厂
             ActiveMQConnectionFactory activeMQConnectionFactory = new ActiveMQConnectionFactory(ACTIVEMQ_URL);
             //创建连接
             Connection connection = activeMQConnectionFactory.createConnection();
             //打开连接
             connection.start();
             //创建会话
             Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
             //创建队列目标
             Destination destination = session.createQueue(QUEUE_NAME);
             //创建一个消费者
             javax.jms.MessageConsumer consumer=session.createConsumer(destination);
             //创建消费的监听
             consumer.setMessageListener(new MessageListener() {
                 public void onMessage(Message message) {
                     TextMessage textMessage=(TextMessage)message;
                     try {
                         System.out.println("获取的信息:"+textMessage.getText());
                     } catch (JMSException e) {
                         e.printStackTrace();
                     }
               }
             });
     
         }
     }
     ```
     
     

2. Pbu/Sub模型

   * 概念

     > 主题(Topic)、公布者(Publisher)、订阅者(Subscriber)。

   * 特点
   
     > * 每个信息可以有多个消费者
     > * 公布者和订阅者之间有时间的依赖性
     
   * 代码实列
   
     ```java
     package com.zjs.MQ;
     
     import org.apache.activemq.ActiveMQConnection;
     import org.apache.activemq.ActiveMQConnectionFactory;
     
     import javax.jms.*;
     //生产者
     public class MessageProducer {
         //定义ActivMQ的链接地址
         private static final String ACTIVEMQ_URL="tcp://127.0.0.1:61616";
         //定义主题名称
         private static final String TOPIC_NAME="MyMessage";
     
         public static void main(String[] args) throws JMSException {
             //创建链接工厂
             ActiveMQConnectionFactory activeMQConnectionFactory=new ActiveMQConnectionFactory(ACTIVEMQ_URL);
             //创建链接
             Connection connection=activeMQConnectionFactory.createConnection();
             //打开连接
             connection.start();
             //创建会话
             Session session=connection.createSession(false,Session.AUTO_ACKNOWLEDGE);
             //创建主题目标
             Topic destination=session.createTopic(TOPIC_NAME);
             //创建一个生产者
             javax.jms.MessageProducer producer=session.createProducer(destination);
             //创建模拟100个消息
             for(int i=1;i<=100;i++)
             {
                 TextMessage message=session.createTextMessage("我发送的message: "+i);
                 //发送消息
                 producer.send(message);
                 System.out.println("我现在发送的消息是:"+message.getText());
             }
             //关闭链接
             connection.close();
     
         }
     }
     
     
     
     package com.zjs.MQ;
     import org.apache.activemq.ActiveMQConnectionFactory;
     
     import javax.jms.*;
     //消费者
     public class MessageConsumer {
         //定义ActivMQ的连接地址
         private static final String ACTIVEMQ_URL = "tcp://127.0.0.1:61616";
         //定义发送消息的主题名称
         private static final String QUEUE_NAME = "MyMessage";
     
         public static void main(String[] args) throws JMSException {
             //创建连接工厂
             ActiveMQConnectionFactory activeMQConnectionFactory = new ActiveMQConnectionFactory(ACTIVEMQ_URL);
             //创建连接
             Connection connection = activeMQConnectionFactory.createConnection();
             //打开连接
             connection.start();
             //创建会话,并且确认的方式有四种
             Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
             //创建主题目标
             Topic destination = session.createTopic(QUEUE_NAME);
             //创建一个消费者
             javax.jms.MessageConsumer consumer=session.createConsumer(destination);
             //创建消费的监听
             consumer.setMessageListener(new MessageListener() {
                 public void onMessage(Message message) {
                     TextMessage textMessage=(TextMessage)message;
                     try {
                         System.out.println("获取的信息:"+textMessage.getText());
                     } catch (JMSException e) {
                         e.printStackTrace();
                     }
                 }
             });
     
         }
     }
     ```
   
     

##  JMS的一些关键接口

1. ConnectionFactory：用于创建连接到消息中间件的连接工厂

2. Connection：代表了应用程序和服务之间的连接通路

3. Destination：指消息发布的地点，包括队列模式和主体模式

4. Session：表示一个单线程的上下文，用于发送和接受消息

5. MessageConsumer：由会话创建，用于接受发送到目的的消息

6. MessageProducer：由会话创建，用于发送消息

7. Message：是在消费者和生产者之间传递的对象，消息头，一组消息属性，和一个消息体

8. **接口关系如下**

   > ![](C:\Users\87360\Desktop\cvpr\20180714104124465.png)