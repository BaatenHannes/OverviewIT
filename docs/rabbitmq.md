# RabbitMQ

## Overview

To prevent too many connections from opening, only one connection is opened, and channels are logically divided to run on that one connection.
- one physical connection
- multiple 'logical' connections -> channels


### Exchanges

* Direct -> routing key to a single queue
* Fanout -> dispatch to all queues bound to this exchange without routing key
* Topic -> routing key with regex like routing
* Header


### Queues

* Persistant -> data is kept on restart (stored phyically)
* Transient -> data is lost on restart

Message TTL: can be declared by developers on publish, or inherited by queue. Default is no end time to live.


### Consumer


### Acknowledge

How to make sure message is succesfully produced or consumed.

Producer

* Transaction: callback after commit sending multiple messages
* Notification: (on single message)

Consumer

* Manual acknowledge: in code explicit (automatically in MassTransit,, together with retry)
* Automatic acknowledge: 


### Virtual Host

Name - to differentiate between different environments, each with its own credentials.
Virtual hosts for different clients - to seperate different client needs.
Each has its own user, exchanges and queues.

### Scaling

#### Clustering

Node - installation of rabbitMq. Cluster contains multiple nodes (or installations).
To avoid single point of failure.
Can be automatically or manually build. (join cluster wit cmd)
-> queues (definition) is replicated, data is not automatically replicated. 
-> apply a policy to enforce data replication - mirrored queue
-> data is consumed from one master node

-> load balancer needed as intermediate layer, to shield the producer from referencing mutliple nodes (with ip and port etc).

Good from raising Availability, but lowers Performance.


#### RabbitMQ Federation/Shovel

Federation: possibility to produce to one cluster, but consume from another cluster.
Source and destination cluster.
Federation link between the clusters (queues or exchanges).
Also usefull for linking data between two physical location (for complete disaster failure of one cluster).


### Read without remove

Negative acknowledge is possible - reads but not removes from queue.



HttpPort: 15672 voor GUI
AMQP-port: 5672


### MassTransit: Command vs Event

* Command with one consumer
* Event with multiple consumers

Sending or Publishing will generate an Exchange
Consuming will generate an exchange that is coupled to the publisher exchange together with a queue.

MassTransit consumer - multiple consumers possible without destroying message on ackn. Masstransit will create a general consumer in the background to publish messages to multiple consumers. (Adds a consumer layer.)




Questions:
 - large or small messages