---
layout: post
title: Chat Application with Erlang - 1
tags: [Erlang]
---

Erlang is one of the less popular and known languages among the developer community which could deliver high availability using internal constructs of the programming language and the virtual machine running on beams ( similar to bytecode in java ). Whatsapp have been one of the most popular applications that has been using Erlang for its core handling millions of messages per minute.( Facebook also have been using Erlang for a while and now they have moved away from the Erlang) Not only Whatsapp, but also thousands of telco messaging servers run on Erlang providing high avaialability and fault tolerence. 

In this episode we will be developing a simple chat application covering the basic design concepts in Erlang. 

## Prerequisites

You will require Erlang installed in your machine and you can test for the correct installation by running `erl` command on the termincal of your machine. 

## Structure

Application will consist of a chat server, chat state machines ( handling state of each chat ) and a chat client for you to start chats. 

Chat server will be running as a background process waiting for incoming connections. Once a client connects the chat server, a chat state machine is spawned which will be handling all the events from the chat client. A reference to the chat state machine will be passed to the chat client upon the connecting witht the chat server, which will be used by the chat client for sending future messages. 

## Coding




## Testing 


## Notes

A method similar to the above is used in many telco application in the case of handling sessions from the mobile devices. For instance consider USSD, once you start a USSD session, a state machine will be spawned in the core server which is responsible for managing the sessions for your session. The same approach is used for call handling the core networks.

Cheers !!!

**mahanama94**