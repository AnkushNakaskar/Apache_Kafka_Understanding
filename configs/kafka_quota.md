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
    * producers/consumers can result into DOS (Denial of Service) for other producers/consumers
  * **How Quota can be applied ?**
    * Quota is only applied per broker instead of cluster level
      * If we have cluster wise quota then would require a mechanism to share client quota usage among all the brokers
        * Which is very hard to manage, might not get it right
  * **Ways to implement quota ?**
    * Network bandwidth quotas define byte-rate thresholds
      * it is defined by byte range threshold ,which will be **shared by all the producer**
      * Each group of clients can publish/fetch a maximum of X bytes/sec per broker before clients are throttled.
      * Types :
        * Consumer throttle 
          * allows you to limit the amount of data a consumer can retrieve
          * prevents any single consumer from using excessive network bandwidth
        * Producer throttle :
          * limits the amount of data a producer can send to the Kafka
          * producers do not overload the system by sending excessive data
    * Request rate quotas :
      * defined by CPU utilization thresholds as a percentage of network and I/O threads
      * percentage of time a client can utilize on both request handler I/O threads and network threads of each broker within a time window.
  * **How Quota works ?**
    * Quotas are set at broker level
    * Each unique client-id has fixed number of bytes/second, meaning it can fetch/push that much of bytes on particular broker
    * bytes rate is measure within a 30 windows of each 1 second
  * **What happen when violation of quota occur ?**
    * brokers do not throw any error when it detects a quota violation
    * it attempts to slow down a client exceeding quota by delaying the response
    * Amount of delay is computed by brokers in a way that client does not violate its quota
    * broker first computes the amount of delay needed to bring the violating client under its quota and returns a response with the delay immediately
    * Fetch (consumer throttling)
      * broker mutes the channel to the client, not to process requests from the client anymore, until the delay is over
    * Push (Producer throttling)
      * client will also refrain from sending further requests to the broker during the delay
    * **How do we set quota ?**
      * default quotas can be changed in Kafka configuration file and require Kafka broker restart
        * ```
          # Sets producer quota to 50 MB
            quota.producer.default=52428800
          
          # Sets consumer quota to 50 MB
            quota.consumer.default=52428800 
          ```
      * we may need to configure different quota for different clients at run time without any restart of Kafka brokers
      * below commands can be executed from Kafka broker home directory to configure client with id "test-client" with producer quota as 10 MB and consumer quota as 20 MB 
        * ``` 
             # Adds configuration for client with id test-client
             ./bin/kafka-configs.sh  --zookeeper localhost:2181 --alter --add-config 'producer_byte_rate=10485760,consumer_byte_rate=20971520' --entity-name test-client --entity-type clients 
          ```
        * Once updated, you can also verify your configurations using below describe command -
          * ``` 
            # Describe configurations
            ./bin/kafka-configs.sh  --zookeeper localhost:2181 --describe --entity-name test-client --entity-type clients

            #Output of above command will be as below
            Configs for clients:test-client are producer_byte_rate=10485760,consumer_byte_rate=20971520 
            ```
  
    
