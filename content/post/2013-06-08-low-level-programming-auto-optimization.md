---
date: "2013-06-08T00:00:00Z"
title: Low-level programming discourages automatic optimization
slug: low-level-programming-auto-optimization
---

[Frances Allen](https://news.ycombinator.com/item?id=5809336):

> C has destroyed our ability to advance the state of the art in automatic optimization, automatic parallelization, automatic mapping of a high-level language to the machine.

[Example](https://news.ycombinator.com/item?id=5811779) from HN user 'waps':

> one example \[vs Haskell] that C can never hope to optimize : deforestation. Before optimization : the programmer requests a list to be created, fill it by calling functions and then passes the completed datastructure list (or tree) along to another function, which executes commands according to what the list contains.
>
> After optimization there is no more list. Instead the function the list is passed to will call a generated function that generates exactly the needed elements of the tree just-in-time. Result: no list, no memory (aside from 1 element on the stack), no allocation, no clearing of memory afterwards.


https://news.ycombinator.com/item?id=5838326

> You're right in that emulating the NES "good-enough" for most games is no big feat, in terms of power, but you are wrong in saying that it's because the platform is "old". There are old systems that are much trickier to emulate simply because software takes advantage of every corner case in the hardware design, using cycle perfect timing to exploit unexpected behavior in the chipset. The C64 is the perfect example, still having a pretty vibrant demoscene, coming up with new tricks that break emulators every year.


Alex Gaynor perspective on dynamic tracing:

https://speakerdeck.com/alex/why-python-ruby-and-javascript-are-slow



Steve Yegge...



