---
layout: post
title: Building Distributed Systems with Erlang
tags: [Erlang]
---

Erlang is a concurrent, distributed and functional programming language that runs on the Erlang virtual machine. Erlang Virtual Machine and the libraries that we use today were designed by Ericsson few decades back for building reliable, distributed systems fot telecommunication. Thus the Erlang has the distributed computing concept directly in the construct of the virtual machine.

## Application Structure

Erlang applications are started and terminated as a single unit on any Erlang node that might reside in same or different servers. Thus each node might offer different serrvices to others.

For instance consider a typical web application. A possible architecture of a chat application can be broken down in to smaller appplications performing a certain service as follows,
* HTTP Service - Handling web traffic.
* Log Service - Logging activities.
* Database Service - Connecting with Databases for accessing data.
* Reporting Service - Reporting behaviors, anomalies.
* Analytics Service - Performing analytics and Providing insights.

## Communication

In a typical distributed system, you would need a communication protocol to communicate between different services running on different servers across a network. In the todays context many opt to go with JSON over HTTP, which can be very expensive for your application as it scales.

However, in Erlang has a communication protocol to handle the communication between 2 different nodes. All you will have to do is share the same cookie with the servers and they will be visible to each other.

Further, the distribution is transparent with no difference between communication between processes in the same node or different nodes. Thus there will be no change for your application code built with message passing.

## Failures

As Earlier mentioned, the distribution of the Erlang is transparent, if a communication failure or a node failure occurs between your nodes, it will be interpreted as a process failures.

In the case of application having redundant nodes for same task, the transition between the redundant in the event of a failover will be invisible.

## Sample

![Application Structure]({{ site.baseurl }}/images/distributed-systems/sample-application.jpg "Application Structure")


As you might notice, the above application suggests us with the micro services architecture, which can make the post too much too much.

Cheers !!!

**mahanama94**
