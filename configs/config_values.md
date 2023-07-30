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
        * Log rentain/message retain
            * There are three param to consider the message expiry in kafka
                * log expiry by size : as it says setting by which size exceed by that memory. it will mark for expire. This is per partion
                * log expiry by time : as it say, setting by which time the expiry message is gone.
            * All the above param works on segments not on every message.
                * segment is a file size of message. you rotate the file once it is filled. like in our case
                    log location : /usr/local/var/lib/kafka-logs/sampleexample-0
                    -rw-r--r--  1 ankush.nakaskar  admin       178 Jul 30 10:57 00000000000000000000.log
                * Default size of log segment is 1GB.
                    * Once it is filled , that log segment is close and new one is open
                    * Adjustment of this log size is very important. Only those messages get expire who are in close log segment.
                    * So even if the message is expire(base on above time & size retention) but the that message in current log segment, it will not be gone.
                    * Also having the small log segment size , can lead to many small file and can increase the disk IO. so decide it very cautiously
                    * you can roll the file of this segment by two params
                        * one by time and one by size like the message retention.
        * you can define the message max size, default one is 1MB, message will not be send unless this message is within limit.
            * larger message size has complication on broker end, like disk io will be large, replication and meta data etc will be impacted.
        * Broker Memory matter, especially when cosumer is almost caught up with producer since consumers read the messages from memory

                    

            
