---
template: post
title: Why use GraphQL?
slug: why-use-graphql
draft: false
date: 2020-07-20T20:07:26.017Z
description: Why use GraphQL? I will recap the RFC I presented StockX
  engineering on GraphQL!
category: GraphQL
tags:
  - GraphQL
  - Tech
---
<!--StartFragment-->

Almost last year ago, I wrote a 16 page RFC (request for comments) for StockX to adopt GraphQL at our edge layer. It was the first step in a long ride that we are still on at StockX. The rest of the blog post hits on some of the main points I put into the RFC.

# Before Why

## What is GraphQL?

Before we get into the meat of the RFC, lets level set a little. This is what the [GraphQL website](https://graphql.org/learn/) says GraphQL is:

> GraphQL is a query language for your API and a server-side runtime for executing queries by using a type system you define for your data. GraphQL isn't tied to any specific database or storage engine and is instead backed by your existing code and data.

## How are we trying to use GraphQL?

The proposed use of GraphQL is in more of an API Gateway/masking services/stitching layer. And what I mean by that is, it sits on the edge of our backend, as an API Gateway, and it handles all the stitching of data from microservices, monolith, and some of our 3rd party integrations.

# Why use GraphQL?

In no order, here are some of the reasons to use GraphQL:

## No more versioned APIs

In a RESTful world, APIs have versions, and as APIs evolve, API version numbers increase. The version increases will never stop as long as services keep growing. This leads to problems when trying to use the API because no one knows what version is the latest or returns you the correct data. This also leads to multiple versions of each resource, version 1 of the resource from V1, version 2 of the resource from V2, etc. GraphQL solves this problem by having one graph that has 1 version. This leads to 1 version of each resource that evolves with your ecosystem.

## Smaller Payloads

In RESTful API, you get everything, every time. In GraphQL, you must define your query, making the response smaller, or at least the same side. This is huge for clients, depending on how mature your restful API is, this may be a massive reduction in the data transmitted to your client device.

## More comfortable and defined interfaces

In a RESTful world, you have your base URLs, say `http://www.restful.com/api/v1`, and then you start adding your resources. When we add the resource, we will use posts for this example; we have `http://www.restful.com/api/v1/posts`. That URL will return an array of posts. This works great until we start adding query params onto this URL because we need a little more data. So if we want to include authors, this can turn into `api/v1/posts?include=author` , but then another use case pops up where we need to sort the posts. Then the URL becomes `api/v1/posts?include=author&sort=ASC`. As we start adding other sorts and other query params, the endpoint becomes much more unmanageable. The question we had internally was, "does this query param or path work like this?" or "What are the valid params I can send, and how do I send them?". If you have a front end and back end teams, these query parameters become undocumented and lost. GraphQL solves this by having everything defined in the schema and having a single endpoint. The schema defines everything around what extra data you can get (the "I want this other data in my request" problem). The other thing that comes out of the schema is all of the "query parameters" are defined and documented. The schema defines what the params are and is acceptable when resolving to a field with parameters. This is a huge benefit when there are different sorts and or multiple parameters. For example, in posts, what can you order by? In the RESTful world, there's not a great way of defining a list, there are ways, but none are significant. In GraphQL, you can create an enum that holds this list, which is huge.

## Better client performance

Along with the smaller payloads mentioned before, the round trips to the server are reduced as well. In a RESTful world, you would request a resource, that resource will give you IDs to then go to another resource and get the information for those requests. This is also the client to server requests, which generally are not nearby. GraphQL allows for one request to get all the data you need, no need for follow-up requests. The other benefit from GraphQL that is not mentioned as much is the performance increase from having one client to server request and multiple servers to server requests vs. multiple client to server requests. This is huge when your company has clients on the other side of the seas from your servers.

## Less Developer Time spent navigating APIs

With traditional APIs, some type of swagger doc are used. Swagger docs are great if you have one service that everyone keeps the documentation up to date with. Once you get into the microservice world, this quickly becomes a problem. With swagger docs scattered over multiple repos and hoping they are all up to date, ramping up is a little hard. In GraphQL, the schema can be documented. It looks like code comments and leads to being outwardly accessible. And again, this self-documenting schema coupled with 1 URL makes onboarding developers much more straightforward than sending them multiple swagger docs. GraphQL also has some excellent tools that it comes with, like playground or GraphiQL, depending on the flavor of GraphQL you are using. An easy way to see this is by using a site like [GraphQL voyager](https://apis.guru/graphql-voyager/) that turns your schema into a visual graph. Pro tip if you click on one of the fields, it has the description from the schema.

## Legacy App Support

When RESTful API is released into the wild, it has to stay available for a long time. Depending on your company's stance on legacy apps, that could be for as long as someone has that app. This means you can't remove the V1 endpoints from your code no matter how out of date the endpoint is. It's usually not a big deal until it is a massive problem. It can force you to keep older versions of packages, which might have security vulnerabilities or significantly increase the side of your build. The other sizeable unexpected problem this causes is circumventing the new API. If V1 has different requirements than V2, it can wreak havoc on the backend when a resource is added with two different sets of required fields. In GraphQL, there is one graph that is kept up to date. So if you swap out the data from the graph, that's ok as long as the response looks the same; this allows people to swap out backends and change the source of truth on the backend with no changes from the front ends. And in addition to backends changing with no client changes, as long as the schema supports the query path, the legacy app will always be supported.

## Better Error Handling

One of the things that traditional APIs can cause are complications in front end code due to the binary errors they return. If any part of the code on a server fails, that means the whole request fails. This leads to a lot of code bloat and complex logic on the front ends when a microservice ecosystem fails. If you have 3 subsequent REST calls that happen on a page, you have to write code to figure out what happens when call 1 fails, when calling one pass, and call two fails, and when calls 1 and 2 pass but call three fails. In GraphQL, it will resolve as much data as it can before returning. It returns a top-level error object with an array of errors but still returns as much data as possible, allowing front ends to decide how to handle the partial data. Think of a product page on amazon.com. If a RESTful API backed it, how would the UI deal with partial failure, and how many partial failures would it have to deal with? If it was supported by GraphQL, it could quickly check the error array to see if there is an error for the section it was about to display.

<!--EndFragment-->