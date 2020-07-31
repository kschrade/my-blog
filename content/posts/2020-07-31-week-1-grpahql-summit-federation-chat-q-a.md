---
template: post
title: Week 1 GrpahQL Summit Federation chat Q/A
slug: GQL-Summit-w1-qa
draft: true
date: 2020-07-31T11:47:44.516Z
description: Last week was the first week of the GraphQL summit. Here's a recap
  of all of the questions and answers I was involved in.
category: GraphQL
tags:
  - GraphQL
  - Tech
  - GraphQL Summit
---
\[intro]

### Rate limiting 

A question arose of how do you do rate-limiting in the graph. There are a few things you can to do rate limit in GraphQL. The easiest thing to do (and yes this is a cop-out) is put your graph behind an API gateway and let the gateway handle the limiting. If you wanted to roll your own solution you could get away with some apollo plug-ins.  This means you have to do EVERYTHING, and that's asking a lot.

### How are you using GraphQL? 

At StockX we use GraphQL to stitch all of our data together. We have basically replaced the mental mapping of data into a graph. For example, instead of getting a product UUID back from a search and then having to go get the product, you can just make a single query and get all the product information you want back. Grated that a super simple example but the general idea is there.  

### Can you use GraphQL to do your service to service communication?

Yes, you can! But be careful! You get the benefits from GraphQL, single source of truth, stitching of data, etc. All of this is great but it can also lead to a few problems. This will increase the number of requests on your gateway and federated servers. This will cause an increase in latency, more hops equal more time. The less obvious problem is you will sometimes create awkward paths that can cause problems. At StockX we witnessed service A making a call into our graph then as the graph is resolving the request it calls service A again. This loop lead to a few problems with sockets. So yea you can do it, just be careful. 

### How do federations work?

You create a set of federated services that all serve up a portion of your full graph. The GraphQL gateway then pulls all the federations together and stitches all the schemas together into a single graph. 

### How do you do a public and private Graph?











public vs private \
https://discord.com/channels/733693158499549286/738397370621886476/738434463339511909

how do you handel breaking changes? 

https://discord.com/channels/733693158499549286/738397370621886476/738438035414188183

why use federations

https://discord.com/channels/733693158499549286/738397370621886476/738438703252373626

monorepo?

https://discord.com/channels/733693158499549286/738397370621886476/738438891236884628

https://discord.com/channels/733693158499549286/738397370621886476/738439460579967027

how do you cache?

https://discord.com/channels/733693158499549286/738397370621886476/738441796501110904

additional gateway functionality

https://discord.com/channels/733693158499549286/738397370621886476/738442664894005389

scaler issue:

https://discord.com/channels/733693158499549286/738397370621886476/738454021634785390