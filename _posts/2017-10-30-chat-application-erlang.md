---
layout: post
title: Chat Application with Erlang
tags: [Erlang]
---

Erlang is one of the less popular and known languages among the developer community which could deliver high availability using internal constructs of the programming language and the virtual machine running on beams ( similar to bytecode in java ). Whatsapp have been one of the most popular applications that has been using Erlang for its core handling millions of messages per minute. (Facebook also have been using Erlang for a while and now they have moved away from the Erlang) Not only Whatsapp, but also thousands of telco messaging servers run on Erlang providing high avaialability and fault tolerence.

In this episode we will be developing a simple chat application covering the basic design concepts in Erlang.

## Prerequisites

You will require Erlang installed in your machine and you can test for the correct installation by running `erl` command on the termincal of your machine.

## Structure

Application will consist of a chat server, chat state machines ( handling state of each chat ) and a chat client for you to start chats.

![Application Structure]({{ site.baseurl }}/images/chat-application/application-structure.png "Application Structure")

Chat server will be running as a background process waiting for incoming connections. Once a client connects the chat server, a chat state machine is spawned which will be handling all the events from the chat client. A reference to the chat state machine will be passed to the chat client upon the connecting witht the chat server, which will be used by the chat client for sending future messages.

![Messaging Structure]({{ site.baseurl }}/images/chat-application/messaging-structure.png "Messaging Structure")

## Coding

### Chat client

Chat client is a simple gen_server module communicating with the chat server and the fsm once a state machine has been allocated for the chat client. To start the gen_server, we will be prompting the user to provide his name through function parameters. ( We will write a seperate API function that will invoke a gen_server start_link)

```erlang
-module(chat_client).

-behaviour(gen_server).

-record(state, {handler_pid}).

start_link(Name) ->
    gen_server:start_link({local, ?SERVER}, ?MODULE, [{name, Name}], []).
```

gen_server start_link will spawn a gen_server with the `chat_client` module with Arguments `[{name, Name}]` and options `[]`. Which will call the `init/1` with the arguments passed.

In the initialization of the chat client gen_server, we will therefore get a proplist containing the name for the chat client to register with the chat server. Once the chat client recieves a handler pid from the chat server upon successful connection, handler pid will be saved in the state of the chat client.

``` erlang

init(Args) ->
  %%Name = proplists:get_value(name, Args), Can be used to get the name
  {value, {_, Name}} = lists:keysearch(name, 1, Args),
  io:fwrite("Name : ~p ~n", [Name]),
  case gen_server:call({global, chat_server}, {register, Name}) of
    {ok, HandlerPid} ->
      {ok, #state{ handler_pid = HandlerPid}};
    {error, Reason} ->
      io:fwrite("terminating chat_client Reason : ~p ~n", [Reason]),
      {stop, normal}
  end.

```
Then we will require method to send a message to another client. For the purpose, we willopen up another API function which will call the gen_server we have registerd through a `gen_server:call/3`

``` erlang
send_messge(Name, Message) ->
  gen_server:call(?SERVER, {send, {Name, Message}}).
```

This will be recieved by our `chat_client` thorugh `handle_call/3` which we have to send to the fsm dedicated for the chat client we have just created, through a `gen_fsm:event/2`.

``` erlang
handle_call({send, {Name, Message}}, _From, State) ->
  HandlerPid = State#state.handler_pid,
  Reply = gen_fsm:send_event(HandlerPid, {send, {Name, Message}}),
  {reply, Reply, State};
```

#### Assignment : implement methods to handle incoming messages, join updates

### Chat Server

Chat server is also a `gen_server`, which has the responsibility of handling incoming connections for our super awesome chat. It will spawn a `gen_fsm` for each request and passed the `pid` of the `gen_fsm` it spawns. Further, it will broadcast the new connections to all existing handlers.

``` erlang
-module(chat_server).

handle_call({register, Name}, From, State) ->
  {ClientPid, _Tag} =  From,
  Clients = State#state.clients,
  case gen_fsm:start_link(chat_fsm, [{clients, Clients}, {name, Name}, {client_pid, ClientPid}], []) of
    {ok, Pid} ->
      NewState = State#state{ clients = lists:concat([Clients, [{Name, Pid}]])},
      lists:foreach(fun({_UserName, UserPid}) -> gen_fsm:send_all_state_event(UserPid, {join, {Name, Pid}}) end, Clients),
      {reply, {ok, Pid}, NewState};
    {error, Reason} ->
      io:fwrite("gen_fsm start_link fail Reason : ~p ~n", [Reason]),
      {stop, normal, State}
  end.
```
#### Assignment : Decide state for gen_server, implement other required methods

