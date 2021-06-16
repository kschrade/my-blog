---
"0": I
"1": n
"2": t
"3": r
"4": o
"5": " "
"6": g
"7": o
"8": e
"9": s
"10": " "
"11": h
"12": e
"13": r
"14": e
template: post
title: How to Make Sure Your Graph is Not Successful
slug: how-to-make-sure-your-graph-is-not-successful
draft: true
date: 2021-04-26T17:00:00.000Z
description: We're going to walk through how to make sure a graph is not
  successful. If you want your graph to be successful, do the opposite of these
  points.
category: GraphQL, Schema
tags:
  - GraphQL
  - Schema
---

## Picking the Wrong Type of Graph
When looking to create a graph, the two main choices people pick between are federations and a monolithic server. 

Federated graphs are great for people when traffic is too much for a single server and/or you have too many teams trying to work on a single server. Federated graphs are great for helping handle the load of production traffic due to the ability to scale individual parts of the graph. To ensure you have a more challenging time, don't use federations if you have a large amount of traffic. A nonfederated graph will still work, but you will have more problems fighting with scaling.  Without the ability to scale parts of the graph separately, you will have to scale your entire graph as one big entity. This does work but leads to a lot of wasted resources. Now on the flip side, if you have a small amount of traffic and are doing a POC type service, make sure to make it overly complicated and do a federated graph. Federated graphs are great, but they can be overkill when doing something small or just proving GraphQL is an excellent technology to use. 


## Not Using the Graph as a Graph
When creating schema for a graph, we tend to try and make resolvers jump from one thing to another that makes sense. If you think of movies, you can resolve from query to movie to cast to movies and so on. The easiest way to make this graph not work well is by making your graph more restful than graphy. In the movie example, to make the graph hard to work with, don't allow anything past one level away from query. So now, instead of being able to make one query to get everything you need, you have to make multiple requests.

## Don't Use Typing as needed

## No descriptions 

## No Consistency