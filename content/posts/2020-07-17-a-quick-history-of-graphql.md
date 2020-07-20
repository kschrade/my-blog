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



## Legacy App Support

## API Documentation is easier

## Better Error Handling