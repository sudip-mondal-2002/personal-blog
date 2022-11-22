---
layout: post
title:  "Ludo game - System design"
date:   2022-11-23
excerpt: "T

re is a lot of users who love to play online multiplayer games. Ludo is one of the simple multiplayer games. Hence, we'll talk about it's system today."
---
Ludo is a great game for both children and adults alike- especially people looking for something fun to do with their friends. 
It can be played in various ways depending on how many people are playing and what variations they prefer to use. 
There are even competitions based on local gaming clubs where players from all over participate in tournaments and regional competitions. 
Ludo is an ancient game with many variations that's still enjoyed today! 

We've all enjoyed the sneaky and difficult wins with our friends in the game and every time we play how this system works consistently and accurately in real time and with very low latency How is this app able to manage and scale an unprecedented user base?
How much engineering work went into bringing us this amount of entertainment. 
I've been thinking about this for a long time. 
From my basic knowledge of large scale system development, the tools I used and the basics of system design With that knowledge in mind, 
I'm going to go the LUDO - Designing Scalable and User Satisfying Games. 
If you are reading this, I accept constructive criticism. 

## Breaking down of problem statement

### Functional Requirements
- Users can signup and login to their account
- Users can create and join games, at least 2 and at most 4 players can join a game
- Players can roll a dice, perform valid actions with their pawns according to their dice roll

### Non-functional requirements
- Consistency: The data across all the clients should be exactly the same or it'll create a huge chaos.
- Availability: The application must be available and must be fault-tolerant. 
- Throughput: The servers must accept huge number of requests and must not make any request in the waiting state to perform.
- Scalability: Ludo is often very much played in occasions when everyone get some time to spend together and demand increases significantly.


## The system architecture

Here is the architecture I propose. Let's have a look and I'll explain step by step.

![Thumbnail](/assets/images/ludo_system.jpg)

### Users and Matchmaking service

The users service is simple authentication system which can be used by the users to login and sign up to the application. 
The matchmaking service will be used to create games between different users.
Both the service have their own database but there will be a lot of data redundancy (as same users will be there for matchmaking). 
So we need a replication database to make sure data between these two database is consistent.
Another approach could be that we keep a messaging queue in between these two services and push data from users service to matchmaking service. 
But this will not ensure consistency at the database level, as well as it may create extra load on the matchmaking service.
Matchmaking service database can be updated with only logged in users. That way matchmaking can be very fast as the query will be run on a small amount of users.
Matchmaking service will be serving a lot more number of requests, hence there will be a bottleneck at the capacity of this service. 
To handle this, I propose to horizontally scale this server.

### Game service

This service will be responsible for all the gameplay. 
So definitely it'll be taking most of the requests coming to the server. 
This server will be responsible for redirecting the moves of each player to other players in the game. 
It'll be connected to an Apache kafka server (or any other message queue) to serve the asyncronous communication between different servers.
In addition to this, if some user get's disconnected due to network issues, once he/she joins back, he should get the current state of the game.
So, the server needs to keep track of the moves. This will need a database associated to it. As, the database will be continuously getting updated.
Horizontal scaling has a greater overall capacity than vertical scaling and that makes NoSQL databases the preferred choice for large and frequently changing data sets.
We're able to handle higher traffic by sharding, which adds more servers to your NoSQL database.

**It should be kept in mind that one game should be served by only one server.**

### The API Gateway

The API gateway will be acting as a reverse proxy in front of the backend servers and connce the clients with them. 
The clients should be connected with websockets as we need an event driven architecture as well as persistent connections. 
This will allow us to send one users move to other users without creating a chaos of multiple HTTP requests.

### The Orchestrator

The orchestrator will keep things at one place. It'll be the context of the system. 
It can also be used as the config server also to keep the secrets at one place and share between different srvers.

### Static Content host

This will be a simple server (maybe with distributed caching) that serves the images, fonts and html pages for the application if needed.



