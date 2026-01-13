# message queue

It is a queue like struture which used to manage tasks and messages of server.

It used by load balancer if in mutiple server system, one server fails to  response in particular amount of time then the task or message will be provided to another server to serve.

## examples 
1.Celery + RabbitMQ/Redis: Celery is the task system (serialization, retries, scheduling). RabbitMQ/Redis are brokers that handle message delivery, routing, and worker coordination.
2.RDBMS-backed task queue: enqueue tasks in a jobs table, workers poll and atomically claim rows. Works for low throughput but needs careful locking and provides less flexibility than a message broker.
3.Serverless/managed: Cloud task services (AWS SQS + Lambda, Google Cloud Tasks) combine broker-like delivery with execution endpoints so the broker is managed for you.


 Keep in mind, they're not really "disadvantages" but rather "implementation details":

Optimized for discrete message handling. If you want batching, you need to implement it in your model (message body) or you need to move away from Rabbit's reliable messaging constructs (transactions).
It's not as fast as Apache Kafka - that's because Kafka batches messages and does not offer a robust routing and security feature set.
It's not as fast as ZeroMQ - that's because ZeroMQ is not a messaging framework (it's a communication library). You effectively have to build your own broker and bake in your own form of reliability. But honestly, if you don't know the difference between ZeroMQ and RabbitMQ, don't even consider using ZeroMQ.
AMQP considered complex. I think this is a shallow argument because if you understand it, you get fantastic flexibility.
Written in Erlang. This throws a lot of people off at first for a number of reasons:
The configuration files are essentially an Erlang tuple, which is a very jarring format if you come from XML, JSON, YAML, or INI based configuration systems.
If you cluster the server, you will need to deal with Erlang's message passing framework. It really helps to understand how Erlang works if you run into issues (like dealing with Erlang cookies or EPMD ports, etc.).
Ops team will need to get accustomed to Erlang deployments and managing RabbitMQ. A couple of years ago, we had trouble installing newer versions of Erlang on Amazon Linux because of it's GUI dependencies. It's probably a lot better now, but something to consider.
Reading the source will be difficult. Erlang's syntax is unlike any language I've used, and it's programming constructs (like pattern matching and process architecture) will take some time to get used to.
Extending the system will be even more difficult (for the same reasons as 3.d.). Take my advice and learn Elixir (compiled to Erlang).
So what happens if RabbitMQ fails? What happens to the messages?

Concerned about reliability huh? This is where RabbitMQ shines! RabbitMQ mitigates failure in a number of ways:

RabbitMQ supports fully transactional communication between clients and brokers.
You can set up queues to require acknowledgements (ACKs) from the client before a message is removed from the queue. This means if a client crashes in the middle of processing a message and doesn't send the ACK, the message will remain in the queue until the client returns and ACK's it's consumption.
You can set up the same confirmations for clients (servers ACK the ACK).
RabbitMQ queues can be durable. This means, when messages are placed in the queue, the message will be stored on disk before being made available to clients. If the server dies and restarts, the queue will retain all messages written to that queue.
RabbitMQ nodes can be clustered. When clustered:
Nodes can drop off a cluster and be recovered.
Messages can be published and received from any broker in the cluster.
Exchanges (entry point of messages) exist on every broker (basically every broker knows how to route a message).
Queues have locality (meaning they exist on a single broker in the cluster - I believe the node that has the queue is randomly chosen).
Queues can be made Highly Available (Active/Active clustering). In the previous point, I mentioned that queues have locality to a single node in the cluster. Active/Active availability allows RabbitMQ to mirror a queue on multiple nodes in a cluster.
RabbitMQ has awesome plugins to help it's reliability.
Federation plugin - run multiple RabbitMQ clusters passing messages between them (this is different from clustering in the sense that each cluster communicates via AMQP with other clusters - they're effectively clients of each other).
Database integration (community plugins) like a Riak sink (copy all messages to Riak).

# types of message queue

1. point to point messaging queue
Point-to-point message queues are the simplest type of message queue. When a producer sends a message to a point-to-point queue, the message is stored in the queue until a consumer retrieves it. Once the message is retrieved by a consumer, it is removed from the queue and cannot be processed by any other consumer.

it used in patterns like request-response,Work Queue, Gurranteed Delivery

2. Publish-Subscribe Message Queues

Publish-Subscribe Message Queues are more complex than point-to-point message queues. When a producer publishes a message to publish/subscribe queue, the message is routed to all consumers that are subscribed to the queue. Consumers can subscribe to multiple queues, and they can also unsubscribe from queues at any time.

it used in streaming application such as social media and stock market tickers.

# use Cases

-email notification
-video processing 
-order processing 
-log aggregation

# when to use?

