
## Deployment

To deploy RabbitMQ in Docker

- First u type `dir` it will display information about all files and subfolders in the current directory

```powershell
Mode                 LastWriteTime         Length Name                                                                                                                                                                                        
----                 -------------         ------ ----                                                                                                                                                                                        
d-----          1/4/2024   6:03 PM                .github                                                                                                                                                                                     
d-----         7/31/2024   2:48 PM                ConsumerAPI                                                                                                                                                                                 
d-----          1/4/2024   6:04 PM                Contract                                                                                                                                                                                    
d-----         7/27/2024   5:58 PM                ProducerAPI                                                                                                                                                                                 
-a----         7/30/2024   6:33 PM            838 docker-compose.Dev.Infrastructure.yaml                                                                                                                                                      
-a----         7/27/2024   5:57 PM           2393 RabbitMQDemo.sln 
```
- Then u copy the name of file <docker.yaml> and write command below

````powershell
docker compose -f <docker.yaml> up
````

- Finnaly wait to docker deploy ....

# Some Notes

- Producer:

    - Create Exchange:
        + Based on the Class of the Message sent to create an Exchange
        + Based on the interfaces/classes that the message tries to create more Exchanges

NOTE: Use `[ExcludeFromTopology]` to remove redundant Exchanges
````C#
    // When use Topic Exchange -> dont use [ExcludeFromTopology]
    [ExcludeFromTopology]
    public interface INotification : IMessage
    {
        public Guid Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        public string Type { get; set; }
        public Guid TransactionId { get; set; }
    }
````

- Consumer:
    
    - Create Queue: Create Queues based on the class inheriting IConsumer
    - Create Exchange:
        + Create an Exchange similar to the Queue name
        + Create an Exchange with the name of the Message

- Duration: When stopping RabbitMQ, the messages will be saved below the drive
- Use `KebabCase` to rename the endpoints (do not change the root).

        Exchange: MasstransitRabbitMQ.Contract.IntegrationEvents:DomainEvent-SmsNotificationEvent
        --> Exchange: sms-notification-event

Publish: When sending 1 event, only 1 Exchange is created

Bus: sending 1 Command, 1 Exchange and 1 Queue will be created

    - Tips Name Producer and Consumer:
        + Publish used for Event(Ved + N)
        + Bus used for Command(V + N)

## Work Flow

![rabbitmq_flow](https://github.com/user-attachments/assets/f3804fc7-9d3a-4f65-96c0-6aadd6c204e9)


## Dead-Lettered Effects

The `reason` is a name describing why the message was dead-lettered and is one of the following:

- `rejected`: the message was rejected with the requeue parameter set to false

- `expired`: the message TTL has expired

- `maxlen`: the maximum allowed queue length was exceeded

- `delivery_limit`: the message is returned more times than the limit (set by policy argument delivery-limit of quorum queues).


![logic](https://github.com/user-attachments/assets/ce03b1a6-3bb6-4858-ac5d-06daff7c659e)

![time_to_live](https://github.com/user-attachments/assets/8a9263f8-e106-4b79-9c29-5895d3fb19e0)










