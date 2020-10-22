---
template: post
title: Caching and GraphQL Federations
slug: caching-and-graphal-federations
draft: true
date: 2020-10-20T23:11:50.572Z
description: Let's take a dive into a caching and how you can use a remote cache
  to improve your GraphQL Federation proformance.
category: GraphQL
tags:
  - GraphQL
  - Apollo Federations
  - cache
---
GraphQL Federations and caching present a fascinating problem; how do you cache efficiently over many services and many requests?

# Let's start a journey through caching

Every good journey has a starting point, and ours is a super simple ecosystem with a GraphQL gateway, a GraphQL federated service behind that gateway, and a service feeding data to that federation.

![](/media/basic-server.jpg)

As you can see, every time a request comes into the gateway, it will always turn into a request to the underlying service.

# Caching, our first step

When we look at the picture above, we quickly realize a one-to-one ratio of requests that come into the gateway and requests our underlying service. If we want to make the ratio more efficient balance of requests to the gateway and requests to the service, we will need to introduce a cache. We can easily introduce a local in-memory cache on the API call from our federation to the service under it. When we add that to our architecture diagram, it looks like this:

![](/media/server-with-cache.jpg)

## Memoization

When we talk about caching, in most cases, we are talking about memorization. Per definitions.net, memoization is: "an optimization technique used primarily to speed up computer programs by having function calls avoid repeating the calculation of results for previously processed inputs." Memoization boils down to if your program will call the same function repeatedly with the same parameters and don't run the function but return the cached value instead. The parameters of the function create the cache key. When talking about caching an API call, as we did above, memoization allows for much cleaner code. If we don't memoize functions, we have to implement the caching searching functions everywhere, and that's not a fun time.

<<insert code snippet here of memoization>>

# Caching the cache!

An in-memory cache is great until you have multiple instances of a federated service running.

![](/media/multiple-federations.jpg)

The in-memory cache loses a lot of its efficiency when this happens, mainly due to just having so many caches that are not shared. But don't worry; there is a solution to this! A distributed cache helps solve this problem! If you create a distributed cache layer under the current caches, you get something that looks like this:

![](/media/multiple-federations-and-distributed-cache.jpg)

## But how do we do this!?

Well, if you look at the memoization function we created, it takes a cache and uses that to back its memoization logic. So if we make two memoization functions, one backed with an in-memory cache and one backed by our distributed cache, we can easily just apply them one after another. After they have been applied to a function, the logic on the server would first check the in-memory cache, then the distributed cache, and if both misses, make a call to the server and then cache the result in both in-memory and distributed caches.

# Awe-inspiring benefits to this approach

One of the best parts of this approach is how it allows your GraphQL ecosystem to cache things for each other. If we expand our picture a little bit to have more federations like so:

![](/media/2-different-federations.jpg)

If federations A and B are making calls to the same endpoint in the underlying service, they can share that cache value. This is HUGE! For a practical example, say you have a search federation and a product federation. Suppose a user searches for a product on your site. That search query ends up calling your media service to get the URLs of that product's pictures. Using the approach we have described, this would get cached in the distributed cache. When the user then clicks on the product, a product query is issued. That product query would go to the product federation; the product federation would have to get the URLs for the product's pictures from the media service. But with our caching, it would fetch the response from the media service from the cache, not the media service.

# But Our journey isn't done yet!

Now that we have some fantastic caching, we can make our system more efficient by introducing dataloader. Dataloader is a marvelous package used to batch requests together. We can weave this in right above our API layer but after our caching. Going back to the product and media example, if we request seven products and their images. If we have three cache hits and four misses, usually this would mean we make four calls to the media and products service. But dataloader comes to to the rescue here! When we insert it right before the API to bundle all four products up and make one call to the product service, we call the media service to get all the data. And the amazing this about all of this is the result of dataloader is cache INDIVIDUALLY! This is a massive deal because it makes our caching logic way less complicated. Due to everything being cached individually, the caching works for any sized group of items. Pictures make this easier to understand:

![](/media/adding-dl.jpg)

# We made it to the end

Now we have made it to the end. Taking a look back, we talked through an evolution of caching in GraphQL federations. We start with a simple cache, and as our problem became more complicated, so did our solution. We ended with an efficient caching strategy that allows for batching under caching but still caches individually.