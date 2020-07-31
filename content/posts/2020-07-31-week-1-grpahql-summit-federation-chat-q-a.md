---
template: post
title: Week 1 GrpahQL Summit Federation chat Q/A
slug: GQL-Summit-w1-qa
draft: true
date: 2020-07-31T19:40:48.283Z
description: Last week was the first week of the GraphQL summit. Here's a recap
  of all of the questions and answers I was involved in.
category: GraphQL
tags:
  - GraphQL
  - Tech
  - GraphQL Summit
---
In the past week, I attended the GraphQL Summit. To get out into the community, and complete a personal OKR, I answered a bunch of questions and participated in numerous discussions. Here are some of the highlights of the weeks in the federation channel.

### Rate limiting

A question arose of how do you do rate-limiting in the graph. There are a few things you can to do rate limit in GraphQL. The easiest thing to do (and yes this is a cop-out) is put your graph behind an API gateway and let the gateway handle the limiting. If you wanted to roll your own solution you could get away with some apollo plug-ins. This means you have to do EVERYTHING, and that's asking a lot.

### How are you using GraphQL?

At StockX we use GraphQL to stitch all of our data together. We have replaced the mental mapping of data into a graph. For example, instead of getting a product UUID back from a search and then having to go get the product, you can just make a single query and get all the product information you want back. Grated that a super simple example but the general idea is there.


![a cat sewing](https://media.giphy.com/media/l41YqlWuKy3qhIWsw/giphy.gif)

### Can you use GraphQL to do your service to service communication?

Yes, you can! But be careful! You get the benefits from GraphQL, single source of truth, stitching of data, etc. All of this is great but it can also lead to a few problems. This will increase the number of requests on your gateway and federated servers. This will cause an increase in latency, more hops equal more time. The less obvious problem is you will sometimes create awkward paths that can cause problems. At StockX we witnessed service A making a call into our graph then as the graph is resolving the request it calls service A again. This loop leads to a few problems with sockets. So yea you can do it, just be careful.

![power picture](https://i.ytimg.com/vi/QSKiDEMxUog/maxresdefault.jpg)


### How do federations work?

You create a set of federated services that all serve up a portion of your full graph. The GraphQL gateway then pulls all the federations together and stitches all the schemas together into a single graph.

### How do you do a public and private Graph?

There are a few ways to go with this. The current way people seem to be doing this is by creating multiple gateways. These separate gateways consume only the federations they want to expose, ie the public gateway does not consume internal federations. The other way people have wanted to do this is by using a schema directive. This was opened as an [issue](https://github.com/apollographql/apollo-server/issues/2812) opened to do this. What the issue talks about is having a directive (an annotation in the schema) that would hide fields from public view. I think this would be a better way. Developers who create federations already use to this idea for things like `@external, @required, @provides`.

### Why use federations?

Federations allow for some great separation of concerns. Each concern can easily be split out into its own federation. They allow for separate scaling, allowing federations to be more cost-effective. It also allows for isolation of scale, not every federation needs to be on steroids. Having your graph split into multiple multiple sections also allows it to be colocated with the service that provides the graph with that data. They are easier to gain adoption around. Each federation can be managed by a smaller group of people, allowing for fewer people to have to sign off on changes and approve standards.

### Can you set your federations in a monorepo?

YESSSSSSS!!! We do it at StockX and I would recommend it! You don't have to use a monorepo, but I think it works way too well.

![happy picture](https://media.giphy.com/media/UO5elnTqo4vSg/giphy.gif)

### How do you handle breaking changes?

We have taken a page out of the Protocol Buffers and made almost everything in our schema nullable, so we rarely have a breaking change. When we do have a breaking change we normally do one of two things. If it's not used, sadly, we just push and have things update after the push. It's normally not ideal to break other developers' code but it's better than allowing bad patterns that will be removed. The other way is adding the \`@deprecated\` tag to the field. This will hide the field from everyone but not break the current code. After pushing this change we monitor the use of the field. Once the field is no longer used, we remove it from the schema.

### How do you cache?

I mainly work on the server-side of the GraphQL, so I can't speak about caching on the client-side but I can speak on the server-side. On the server-side, we cache our calls from GraphQL to a backend service. We also have an opt-in model with our caching. This is super helpful when trying to decide what to cache. We can watch our monitoring and then introduce caching where it is needed. We also introduce caching where we know it is needed. It also elevates the problem of caching personal information. When you have an opt-in model, it's super simple, you just don't opt-in.

### What additional functionality can I add to GraphQL Gateway?

The gateway is a fancy express server, and that means the sky's the limit! There are a few plug-ins that are already made around security, the two common ones I see are a bit limit and a query depth limit plug-in. After that, you can attach any express plug-in to your gateway.