### Chat handler

Chat handler is a `gen_fsm`, (newer construct is `gen_statem`) whichis responsible for a chat client. Each will contain information about which `fsm` is serving which client and routing decisions will be taken here.

```erlang
-module(chat_fsm).

-behaviour(gen_fsm).

-export([start_link/0]).

-record(state, {clients, client_pid, name}).

start_link() ->
  gen_fsm:start_link({local, ?SERVER}, ?MODULE, [], []).

init(Args) ->
  Clients = proplists:get_value(clients, Args),
  Name = proplists:get_value(name, Args),
  ClientPid = proplists:get_value(client_pid, Args),
  {ok, connected, #state{clients = Clients, name = Name, client_pid = ClientPid}}.

```

The fsm will have a single state `connected`, which will be the initial state and requires coding to handle incoming send or recieve message events.

``` erlang
connected({send, {RecieverName, Message}}, State) ->
        %%io:fwrite("Send ~p ~p ~n",[RecieverName, Message]),
  Clients = State#state.clients,
  SenderName = State#state.name,
  Reply =
  case proplists:get_value(RecieverName, Clients) of
    undefined ->
      {error, no_client};
    HandlerPid ->
      gen_fsm:send_event(HandlerPid, {recieve, {SenderName, Message}})
  end,
  {next_state, connected, State};

connected({recieve, {SenderName, Message}}, State) ->
%%io:fwrite("receive ~p ~p ~n",[SenderName, Message]),
  ClientPid = State#state.client_pid,
  Reply = gen_server:call(ClientPid, {recieve, {SenderName, Message}}),
 %%ClientPid !  {recieve, SenderName, Message},
  {next_state, connected, State};

connected(_Event,  State) ->
  Reply = ok,
  {next_state, connected, State}.

```

And complete the Application according to your preference.For instace you may centralize the routing function, or may use some persistancy with `mnesia` or some other database, and many more.

## Testing
#### Compile the code.

You can simply use `c/1` on the erl shell to compile you modules. Ot you can use erlc or emake if you are using a project template.


#### Start the chat server
```
erl -sname server -setcookie  chat
(server@bhanuka-Inspiron-3542)1> chat_server:start_link().
{ok,<0.41.0>}

```

#### Start a client 1.
```
erl -sname bhanuka -setcookie chat
(bhanuka@bhanuka-Inspiron-3542)1> net_adm:ping('server@bhanuka-Inspiron-3542').
pong
(bhanuka@bhanuka-Inspiron-3542)2> chat_client:start_link("bhanuka").           
Name : "bhanuka"
{ok,<0.80.0>}
```

#### Start client 2.
```
erl -sname rajith -setcookie chat
(rajith@bhanuka-Inspiron-3542)1> net_adm:ping('server@bhanuka-Inspiron-3542').
pong
(rajith@bhanuka-Inspiron-3542)2> chat_client:start_link("rajith").
Name : "rajith"
{ok,<0.52.0>}
```

#### Headover to client 1.
```
"rajith" Joined the Server
```

#### Say Hi to rajith
```
(bhanuka@bhanuka-Inspiron-3542)3> chat_client:send_messge("rajith", "Hi").
ok
```

#### Headover to client 2.

```
"bhanuka" : "Hi"
```
<img src="{{ site.baseurl }}/images/chat-application/i-know-erlang-now.jpeg" width="60%" alt="I Know Erlang Now!!" title="I Know Erlang Now!!">


## Notes

A method similar to the above is used in many telco application in the case of handling sessions from the mobile devices. For instance consider USSD, once you start a USSD session, a state machine will be spawned in the core server which is responsible for managing the sessions for your session. The same approach is used for call handling the core networks.

### Hire me

I currently work as a freelancer for projects in mobile applications, web applications, desktop applications, data mining
and machine learning. I provide my services mainly using Ionic framework, PHP, NodeJs, Java, electronJs, Erlang and Python.
You can directly contact me for your projects or can visit my Fiverr profile. Please contact me before placing an order.

[mahanama94 Fiverr](https://www.fiverr.com/mahanama94/)

Cheers !!!

**mahanama94**
