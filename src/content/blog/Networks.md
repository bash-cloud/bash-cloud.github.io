---
title: Computer Networks Deep Dive
author: Bash
pubDatetime: 2024-10-27T01:00:00Z
slug: Networks
featured: true
draft: false
tags:
  - Networks
  - OSI 
  - TCP/IP
ogImage: ""
description: Computer Networks

---

---

Computer Networks Deep Dive
Networks. The world we live in today is run mainly driven by the internet. Majority of the world population spend a lot of time surfing the web, some of us technical workers even get our livelihood from the internet. So what is the internet and what is it all about?
According to wikipedia the internet is a global system of interconnected computer networks. I have been curious about computer networks over my professional career since everything we know of today, all the innovation and developments we see around us today is all thanks to computer networks that allows us to receive, transmit and analyze data.
In this series we will study the book Computer Networks by Andrew S. Tanenbaum and learn all there is to know about computer networks. We will then go ahead and learn plus explore how we can exploit different aspects of the computer networks. I will be writing this series as a guide book for beginners wanting to learn about computer networks while keep it interesting for experts who want to have a refresher on the basics. 
Someone once quoted " The most interesting part about computers is how we get them to talk to each other". There are many ways we can get devices to communicate, from the commonly known WiFi to Bluetooth and even RFID to many other forms of network communication that we will cover. 

---

Network Reference Models
In Computer Science, a concept called abstraction is widely used to reduce complexity of system operations. Abstraction is where the internal workings of a hardware or software is hidden and only an output is provided from a process. The underlying algorithms and process applied to achieve that specific output is not shown. 
Networks apply a similar concept where there are stacks of layers that offer certain services to the layer above it. Think of it as a hierarchical system where the layer above is dependent on the layer below. Different forms of networks have different layers and provide different services. Layer n from machine A uses layer n protocol to communicate with layer n of machine B. Protocol therefore is the agreed standard of communication between each layers. 
Each layer has a defined set of functions that is has to achieve and interfaces it uses to communicate with the next layer. To help us understand these layers deeper let us look at two network reference models:
The OSI Reference model 
The TCP/IP Reference model

The Open Systems Interconnection (OSI) Reference Model was revised in 1995. The Protocols associated with this model are not in use today but the model itself is valid as it will help us understand abstraction in networks better. The model has seven layers:
<img src="/assets/osi.png" class="sm:w-1/2 mx-auto" alt="osi model">
Each layer has a set of functions , data unit and protocols. The physical layer will mostly have Telecommunication concepts but it will be important to understand as we delve deeper into the science behind it. We will study the layers in detail later.
The TCP/IP Model was first described in 1974 and later defined as a standard by the internet community in 1989. It is similar to the OSI model with a few key differences. The model only has 4 layers and the services in the missing layers are encapsulated in the adjacent layers.
<img src="/assets/tcpip.png" class="sm:w-1/2 mx-auto" alt="tcpip model">

The main differences between the OSI and the TCP/IP layers are:
OSI model has 7 layers while the TCP/IP has 4 layers
The OSI model has both connection-less and connection-oriented service in the network layer and only connection-oriented communication in the transport layer while TCP/IP supports only connection-less in the network layer and both in the transport layer giving users a choice.
Error Handling is built in the Data link and transport layers in OSI while error handling is built in the protocols in TCP/IP.

In general, TCP/IP is preferred more today than the OSI model. By the time the OSI model was to be implemented the TCP/IP was already widely in use by most research institutes and universities. The OSI model and protocol was flawed and it had bad politics around it.

---

One of the things I have learned reading this book is that politics plays a major role in what is implemented and what flops in the science world. There are governing bodies that govern standardization of computer networks. Some of these bodies include: ISO ( that deals with a wide range of items beyond tech apparently), NIST, IEEE.
Research on new standards are issued through technical reports called RFCs ( Request for Comments). The IRTF (Internet Research Task Force) and the IETF (Internet Engeneering Task Force) were created as subsidiaries to IAB ( Internet Architecture Board). The IRTF focused on long term research while the IETF dealt with short-term engineering issues. To make a new standard, an idea is first submitted as an RFC to become a proposed standard if it interests the internet society. A working implementation is developed and tested by two independent sites for atleast 4 months to become a draft standard. The IAB then declares it and internet standard if it is convinced the software works.
In the next part of the series we will explore more on computer networks, cover each layer of the reference models mentioned and learn about them in-depth. See you in the next one!

References:
Computer Networks by Andrew S. Tanenbaum