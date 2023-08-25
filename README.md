# Apache_Kafka_Understanding
This project guide us for apache kafka


### Request flow for message

### 
    -->Kafka Client -> Connect to any of the broker mentioned in (broker list of kafka client app), since all broker are in sync and have all the information about the topic/partitions
    -> Particular broker get details of topic and partions related to it. -> leader partition is selected for that topic and list of leader partions is fetch back, client select the leader partion on which that message should go -> leader partion replicate the message into partion replica -> consumer consume the message  for nearest in sync replica
    -> next time, client remember the broker who has the topic and details  of partions
![](diagram/kafka_client_diag.png)



* If a broker receives a produce request for a specific partition and the leader for this partition is on a different broker, 
  * the client that sent the produce request will get an error response of “Not a Leader for Partition.” The same error will occur if a
  * fetch request for a specific partition arrives at a broker that does not have the leader for that partition.
  * Produce request : request which has the message body from client
  * Fetch request : request send by various replica's of leader partitions to get the recent message
    * ```Kafka’s clients are responsible for sending produce and fetch requests to the broker that contains the leader for the relevant partition for the request```
* **How do the clients know where to send the requests?**
  * Kafka clients use another request type called a **_metadata request_**, which includes a **_list of topics the client is interested in_**
    * server response specifies which **partitions exist in the topics, the replicas for each partition, and which replica is the leader**
    * Metadata requests can be sent to any broker.
    * Clients typically cache this information and use it to direct produce and fetch requests to the correct broker for each partition
    * occasionally refresh this information
    * **Note : if a client receives the “Not a Leader” error to one of its requests, it will refresh its metadata before trying to send the request again, since the error indicates that the client is using outdated infor‐ mation and is sending requests to the wrong broker**
![](diagram/kafka_client_request_flow.png)
