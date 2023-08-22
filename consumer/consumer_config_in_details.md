
### Configuring/Properties Consumers
* **fetch.min.bytes** : default 1 byte
    * specify the minimum amount of data that it wants to receive from the broker when fetching records
    * records amount to fewer bytes than fetch.min.bytes, the broker will wait until more messages are available before sending the records back to the consumer
* **fetch.max.wait.ms** : default is 500ms
    * wait for previous config time to get min bytes
    * will respond with data either when it has **fetch.min.bytes** of data to return or after current config time , whichever happens first.
* **fetch.max.bytes** : default : 50 MB
    * specify the maximum bytes that Kafka will return whenever the consumer polls a broker
    * size of memory that the consumer will use to store data that was returned from the server
    * limit is ignore if the record is more in size than value specified
* **max.poll.records** :
    * controls the maximum number of records that a single call to poll() will return
* **max.partition.fetch.bytes** : default : 1 MB
    * When KafkaConsumer.poll() returns ConsumerRecords, the record object will use at most max.partition.fetch.bytes per partition assigned to the consumer
    * Note that controlling memory usage using this configuration can be quite complex, as you have no control over how many partitions will be included in the broker response
        * we highly recommend using fetch.max.bytes instead, unless you have special reasons to try and process similar amounts of data from each partition.
* **max.poll.interval.ms**  : 5 minutes
  * time during which the consumer can go without polling before it is considered dead
  * The easiest way to know whether the consumer is still processing records is to check whether it is asking for more records
* **default.api.timeout.ms** : 1 min
  * all API calls made by the consumer when you don’t specify an explicit timeout
* **request.timeout.ms** : 30 Sec
  * time the consumer will wait for a response from the broker
  * broker will not respond at all, close the connection, and attempt to reconnect
* **auto.offset.reset** : latest
  * consumer when it starts reading a partition for which it doesn’t have a committed offset
  * latest : lacking a valid offset, the consumer will start reading from the newest records (records that were written after the consumer started running)
  * “earliest,” which means that lacking a valid offset, the consumer will read all the data in the partition, starting from the very beginning
* **enable.auto.commit** : true
  * the consumer will commit offsets automatically 
  * true or false
* **partition.assignment.strategy** : Range
  * Range
    * each consumer a consecutive subset of partitions from each topic it subscribes to
    * like Topic T1 -> P1,P2,P3  & T2 -> P4,P5,P6 and C1 & C2 as consumer
      * Since partitions are not even, hence first consumer will get more , like C1 will listen to P1,P2, P4,P5 & C2 will listen P3,P6. First consumer will get more always in range.
  * RoundRobin :
    * all the partitions from all subscribed topics and assigns them to consumers sequentially, one by one
    * C1 will have P1,P3,P5 & C2 will have P2,P4,P6
  * Sticky :
    * in case of a rebalance, it will leave as many assignments as possible in place, minimizing the overhead associated with moving partition assignments from one consumer to another
* **client.id**
  * brokers to identify requests sent from the client, such as fetch requests. It is used in logging and metrics, and for quotas.
* **client.rack**
  * By default , consumers will fetch messages from the leader replica of each partition
  * but to fetch from replica which near to consumer, you can specify this property.
* **offsets.retention.minutes** : 7 days
  * consumer group offset maintain duration
  * if consumer group is inactive, then offset become empty , and if it restart/rejoin the group, that will behave like brand new will start message consumption from start.
### Questions ?
  * What if no message is there on topic for longer time, does it mark the consumer is dead or consumer group is dead ?
    * No, since poll request is in infinite loop, even though it will not get record, but the consumer wil mark alive. 
