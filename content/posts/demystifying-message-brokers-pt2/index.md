+++
title = 'Demystifying Message Brokers: Part 2 - RabbitMQ'
date = 2024-02-24T08:04:05+04:00
draft = false 
showPublicationDate = true
showReadingTime = true
toc = true
comments = true
tags = ["backend", "architecture"]
categories = ["Guides", "Back End", "Architecture"]
keywords = ["RabbitMQ", "RabbitMQ installation", "docker container for RabbitMQ", "C# RabbitMQ tutorial", "creating a publisher application", "creating a consumer application", "RabbitMQ basic setup", "RabbitMQ event-based consumer", "RabbitMQ message handling", "testing RabbitMQ applications", "real-world RabbitMQ usage", "microservices architecture with RabbitMQ"]
description = "In the previous part we've talked about message brokers in general and I hope you got a brief understanding of how they work and why you'd need them. In this article, we're going to dive deeper and explore one of them - **RabbitMQ**."
coverImage="/images/20-minute-adventure.webp"
coverImageAltText="Cartoon illustration featuring characters from 'Rick and Morty' engaged in a humorous exchange about a '20-minute adventure'"
slug = "demystifying-message-brokers-pt2"
loading = "eager"
+++

{{<img loading="eager" src="/images/20-minute-adventure.webp" alt="Cartoon illustration featuring characters from 'Rick and Morty' engaged in a humorous exchange about a '20-minute adventure'" >}} <br>

In the [previous Part](/posts/demystifying-message-brokers-pt1) we've talked about message brokers in general and I hope you got a brief understanding of how they work and why you'd need them. In this article, we're going to dive deeper and explore one of them - **RabbitMQ** and we'll be using C# for this purposes.

## Installing RabbitMQ

First thing you'd want to do is of course install RabbitMQ. Easiest way to do this is by running a docker container with it, as stated in RabbitMQ docs you can do it with a following command

```
docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.13-management
```

Now let me just clarify what this command does:

- **-it** - Runs a container in an interactive mode, in this case it will just be used to let you see the running logs which might help if you run into any problem.
- **--rm** - This flag tells docker to remove the container once you exit it, generally used for testing purposes or short-lived services.
- **--name** - Name of the container running.
- **-p** - Maps the container port to our machine port, in this case port `5672` is used for AMQP (Advanced Message Queuing Protocol) and `15672` for RabbitMQ's management UI.
- **rabbitmq:3.13-management** - Image that we're going to be using, `management` keyword indicates that we also will be running management web-based UI.

> After creating this docker container, you'd want your terminal window to be open, so that container will not exit and remove itself.

If you want to run RabbitMQ as a background container and decide later when to exit and delete it, you could remove a few flags and run it as follows

```
docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.13-management
```

> **-d** flag indicates detach, run in background.

After you're done with it, you can verify that your container is actually running via following command

```
docker ps
```

You should see your RabbitMQ container running, which means we're good to go.

## Creating the publisher application

As you already know, message brokers require at least two applications to have them work properly, one of which will be a producer (publisher) and the second one would be a client (consumer).

Let's start by creating our publisher application, we can easily do this via dotnet CLI.

```
dotnet new console --name Publisher

// For the better understanding, let's change the name of the main file
// If you're running Linux
mv Publisher/Program.cs Publisher/Publisher.cs

// If you're running Windows, you can use Powershell
Move-Item -Path "Publisher/Program.cs" -Destination "Publisher/Publisher.cs"
```

After creating our project, we need to add the RabbitMQ.Client NuGet package to our project.

```
cd Publisher
dotnet add package RabbitMQ.Client
```

Once the package is installed, we're ready to start actually making our publisher.

First of all, let's build ourselves a connection to RabbitMQ instance we're running in a container.

```csharp
using System.Text;
using RabbitMQ.Client;

var factory = new ConnectionFactory { HostName = "localhost" };
using var connection = factory.CreateConnection();
using var channel = connection.CreateModel();
```

At this moment, we're already connecting to our instance, but let's have a brief explanation of what actually is happening here.

First of all, `ConnectionFactory` abstracts a lot of things for us, such as authentication and so on, but the key part which you'll be using for interacting with RabbitMQ is a `channel`, this is where all the API resides.

RabbitMQ uses queues for messages, think of it as a post box, so at first, we must create a queue where we're going to send our messages.

```csharp

channel.QueueDeclare(queue: "hello-world",
                     durable: false,
                     exclusive: false,
                     autoDelete: false,
                     arguments: null);
```

You don't really need to understand all the variables we're passing right now, but you'll need your queue name later when we'll be building our consumer application.

> Creating a queue in RabbitMQ is an idempotent action, so a queue will only be created if it doesn't exist, otherwise, it'll just ignore it without any error.

