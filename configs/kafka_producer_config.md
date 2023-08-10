# Kafka producer
* Producer is designed and developed in java 
* Response of kafka push : topic, partion & offset of message in partition.
  * Producer is composite of 
        
      ```
              private Future<RecordMetadata> sendRecord(K key, M record, Callback callback) throws IOException {
                  ProducerRecord<K, M> producerRecord = new ProducerRecord(this.topicName, key, record);
                  return this.kafkaProducer.send(producerRecord, callback);
              }
    
      ``` 
  * Producer is of three categories : 
      * Fire and forget : we dont care if the message is handle properly or not
      * Send and wait until we get a response back from kafka cluster
      * Send and wait asynchronously by providing a callable function, Above function is a exmaple of that.
        * Suppose the network round-trip time between our application and the Kafka cluster is 10 ms. If we wait for a reply after sending each message, sending 100 messages will take around 1 second
        * It is not recommended to perform a blocking operation within the callback
  * Params in producer configs :
  * ACKS : acks parameter controls how many partition replicas must receive the record before the producer can consider the write successful
    * ack=0 , ack=1 , ack=all  : latencies are in decreasing order in this case.
    * ack =0, means does not wait for broker to get message successfully
    * ack=1 , will wait for success from broker ,that message got successfully
    * ack =11, Will wait till all in-sync replica of broker also got message
  * In order to maintain consistency, Kafka will not allow consumers to read records until they are written to all in sync replicas
    * end to end latency matter when all in-sync replicas have the message.
  * Times in kafka producer :
    * max.block.ms : 
        * Time when send() call actually send data to kafka until to batch , adding into batch
        * send() returned successfully and the record is placed in a batch
    * delivery.timeout.ms : 
      * Time when that particular batch is pushed to broker , including time spent on retries
      * exceeds while retrying a batch push :
        * callback will be called with the exception that corresponds to the error that the broker returned before retrying
      * exceeds while record batch was still waiting to be sent
        * callback will be called with a timeout exception
  * retries and retry.backoff.ms :
    * the value of the retries parameter will control how many times the producer will retry sending the message
    * default is 100ms between retries
  * linger.ms :
    * linger.ms controls the amount of time to wait for additional messages before send‐ ing the current batch
  * buffer.memory :
    * sets the amount of memory the producer will use to buffer messages wait‐ ing to be sent to brokers
* Kafka Interceptors :
  * It intercepts the kafka record push.
    * ProducerRecord<K, V> onSend(ProducerRecord<K, V> record) : 
      * it is called before record is not send to kafka. O/p of this method is serialised and pushed to kafka.
      * You can modify the input via this method
    * void onAcknowledgement(RecordMetadata metadata, Exception exception)
      * You can capture the o/p from kafka, cannot modify the i/p from this mothod
  * Common use cases for producer interceptors include capturing monitoring and trac‐ ing information; enhancing the message with standard headers, especially for lineage tracking purposes; and redacting sensitive information
  * producer interceptors can be applied without any changes to the client code
* Kafka Quota and Throttle 
  * https://supergloo.com/kafka-tutorials/kafka-quotas/
  * https://cwiki.apache.org/confluence/display/kafka/kip-13+-+quotas
