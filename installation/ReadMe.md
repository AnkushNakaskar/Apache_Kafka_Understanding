# Installing of kafka on macOs
    * Basic requirement for kafka install is below frameworks
        * Java
        * Zookeeper
        * Kafka
    * Since kafka was written in java, we need to install java on server
    * Apache kafka uses zookeeper for client meta data, broker meta data etc.
        
```             
 1. brew install java
 2. brew install zookeeper 
 3. brew install kafka
```
    * You can verify the zookeeper is install by connecting it using below steps
        * telnet localhost 2181
        * then use cmd : srvr
        * Response should be like this :
            ```
                Zookeeper version: 3.5.7-f0fdd52973d373ffd9c86b81d99842dc2c7f660e, built on 02/10/2020 11:30 GMT
                Latency min/avg/max: 0/0/41
                Received: 26214
                Sent: 26232
                Connections: 3
                Outstanding: 0
                Zxid: 0x26d9
                Mode: standalone
                Node count: 32

            ```
    * You can verify kafka by below steps
        * you can check the properties of kafka default using below command 
           ```    /usr/local/opt/kafka/bin/kafka-server-start /usr/local/etc/kafka/server.properties  ```
        * create a test topic
           ``` /usr/local/opt/kafka/bin/kafka-topics --bootstrap-server localhost:9092 --create --replication-factor 1 --partitions 1 --topic test ```
        * Discribe topic using below cmd
           ``` /usr/local/opt/kafka/bin/kafka-topics --bootstrap-server localhost:9092 --describe --topic test ```
        * You can produce message to topic using below cmd
            ``` /usr/local/opt/kafka/bin/kafka-console-producer --bootstrap-server localhost:9092 --topic test  ```
        * You can consume message using below cmd :
            ``` /usr/local/opt/kafka/bin/kafka-console-consumer --bootstrap-server localhost:9092 --topic test --from-beginning  ```
