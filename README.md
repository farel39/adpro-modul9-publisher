# Questions & Answers

## a. How much data your publisher program will send to the message broker in one run?

Looking at the publisher code, it will send 5 UserCreatedEventMessage objects to the message broker in one run. Each message contains:

* A user_id (values "1", "2", "3", "4", "5")
* A user_name (values "2306152424-Amir", "2306152424-Budi" "2306152424-Cica", "2306152424-Dira", "2306152424-Emir")

These 5 messages are published to the "user_created" queue one after another in a single execution of the program.

## b. The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber program, what does it mean?

The URL "amqp://guest:guest@localhost:5672" is the connection string for both the publisher and subscriber to connect to the same RabbitMQ message broker. Breaking it down:

* amqp:// - Protocol (Advanced Message Queuing Protocol)
* guest:guest@ - Username:password credentials (using default RabbitMQ credentials)
* localhost - Host where RabbitMQ server is running (on the same machine)
* :5672 - Port number (default port for RabbitMQ)

Using the same connection string in both programs means they're connecting to the same RabbitMQ instance. This is necessary for the message passing to work - the publisher sends messages to this broker, and the subscriber retrieves them from the same broker. It's the foundation of the pub/sub messaging pattern being implemented with the CrosstownBus library, which is a wrapper around RabbitMQ.