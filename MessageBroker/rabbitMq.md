# RabbitMQ

![](https://blog.zenika.com/wp-content/uploads/2012/03/RabbitMQ-1.jpg)

## I. Introduction
RabbitMQ is a message broker: it accepts and forwards messages. You can think about it as a post office: when you put the mail that you want posting in a post box, you can be sure that the letter carrier will eventually deliver the mail to your recipient. In this analogy, RabbitMQ is a post box, a post office, and a letter carrier.

RabbitMQ, and messaging in general, uses some jargon
- Producing means nothing more than sending. A program that sends messages is a `producer` :

![](https://www.rabbitmq.com/img/tutorials/producer.png)

- A queue: where data are stored. A queue is only bound by the host's memory & disk limits, it's essentially a large message buffer.

![](https://www.rabbitmq.com/img/tutorials/queue.png)

- A `consumer` is a program that mostly waits to receive messages:

![](https://www.rabbitmq.com/img/tutorials/consumer.png)

## II. Concepts
**1. Single consumer**

Send and recieve messages from a named queue

![](https://www.rabbitmq.com/img/tutorials/python-one-overall.png)

**2. Multiple consumers**

Distribute time-consuming tasks among mutiple workers.

Each task is delivered to exactly one worker.

![](https://www.rabbitmq.com/img/tutorials/python-two.png)

The main idea behind Work Queues (aka: Task Queues) is to avoid doing a resource-intensive task immediately and having to wait for it to complete. Instead we schedule the task to be done later. We encapsulate a task as a message and send it to the queue. A worker process running in the background will pop the tasks and eventually execute the job. When you run many workers the tasks will be shared between them.

- `Round-robin` dispatching: By default, RabbitMQ will send each message to the next consumer, in sequence. On average every consumer will get the same number of messages. This way of distributing messages is called round-robin.

- Message `acknowledgement`: In order to make sure a message is never lost, RabbitMQ supports message acknowledgments. An ack(nowledgement) is sent back by the consumer to tell RabbitMQ that a particular message had been received, processed and that RabbitMQ is free to delete it.

- Message `durability`: When RabbitMQ quits or crashes it will forget the queues and messages unless you tell it not to. Two things are required to make sure that messages aren't lost: we need to mark both the queue and messages as durable.

- Fair dispatch: RabbitMQ don't dispatch a new message to a worker until it has processed and acknowledged the previous one. Instead, it will dispatch it to the next worker that is not still busy.

**3. Publish/Subcribe**

Deliver a message to multiple consumers through `Exchange`

![](https://www.rabbitmq.com/img/tutorials/exchanges.png)

- The core idea in the messaging model in RabbitMQ is that the producer never sends any messages directly to a queue. Actually, quite often the producer doesn't even know if a message will be delivered to any queue at all.
- Instead, the producer can only send messages to an exchange. An exchange is a very simple thing. On one side it receives messages from producers and the other side it pushes them to queues. The exchange must know exactly what to do with a message it receives.
- The rules for that are defined by the **exchange type**: `direct` , `topic`, `headers`, `fanout`

**4. Routing**

Subscribe only to a subset of the messages

![](https://www.rabbitmq.com/img/tutorials/direct-exchange.png)

- `Binding`: is a relationship between exchange and queue. Binding can take an extra **binding key** parameter. The mean of a **binding key** depends on the exchange type. (The `fanout` exchange will ignored it)

- `Direct` exchange: The routing algorithm behind a `direct` exchange is a message goes to the queues whose **binding key** exactly matches the **routing key** of the message.

- `Multiple` binding: It is perfectly legal to bind multiple queues with the same binding key. 

In our example we could add a binding between X and Q1 with binding key black. In that case, the direct exchange will behave like fanout and will broadcast the message to all the matching queues. A message with routing key black will be delivered to both Q1 and Q2.

![](https://www.rabbitmq.com/img/tutorials/direct-exchange-multiple.png)

**5. Topics**

Receiving messages based on a pattern (topics)

![](https://www.rabbitmq.com/img/tutorials/python-five.png)

