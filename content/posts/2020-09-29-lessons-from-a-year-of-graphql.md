---
template: post
title: Lessons from a year of GraphQL
slug: lessions-from-a-year-of-gql
draft: false
date: 2020-10-02T21:00:31.345Z
description: "Highlighting some of the ups and downs of using graphQL for a year. "
category: graphql
tags:
  - GraphQL
  - tech
  - reflection
  - Apollo Federation
  - Apollo GraphQL
---
After using GraphQL for a year at StockX, it’s time to reflect a little on the good and the bad. We have had mainly good things come out of using GraphQL. 

![Fancy number 1](https://media.giphy.com/media/YPc5e834zAN5lsUpZV/giphy.gif)


## Build your schema as you go, don’t try to do it all at once. 

When we started using GraphQL, we tried to create as much of the schema as possible. It worked great for a proof of concept, but immediately after getting a green light, we realized it was a bad idea. We added a bunch of fields to mirror the RESTful call. But this created the same problem we had in rest; we have a bunch of extra fields that no one uses. We are still a year later in the process of removing the unused fields. What we have done to combat this is creating Just In Time Schema (JITS). All we mean by that is we only expose what we need to as we need to. We use the term YAGNI (you ain’t going to need it)  a little too much to help push schema. Until someone is going to need it, we don’t expose it. 

## Documenting the Schema helps a lot 

One thing that has been a massive help for consumers of our graph is the ability to document the schema and make that easily consumable. Internally we had an enormous lack of documentation and way too much tribal knowledge of our API. Our schema’s ability to be a large part of our documentation, allowing people to stop guessing what you can do with our API, has been a breath of fresh air. We no longer have to have to track down what query params work and what ones don’t. 

![Note taking gif](https://media.giphy.com/media/uDZexRVCffGww/giphy.gif)

## The performance improvement is real 

In our case, we saw a massive, massive increase in performance. We were talking in SECONDS of improvement, yes you read that right seconds! A few things GraphQL has empowered us to do is improve our caching, introduce data loader, and reduce payloads. All of this is just fueled by using GraphQL, no GraphQLl, no improvements.  

![SPPPPEEEEEEEEEEEEDD](https://media.giphy.com/media/dyRT9h6bzA4fDZvPgy/giphy.gif)

## Learning curve

Most people know how an API works and how to work with RESTful services. They have become very standard. The change to GraphQL is a hockey stick. Unless you have integrated with GraphQL or used GraphQL before, there is a learning curve. It’s very different from your standard API. You are no longer calling an API and getting everything the API will give you. You now have to know what you want as a client and how to request that. But once your mindset changes from the RESTful world to the GraphQL world, you can move faster. 

## Faster development cycles 

After switching over to GraphQL, we have seen our tickets complete faster. Instead of team A waiting for Team B to do something, then team C to expose the data for team A; in the graph, there have been times where team A can look around and use a different part of the graph for their ticket and have no other teams help them. Using any path in the graph has allowed the graph to solve more problems than we initially thought. 

## Don’t recreate the wheel. 

We use Apollo Federations, and we tried to recreate the wheel. Instead of looking at other distributed systems, we thought we would make a “pure” server and bolt things onto this “pure” schema/server. This decision seemed great until our server went under production load. We quickly realized that this idea of a “pure” server was not going to scale. We quickly pivoted to an architecture that looked a lot like RESTful services, but we had parts of our graph, not an actual service. GraphQL still has all the same problems as other tech, take inspiration from that and use the lessons learned from your other services to apply to your graph. 

## GraphQL federations scale well horizontally

After putting our federations under load, we found that scaling horizontally will elevate problems at scale. We use node, and with node being single-threaded, it allows for more requests to be handled simultaneously. We already pre scale federations for things like push notifications. Federations enable us to scale only sections of the graph we need without burning money, scaling everything. 

![](https://media.giphy.com/media/l378y5IDnT6alGGU8/giphy.gif)

## Read, then keep reading. 

We have had some stumbles while adopting GraphQL. The team at StockX has had no prior experience with GraphQL. So we read, and then read, then read even more. The amount of knowledge out there has helped us make more informed decisions than us, making decisions and going for things. 

![](https://media.giphy.com/media/NFA61GS9qKZ68/giphy.gif)

## GraphQL forces domain conversations 

When we started GraphQL, we had a mess of types and returns. Our endpoints returned the same payload but made front ends work; they can return one of seven entities. In the graph, with a clean start, we had an excellent opportunity to improve our types. The change to GraphQL spurred extensive conversations around what belongs in what type, should we continue with these large types that are possibly too flexible, and many other discussions. The change has ultimately led to us having more dedicated types that we believe work better for us. 

![](https://media.giphy.com/media/B0RuiNwNKYP1qYqyhu/giphy.gif)

After all the work in the last year, I still think the change to GraphQL was a major success and incredibly impactful. If we could go back a year and answer the same question of should we do this again? I would say yes, again. It has enabled us to so many great things, and there are only more to come!