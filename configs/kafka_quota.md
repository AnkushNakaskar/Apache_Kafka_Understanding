# Kafka Quotas
  * Consider the Kafka cluster shared for multiple tenants, 
    * this mean same broker is shared across multiple clients
    * resources shared between different clients even though topic might be different, consumer group might be different.
  * **What is meaning of Quotas in Kafka ?**
    * Quotas define number of resources dedicated to be used in kafka cluster.
  * **Why we need Quota ?**
    * Preventing resource contention : one client to use too many resources might affect other client
    * Fairness: equal amount of resources, no matter how big or busy they are.
    * Capacity planning : We can limit the usage of resources , this way can avoid the outage due to running out of resources.
  * **How Quota can be applied ?**
    * Quota is only applied per broker instead of cluster level
      * If we have cluster wise quota then would require a mechanism to share client quota usage among all the brokers
        * Which is very hard to manage, might not get it right
  * **Ways to implement quota ?**
    * Network bandwidth quotas define byte-rate thresholds
      * it is defined by byte range threshold ,which will be **shared by all the producer**
      * Each group of clients can publish/fetch a maximum of X bytes/sec per broker before clients are throttled.
      * 
