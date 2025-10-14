# Introduction 

## What is WebSub ?

<a href = "https://www.w3.org/TR/websub/">WebSub</a> is a W3C recommendation that defines a simple, open, and decentralized protocol for real-time publisher-subscriber communication over HTTP(S). 

In WebSub, publishers and subscribers are decoupled through an intermediary called a Hub. The Hub, which functions similar to a lightweight message broker, receives content updates for topics from publishers and distributes them to all registered subscribers.

This model enables low-latency content delivery, reduces unnecessary network traffic, and provides a standardized way to build event-driven systems on top of HTTP(S).

## What can I use WebSub for ?

WebSub enables real-time event streaming over HTTP(S) and can be applied across a variety of industries and organizations. Some common use cases include:

* To serve as a foundation for building event-driven architectures over the web.
* To notify apps, websites, or partners about new orders, inventory changes, or price updates in retail and e-commerce platforms.
* To stream system alerts and notifications, such as operational status, downtime alerts, or performance metrics to dashboards, mobile apps, or administrative teams in real-time.
* To push updates from news sites, or media outlets to subscribers in real-time.

## What is WSO2 Integrator: WebSubHub ?

**WSO2 Integrator: WebSubHub** is a <a href = "https://www.w3.org/TR/websub/">WebSub</a> compliant hub implementation written in <a href = "https://ballerina.io/">Ballerina programming language</a>. It enables organizations to adopt event-driven architectures by supporting publish-subscribe communication over HTTP(S). WSO2 Integrator: WebSubHub can be configured with the message broker of your choice as its persistence layer, making it ideal for scalable, real-time event distribution across services, applications, and systems.

### How does it work ?

The WSO2 Integrator: WebSubHub distribution is composed of two main components: the WebSubHub and the Consolidator. The WebSubHub is a WebSub-compliant hub implementation that manages topics, processes subscription requests, and delivers content updates to subscribers. The Consolidator is responsible for managing the overall system state.

The WSO2 Integrator: WebSubHub system uses a message broker as its persistence layer. Messages related to topics, as well as system state information such as registered topics and subscriptions, are stored within broker topics. This approach ensures fault tolerance, scalability, and a clean event-driven architecture.

<a href="{{base_path}}/assets/img/get-started/introduction/websubhub-architecture.png"><img src="{{base_path}}/assets/img/get-started/introduction/websubhub-architecture.png" alt="WSO2 Integrator: WebSubHub" width="75%" style="padding-top: 20px" ></a>

### Main concepts and terminology

#### Topic

Represents the resource that subscribers are interested in receiving real-time notifications about.

#### Message

The content delivered to subscribers when a topic is changed or updated. It usually contains the updated resource or a notification containing the updated state of the resource.

#### Publisher

The entity that creates and maintains a topic. Whenever the publisher updates the content, it notifies the hub that a new message is available for that topic.

#### Subscriber

A client or application that wants to receive real-time updates about a topic. It subscribes to a topic via the hub and receives messages whenever the topic is updated.

#### Hub

The intermediary that manages subscriptions and distributes messages. Publishers notify the hub of updates, and the hub pushes these updates to all registered subscribers.

## Features

* At-least-once message delivery guarantee for subscribers.
* Automatic retries for message delivery in case of transient subscriber endpoint failures.
* Failure recovery and seamless resumption of operations with minimal or no downtime.
* Seamlessly integrates with external IdPs to provide authentication and authorization for publishers and subscribers.

## Where to from now ?

To get hands-on experience with WSO2 Integrator: WebSubHub, follow the [Quick Start Guide](quickstart.md).
