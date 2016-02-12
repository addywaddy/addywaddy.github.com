---
layout: post
title: Om Next and Routing
date: 2016-02-12 11:33
--- 

I'm in the process of building a SPA in using om next for a prototype application and have been struggling with how to render the correct component, depending on the current route. How should my route component know which subcomponent to render?

I had a google around to see which approaches others were taking and stumbled upon [this repo](https://github.com/jdubie/om-next-router-example) (thanks for putting this out their, Jack!). Of the two approaches, the one I like the most was the `om/set-query!` one, but as Jack writes, it has it's issues.

We could though, using secretary or bidi, move the query setting part into our route definitions, setting the query on the `root`. Our root component implements the `IQuery` interface and so the key for the props returned by our query could be used to determine which component to use.


I've put up a simple application on github detailing this approach:

[Om Next with Routing](https://github.com/addywaddy/om-next-with-routing)

I'm not completely happy with it (especially the lookup table part), but it works :).

I'm hoping to have a look into mutations in the coming days, so I'll update the repo accordingly.

Feedback and comments welcome!


