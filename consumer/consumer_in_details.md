# Kafka Consumer in details 
* **Consumer & Consumer group**
  * each consumer in the group will receive messages from a different subset of the partitions in the topic
  * ![](diagram/consumer_architecture.png)
    * As above figure depict, below points
      * There is one consumer group, which have only one consumer inside the group
      * And Topic has four partition , since there is only one consumer , that same instance is consuming messages from all the partition.
      * We can have at max number of consumer as number of partition. Excess consumer will not be use while reading message.
    * We can have multiple consumer group subscribe to same topic.
      * You can consider the consumer group as logical use case to process the message.
      * For example : Payment event message, this can be use in two different way
        * Payment notification to user : this is one of consumer group, subscribe to same topic
        * Payment process via bank : this is another consumer group, subscribe to same topic
* **Consumer Groups and Partition Rebalance**
  * When we add a new consumer to the group, it starts consuming messages from partitions previously consumed by another consumer
  * When a consumer shuts down or crashes; it leaves the group, and the partitions it used to consume will be consumed by one of the remaining consumers
  * Reassignment of partitions to consumers also happens when the topics the consumer group is consuming are modified (e.g., if an administrator adds new partitions).
      
