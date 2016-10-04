MQTTClient
=========================

Todo List
---------
These are the features planned to implement

- [ ] ~~Manage construction and destruction of a client in client pool in a manner of [RAII](https://en.wikipedia.org/wiki/Resource_Acquisition_Is_Initialization)

- [ ] ~~Sending subscriptions to each program connected to the client(The way it treats subscriptions now is to broadcast to the single shared memory region).

- [ ] ~~Sending logs generated while the program has executed to the broker.

Prerequisite
==========

- Paho MQTT C [here](https://eclipse.org/paho/clients/c/)

  AFAIK, there are many open-source program handling __MQTT__ protocol out there. We chose to work with __PAHO MQTT C__ client. It supports many features such as __SSL/TLS__ connection, automatic reconnection, non-blocking api and so on.

- Boost C++ Libraries [here](http://www.boost.org/users/download/)

  When it comes to lower level of network communication, _encoding_ is the essence of handling data between client and server and the __Boost C++ Libraries__ supports a variety of functions converting some encoding to others. So we used this to solve the encoding issue.

- CMake [here]

How to use it
==========

Installation
----------
* After installing all of the libraries in the __Prerequisite__ section, all you need to do to install the program is simple.</br>    

```
$ cd ENOW-client/
$ mkdir build
$ cd build
$ cmake --DCMAKE_BUILD_TYPE=Release ../
$ mkdir json
$ make -j($nproc)
```

Running
----------

The client program needs 3 command-line arguments.</br>

__`command-line arguments` :__
```
-a, --address : The ipv4 address of the broker
-k, --key : The key for allocating and identifing IPC resource
-i, --clientID : The identification name transferred to the broker to identify the device you're using now
```

The following example shows how the program run

```
$ sudo ./MQTTClient -a tcp://192.168.1.77 -i ENOW -k 7777
```

Registering your program
----------
After running the MQTTClient, you need to register your program. The way how to do this is as follow

1. Register __PROGRAM PROFILE__

The MQTTClient takes a json STRING as an input. 2 key-value pairs should be in the json STRING.

```JSON
{ "topic" : "topicToSend", "key" : key }
```

* The type of __"topic"__ is string
* The type of __"key"__ is integer

The following example should clarify the input string above

ex)
```JSON
{ "topic" : enow/serverID/brokerID/deviceID", "key" : 1234 }
```

After finishing entering the input, the program automatically allocate an IPC resource in your operating system.

2. Make your own program

The MQTTClient makes use of shared memory in most of the System V Unix operating system. So you should map the IPC resource previously allocated in the first step.

The following example shows how to make your own program.

```

```
