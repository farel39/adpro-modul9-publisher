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

## Proof of Running RabbitMQ as message broker.
![Running RabbitMQ](Running%20RabbitMQ.png)

## Proof of Sending and Processing Event
![Proof of Sending and Processing Event](Sending%20and%20Processing%20Event.png)
The terminal on the left shows the publisher app being run two times. On the right we see that the subscriber app receiving messages from the publisher app. 

When you I the publisher with cargo run, it sends 5 user creation events to RabbitMQ with different user IDs (1-5) and names (Amir, Budi, Cica, Dira, Emir). These events are formatted as UserCreatedEventMessage objects.
The subscriber  connects to RabbitMQ at "amqp://guest:guest@localhost:5672" and listens on the "user_created" queue. When messages arrive, the UserCreatedHandler.handle() method processes each message and prints it to the console.

## Proof of Monitoring Chart based on publisher.
![Proof of Monitoring Chart based on publisher.](Monitoring%20chart%20based%20on%20publisher.png)
The image shows a chart on RabbitMQ that has several spikes that are directly proportional to the time when I run the publisher program several times in the terminal. This means that the spikes are the result of publishing events from the publisher.