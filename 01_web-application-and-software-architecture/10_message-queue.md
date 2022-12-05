# Message Queue
- Queue that routes messages from source to destination;
- Facilitates async behaviour and cross-module communication, which is key in service-oriented or microservices architecture;
- Provide temporary storage for messages until they are processed and consumed by the destination/consumer;
- Supports adding rules to the routing of the messages, such as priority, message acknowledgements, retrying failed messages;
- No definite size;

# Messaging Models

### Publish Subscribe
- Multiple consumers receive the same published message;
- Message queues in this model have _exchanges_, which pushes the messages to the queue based on the exchange type;
- Different types of exchanges:
  - direct;
  - topic;
  - headers;
  - fanout;

### Point to Point
- A message published by a producer is consumed by a single consumer;

# Messaging Protocols
- The 2 most popular protocols:
  - AMQP- Advanced Message Queuing Protocol;
  - STOMP - Simple (or Streaming) Text Oriented Messaging Protocol;
