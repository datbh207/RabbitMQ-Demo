
# Some Notes

Duration: When stopping RabbitMQ, the messages will be saved below the drive

Producer:

    - Create Exchange:
        + Based on the Class of the Message sent to create an Exchange
        + Based on the interfaces/classes that the message tries to create more Exchanges

NOTE: Use `[ExcludeFromTopology]` to remove redundant Exchanges
````bash
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

Consumer:
    
    - Create Queue: Create Queues based on the class inheriting IConsumer
    - Create Exchange:
        + Create an Exchange similar to the Queue name
        + Create an Exchange with the name of the Message

- Use `KebabCase` to rename the endpoints (do not change the root).

        Exchange: MasstransitRabbitMQ.Contract.IntegrationEvents:DomainEvent-SmsNotificationEvent
        --> Exchange: sms-notification-event

Publish: When sending 1 event, only 1 Exchange is created

Bus: sending 1 Command, 1 Exchange and 1 Queue will be created

    - Tips Name Producer and Consumer:
        + Publish used for Event(Ved + N)
        + Bus used for Command(V + N)

