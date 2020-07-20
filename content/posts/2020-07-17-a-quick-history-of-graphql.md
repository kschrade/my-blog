---
template: post
title: Why use GraphQL?
slug: why-use-graphql
draft: true
date: 2021-07-17T18:46:00.000Z
description: Why use GraphQL? I will recap the RFC I presented StockX
  engineering on GraphQL!
category: GraphQL
tags:
  - GraphQL
  - Tech
---
\[intro]

# What is GraphQL?

From the [GraphQL website](https://graphql.org/learn/):

> GraphQL is a query language for your API, and a server-side runtime for executing queries by using a type system you define for your data. GraphQL isn't tied to any specific database or storage engine and is instead backed by your existing code and data.

# Why use GraphQL?

In no order here are some of the reasons to use GraphQL:

## No more versioned APIs

In a RESTful world APIs have versions and as API evolve version numbers increase. This increase will never stop as long as services evolve. This leads to problems when trying to use the API due to no knowing what version is the latest. This also leads to multiple version of each resource, version 1 of the resource from V1, version 2 of the resource from V2, etc. GraphQL solves this problem by having 1 graph that has 1 version. This leads to 1 version of each resources that evolves with your ecosystem. 

## Smaller Payloads

In RESTful api you get everything, every time. In GraphQL you must define your query making the response smaller, or at least the same side. This is huge for clients, depending on how mature your restful api is this may be a huge reduction the data transmitted to your client device.  

## Easier and more defined interfaces

In an RESTful world you have your base urls, say http://www.restful.com/api/v1, and then you start adding your resources. When we add the resource, we will use posts for this example, we have http://www.restful.com/api/v1/posts. That url will return an array of posts. This works great until we start adding query params on this because we need a little more data. So if we want to include authors this can turn into api/v1/posts?include=author but then another use case pops up where we need to sort the posts. then that becomes api/v1/posts?sort=ASC and then we start adding other sorts and it slowly becomes a what can a client sent that works and what are all the query params? If you have front end and back end teams these query parameters become lost. GraphQL solves this by having everything defined in the schema and having a single endpoint. The schema defines everything around what extra data can you get (the I want this other data in my request). The other thing that comes out of the schema is all of the "query parameters" are defined and documented. To resolver to a field that has parameters, the schema has to define what is acceptable params. This is a huge benefit when there are different sorts. For example in posts what can you order by? In the restful world there's not a great way of defining a list, there are ways but none are great. In GraphQL you can create an enum that holds this list, which is huge. 

## Better client performance

Along with the smaller payloads mentioned before the round trips to the server are reduced as well. In a RESTful world you would request a resource, that resource will give you IDs to then go to another resource and get the information for those requests. This is also client to server requests which normally are not in close proximity. GraphQL allows for one request to get all the data you need, no need for follow up requests. The other benefit from GraphQL that is not mentioned as much is the performance increase from having 1 client to server request and multiple server to server request vs multiple client to server requests. This is huge when your company has clients on the other side of seas from your servers.

## Less Developer Time spent navigating APIs

With traditional APIs some type of swagger doc is used. This is great if you have 1 service that everyone keeps the documentation up to date with. Once you get into the micro service world this quickly becomes a problem.   With swagger docs scattered over multiple repos and hoping they are all up to date ramping up is a little hard. In GraphQL the schema has the ability to be documented. It looks like code comments and leads to being outwardly accessible. And again this self documenting schema coupled with 1 url makes onboarding developers much simpler than sending them multiple swagger docs. GraphQL also has some awesome tools that it comes with out of the box, like playground or GraphiQL depending on what flavor of GraphQL you are using. A easy way to see this is using a site like [graphql voyager](https://apis.guru/graphql-voyager/) that turn your schema into a visual graph. Pro tip if you click on one of the fields it has the description from the schema.

## Legacy App Support

When RESTful api is released into the wild it has to stay available for a long time. Depending on your companies stance on legacy apps that could be for as long as someone has that app. This means you can't remove the V1 end points from your code no matter how out of date they are. It's normally not a big deal until is a massive problem. It can force you to keep older version of packages, which might have security vulnerabilities or greatly increase the side of your build. The other large unexpected problem this causes is circumventing the new api. If V1 has different requirements than V2 it can wreak  havoc on the backend when a resource is added with two different set of required fields. In GraphQL there is one graph that is kept up to date. So if you swap out the data from the graph thats ok as long as the response looks the same. This allows for people to swap out backends and change the source of truth on the backend with no changes from front ends. And in addition to backends changing with not client changes, as long as the schema supports the query path the legacy app will always be supported. 

## Better Error Handling

One of the things that traditional APIs that can cause complications in front end code is the fact that they are binary on the errors they return. If any part of the code on a server fails that means the whole request fails. This leads to a lot of code bloat and confusing logic on front ends when in a micro service ecosystem fail. If you have 3 sequential REST calls that happen on a page, you have to write code to figure out what happens when call 1 fails, when call 1 passes and call 2 fails and when calls 1 and 2 pass but call 3 fails. In GraphQL it will resolve as much data as it can before returning. It returns a top level errors object with an array of errors but still returns as much data as it can allowing front ends to make the decision of how to handle the partial data. Think of a product page on amazon.com. If it was backed by a RESTful API how would the UI deal with partial failure, and how many partial failures would it have to deal with? If it was backed by GraphQL it could easily check the errors array to see if there is an error for the section it was about to display.