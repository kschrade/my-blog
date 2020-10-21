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



<< picture goes here of gateway federation and service >>

As you can see, every time a request comes into the gateway, it will always turn into a request to the underlying service.



# Caching, our first step

When we look at the picture above, we quickly realize a one-to-one ratio of requests that come into the gateway and requests our underlying service. If we want to make the ratio more efficient balance of requests to the gateway and requests to the service, we will need to introduce a cache. We can easily introduce a local in-memory cache on the API call from our federation to the service under it. When we add that to our architecture diagram, it looks like this:



<< picture goes here basic gateway federation with caching picture>>

## Memoization

When we talk about caching, in most cases, we are talking about memorization. Per definitions.net, memoization is: "an optimization technique used primarily to speed up computer programs by having function calls avoid repeating the calculation of results for previously processed inputs." Memoization boils down to if your program will call the same function repeatedly with the same parameters and don't run the function but return the cached value instead. The parameters of the function create the cache key. When talking about caching an API call, as we did above, memoization allows for much cleaner code. If we don't memoize functions, we have to implement the caching searching functions everywhere, and that's not a fun time.



<<insert code snippet here of memoization>>

# Caching the cache!

An in-memory cache is great until you have multiple instances of a federated service running.

<<picture here of one vertical stack to one to multiple pods with just in-memory caches>>



The in-memory cache loses a lot of its efficiency when this happens, mainly due to just having so many caches that are not shared. But don't worry; there is a solution to this! A distributed cache helps solve this problem! If you create a distributed cache layer under the current caches, you get something that looks like this:



<<expand picture from above to have distributed cache>>

## But how do we do this!?

Well, if you look at the memoization function we created, it takes a cache and uses that to back its memoization logic. So if we make two memoization functions, one backed with an in-memory cache and one backed by our distributed cache, we can easily just apply them one after another. After they have been applied to a function, the logic on the server would first check the in-memory cache, then the distributed cache, and if both misses, make a call to the server and then cache the result in both in-memory and distributed caches.



# Awe-inspiring benefits to this approach

One of the best parts of this approach is how it allows your GraphQL ecosystem to cache things for each other. If we expand our picture a little bit to have more federations like so:

<<picture here of multiple federations and a media service>>



If federations A and B are making calls to the same endpoint in the underlying service, they can share that cache value. This is HUGE! For a practical example, say you have a search federation and a product federation. Suppose a user searches for a product on your site. That search query ends up calling your media service to get the URLs of that product's pictures. Using the approach we have described, this would get cached in the distributed cache. When the user then clicks on the product, a product query is issued. That product query would go to the product federation; the product federation would have to get the URLs for the product's pictures from the media service. But with our caching, it would fetch the response from the media service from the cache, not the media service.