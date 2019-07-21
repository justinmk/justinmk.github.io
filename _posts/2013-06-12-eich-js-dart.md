---
layout: post
date: 2013-06-12
published: true
title: Brendan Eich comments on Dart, JavaScript
---

_I stashed this away long ago and can't find the source (HN? reddit? a blog?).
Funny how lossy the web can be..._

Brendan Eich:

> Performance gaps in JS engines get filled all the time. This is different in
> kind from potentially adding a second VM, requiring cross-VM-heap cycle GC,
> etc. See https://lists.webkit.org/pipermail/webkit-dev/2011-December/018811.html
>
> Dennis described some of C's flaws (which JS inherited) in typically humble
> style here: http://cm.bell-labs.com/cm/cs/who/dmr/chist.html
>
> JS has too many gaffes and WTFs, but at this point, for most developers, they
> are (mostly) avoidable paper-cuts. All but:
>
> * the lack of exclusively-lexical scope due to the global object, and
> * all the "totally '90s" implicit conversion junk, e.g. (0 == "") === true,
>   are fixed by "use strict" in ES5.
>
> To fix the implicit conversion badness will take some further evolution that
> keeps backward compatibility for == and !=, but adds opt-in ways of binding
> better operators. This is on the agenda for ES7, as noted (value
> objects/types, binary data, SIMD short-vector types). It should cover all
> types and (I think) be lexically scoped, so in the future you would "use
> explicit"; and be free of the junk.

Eich’s own words regarding `use strict`/`use explicit` _runtime_ mode:

> The "marked page" is hard to isolate from related pages loaded in windows it
> opens, iframes it embeds, etc. … 

> The bigger problem is that engine implementors *and* language users do not
> want runtime "modes". The costs multiply and refract through the semantics.
> You can end up with a big explosion of cases, extra bug habitat, user
> confusion, code that should port from old to new mode but doesn't for obscure
> reasons found only if test coverage is perfect, etc. Note how most of ES5
> strict mode is compile-time error checking.

Eich replies to other comment(s):

> The alternative of a single GC and VM for multiple languages designed from
> a "clean slate" is also risky and costly, compared to evolving JS VMs to
> become that single GC/VM system. We have to evolve JS VMs anyway to keep our
> browsers competitive, and we're adapting to selection pressure for better
> cross-compiled code performance already (not just asm.js -- GWT and others).
>
> On your reply to my genetic trope, proposing engineering: We do not get to
> re-engineer our mtDNA _in situ_ now that it has spread throughout 7B people.
> Making alife with new mtDNA would be interesting (and dangerous). Unless you
> make a virus that rewrites 7B humans' mtDNA, you have to deal with "backward
> compatibility".
>
> Backward compatibility is what binds us most. It is deeply engrained in the
> Web, and in the Internet (see Postel's Law and my corollary). If someone
> could get past it and spin up a new Web, they would soon enough face the same
> problems we face, perhaps with a bit better up-front design helping them get
> a bit farther toward one idea of "perfection".
>
> But the evolutionary system doesn't care about "perfect", it cares only about
> "better and backward-compatible enough to hop to".
>
> Given these constraints, I have chosen to work on "better" in reachable hops.
>
> Dart designers seemed to want a much bigger hop -- not feasible for competing
> browsers to make based on DartVM code -- and a "replacement" strategy, but
> they seem to be ending up (via Dart2JS) needing the smaller hops. That
> validates my thesis.
>
> Standards finalization *follows* implementation and adoption, it does not
> precede it, in the modern era
>
> Languages evolve or die. At the limit, evolution can remake a species or
> a language -- the bad old forms fade away through disuse. Linters and "use
> strict" and modules kill the crazy. So the claim that JS "can't be fixed" by
> evolution reflects a choice, not a certainty.