If you run your code right now and navigate to `http://localhost:15672`, you'll be able that your queue has already been created, but there are no messages or consumers, but it's fine.

For logging in to RabbitMQ's web UI, you'll need to use `guest:guest` username and password. Now, we can continue and actually add a message to our queue.

```csharp
const string message = "Hello!";
var body = Encoding.UTF8.GetBytes(message);

channel.BasicPublish(exchange: string.Empty,
                     routingKey: "hello-world",
                     basicProperties: null,
                     body: body
                    );
Console.WriteLine($"Message successfully sent, text - {message}");
```

Messages in RabbitMQ are transported as byte arrays, so we need to turn our string into one. One thing to notice here is the `routingKey`, publishers use it to describe the message's destination and consumers use it to read only the messages that they need.

At this point, if you run your code, you'll be able to see that a message has been successfully sent and you can verify it in the web UI. You could even read it there right away, but keep in mind that once the message has been read, it's being deleted from the queue, so you might need to resend it to read from the actual consumer.

> Keep in mind that, reading a message is a destructive action

{{<img src="/images/rabbitmq-queue.webp" alt="A meme where one person gives another person a piece of paper with text, likely related to message brokers" >}} <br>

Finally, you'll be looking at the code snippet like this one, at this point, our publisher is ready.

```csharp
using System.Text;
using RabbitMQ.Client;

var factory = new ConnectionFactory { HostName = "localhost" };
using var connection = factory.CreateConnection();
using var channel = connection.CreateModel();

channel.QueueDeclare(queue: "hello-world",
                     durable: false,
                     exclusive: false,
                     autoDelete: false,
                     arguments: null);

const string message = "Hello!";
var body = Encoding.UTF8.GetBytes(message);

channel.BasicPublish(exchange: string.Empty,
                     routingKey: "hello-world",
                     basicProperties: null,
                     body: body
                    );
Console.WriteLine($"Message successfully sent, text - {message}");
```

## Creating the consumer application

Once you're done with your publisher application, you'd want to read the messages sent by it as well, for this purposes, we're going to use the consumer application, so let's first create it. We're going to be using the same commands we used to create the publisher application.

```
dotnet new console --name Consumer

// For the better understanding, let's change the name of the main file
// If you're running Linux
mv Consumer/Program.cs Consumer/Consumer.cs

// If you're running Windows, you can use Powershell
Move-Item -Path "Consumer/Program.cs" -Destination "Consumer/Consumer.cs"
```

Once again, we're going to add the RabbitMQ.Client package as well.

```
cd Consumer
dotnet add package RabbitMQ.Client
```

After going through this, we're ready to dive into creating our consumer application, basic setup is the same again.

```csharp
using System.Text;
using RabbitMQ.Client;

var factory = new ConnectionFactory { HostName = "localhost" };
using var connection = factory.CreateConnection();
using var channel = connection.CreateModel();

channel.QueueDeclare(queue: "hello-world",
                     durable: false,
                     exclusive: false,
                     autoDelete: false,
                     arguments: null);

Console.WriteLine("Consumer waiting for messages...");
```

> Note that queue name should be matching with the one you used while declaring it in the producer project.

Once we have the basic setup done, we'll need to start actually reading messages and RabbitMQ does this through event handlers.

```csharp
var consumer = new EventingBasicConsumer(channel);
consumer.Received += (model, ea) =>
{
    var body = ea.Body.ToArray();
    var message = Encoding.UTF8.GetString(body);
    Console.WriteLine($"Received a message - {message}");
};

channel.BasicConsume(queue: "hello-world",
                     autoAck: true,
                     consumer: consumer);

Console.ReadLine();
```

Let's have some deep dive into what's going on in here and how it all works.

First, we're creating an event based consumer attached to our channel, which later on is using the `Received` event to track new messages. As I've already mentioned, messages are just byte arrays for RabbitMQ, so we need to decode it back into string. Later on, we just notify RabbitMQ that the message has been successfully consumed by our consumer instance and it's safe to delete it.

## Testing interaction between our applications

If you run the `Publisher` application right now and follow-up by running your `Consumer` application, your console output will look like something similar to this

```
cd Publisher
dotnet run

Output:
Message successfully sent, text - Hello!

cd Consumer
dotnet run

Output:
Consumer waiting for messages...
Received a message - Hello!
```

You can continue experimenting with it, or trying to run your producer multiple times and then see if all of your messages actually get consumed by your consumer application. That would be it for today and I'll most likely do another part covering RabbitMQ usage in a real world application with microservices architecture. Hope this article helped you better understand how to work with RabbitMQ.

<hr class="border-gray-300 dark:border-gray-600 my-4">

Thank you for taking the time to read! :)
