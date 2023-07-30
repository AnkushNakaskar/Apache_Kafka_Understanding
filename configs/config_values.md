# Understanding of Kafka internal : Log directory & Configs values
    * Kafka is devided into below concept
        * Topic : This is logic represntation where producer push messages
            * Each topic is has a multiple partions
                * this is where , messages are store , it maintain the FIFO order. but across the partions order is not maintain.
            * Based on replication factor , each partion data is replicated to multiple broker(kafka server)
            * Each message is stored in disk mentioned in kafka config (log.dir)
                * Each kafka partion is log file in this directory like in our case
                   * directory path : /usr/local/var/lib/kafka-logs/test-0
                        * Since we have created only one partion and only one topic, you see the directory with name : test-0
                   * File name : 00000000000000000000.log contain the messages we pushed in byte format.
                * Whenever we create/increase new partion or new topic, we see the new log file in this directory
                    * Since we have create new topic : sampleexample, we see the directory name : sampleexample-0
            * Keep in mind that the number of partitions for a topic can only be increased, never decreased    
                
            
