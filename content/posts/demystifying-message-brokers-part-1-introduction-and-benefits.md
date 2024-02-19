+++
title = 'Demystifying Message Brokers: Part 1 - Introduction and Benefits'
date = 2024-02-18T10:46:30+04:00
draft = false 
showPublicationDate = true
showReadingTime = true
toc = true
comments = true
tags = ["backend", "dotnet", "architecture"]
categories = ["Back End", "Guides", "Architecture"]
keywords = ["message brokers", "message broker definition", "message broker functionality", "message broker use cases", "how message brokers work", "advantages of message brokers", "message broker benefits", "why use message brokers", "messaging middleware", "enterprise messaging systems", "message queueing systems", "distributed messaging", "real-time messaging", "asynchronous messaging", "message routing", "message delivery guarantees", "message durability", "scalability with message brokers", "fault tolerance in message brokers", "message broker architectures", "high availability messaging", "message broker comparison", "message broker features", "message broker applications", "message broker deployment", "message broker scalability", "message broker reliability"]
description = "If you're into microservices, you'll eventually have to deal with message brokers. Getting ready for this switch will definitely boost your confidence and make you feel more prepared."
coverImage = "/images/message-brokers-pt1.webp"
coverImageAltText = "Meme about message brokers"
loading = "eager"
+++

{{<img src="/images/message-brokers-pt1.webp" alt="Meme about message brokers" >}} <br>

If you're into microservices, you'll eventually have to deal with message brokers. Getting ready for this switch will definitely boost your confidence and make you feel more prepared.

## What are message brokers?

Let's start with simple, what even is a _message broker_? I'll try to make it easy to understand at this point. Let's take a party for example. Every person wants to share some story, but instead of everyone shouting over each other, we will have one designated storyteller. This person listens to all the stories and shares them one at a time with the entire group. This person is our message broker at this moment, ensuring that all stories reach the group without chaos.

That's exactly how message brokers work, of course, there are more details into it, but it's their general concept.

## Message broker components

- **Publisher**/**Producer** - part of your project that sends message to a **Topic**. In **[Publish/subscribe](#publish-subscribe-messaging)** method (discussed below) they are generally called **Publishers**.
- **Consumer**/**Subscriber** - part of your or maybe separate project that consumes messages sent by **Publisher** via subscribing to a certain **Topic**.
- **Topic** - storage for all of your messages sent by **Publishers**.

## Message broker models

Since we've just mentioned details, let's dive into some of them. Message brokers offer two message distribution patterns, which are:

- **Point to point messaging**
- **Publish/subscribe messaging**

Let's dig into each of them.

### Point to point messaging

{{<img src="/images/point-to-point.webp" alt="Illustration showing point-to-point messaging">}}

With **Point to point messaging** each message in the queue is sent only to one recipient and is consumed only once. This distribution pattern is usually used in systems which require one-to-one relationship, example of which could be financial transaction processing. In such cases, both, senders and recipients need a guarantee, that each payment will be sent once only.

### Publish/subscribe messaging {#publish-subscribe-messaging}

{{<img src="/images/publish-subscribe-messaging.webp" alt="Illustration showing publish-subscribe messaging">}}

If you're using **Publish/subscribe messaging**, you may have one message consumed by multiple clients, or if using the right terms, subscribers. With this method, your producer publishes a message to a topic, where one or more subscribers might be waiting for it. This resembles one-to-many relationship, while one producer has many subscribers.

## Message brokers vs REST APIs

Since you're already working with back-end software, you're most likely familiar with **REST** APIs. Main question that you might be having, would be, why can't we use **REST** services instead of message brokers? Well, it turns out that you can and nobody is stopping you, but there are some caveats. You see, the reason for using message brokers is not just _we want to_. They bring a lot of good as well, and in terms of comparison with **REST** APIs, main advantage would be _fault tolerance_.

### Sending a message to microservice with REST API

Let's discuss a situation where you want to send some message to another microservice and you decide to use **REST** APIs. It might be working pretty good, but in this case you lack the same fault tolerance I told you about earlier. Your service might be running beautifully, have no bugs at all, but there's still a possibility that something goes wrong and your message will never reach destination. Later on you might be performing some checks on did the message reach the destination or if it's saved somewhere so that you could resend it to required service.

### Sending a message using message brokers

On the other hand, if you went with message brokers, you're way safer. Even if your app goes down, or anything else happens so that the service is unreachable, you can still be sure that once you spin up your project again, the message will be delivered right away. It doesn't even matter if you're using one-to-one or one-to-many relationship for your publisher and subscribers. Once your message got published and is added to a topic, you can be sure that whenever you require it, it will be available there. So it's a big win for message brokers.

## Summing up advantages of using message brokers

- **Guaranteed message delivery** - Even if your consumer/subscriber was offline, it will receive the message when it'll be back online.
- **Increased system performance** - Since message brokers work asynchronously, main thread of the application will not be affected.
- **Communicating between applications** - You can integrate communication between all applications even if they are written using different programming languages or frameworks.

## Drawbacks of using message brokers

- **Adaptation process** - There are multiple message brokers and different approaches of using them. You should be able to choose them wisely depending on your needs.
- **Debugging difficulty** - Since we're adding new components to our architecture, debugging also becomes more difficult.

## Most popular message brokers

- **Apache Kafka** - Developed by [Apache Software Foundation](https://www.apache.org/) with initial release in 2011. Written in Java.
- **RabbitMQ** - Initially developed by "Rabbit Technologies Ltd." in 2007, currently under [Pivotal Software](https://en.wikipedia.org/wiki/Pivotal_Software). Written in Erlang.

## Conclusion

I hope now you have a better understanding of how message brokers work and why using them could be so important. Even though we've talked about a lot, there's still so much to explore in the message brokers world, but it's fine since this article is just a beginning of a series. In the next part we're going to explore RabbitMQ more in detail and make a little project with it.

<hr class="border-gray-300 dark:border-gray-600 my-4">

Thank you for taking the time to read! :)
