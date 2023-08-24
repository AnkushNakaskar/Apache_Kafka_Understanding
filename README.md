# Apache_Kafka_Understanding
This project guide us for apache kafka


### Request flow for message

### Kafka Client -> Connect to any of the broker mentioned in (broker list of kafka client app), since all broker are in sync and have all the information about the topic/partitions
    -> Particular broker get details of topic and partions related to it. -> leader partition is selected for that topic and list of leader partions is fetch back, client select the leader partion on which that message should go -> leader partion replicate the message into partion replica -> consumer consume the message  for nearest in sync replica
    -> next time, client remember the broker who has the topic and details  of partions
![](kafka_client_diag.png)
