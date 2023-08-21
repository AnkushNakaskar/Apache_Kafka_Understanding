
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
  * 
