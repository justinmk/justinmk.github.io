---
date: "2013-12-14T00:00:00Z"
title: "The Philosopher's Pile of Stones"
slug: philosopher-stones
---

The generalization of this is "communication", but trying to focus on "improving communication" leaves important stones unturned. Like "end poverty", it is a mission that begins without a fully-formed thought. There is no supply of communication that can be buttressed; rather, to improve communication, instances of the abstract must be identified.

Shannon crystallized this as the problem of _encoding_ information. We know from practice of information theory that information encoding benefits from semantic awareness:

The stones that often get missed are those not easily identified by leads or managers as communication nodes. The stones are tucked away in philosophy. Much has been written in agitation about the

Immutability of data is an attempt to tighten a critical feedback loop that otherwise is addressed by hand-wringing about global state. These days most junior programmers fervently avoid global state and feel good about that. But the problem manifests nevertheless in mutable object state, and collections of mutable objects, and session state.

Continuous integration is another attempt to improve feedback.

Unit-testing

A REPL reduces feedback latency.

Comment habit is a lonely stone. If a comment is concise yet precise, it adds value. If it is rambling, poorly-worded/punctuated, or vague, it adds noise and reduces the information density. It encumbers the feedback loop. Wit begets brevity. Understand your audience. The present context of your mental state will not be communicated in an incomplete comment. The ability to convey context and value with precision is an _encoding_. If your comment has an ellipsis or "lol", don't bother.

Source-control logs (_slog_) are a feedback dampener.

The application of these and other latency-reducing mechanisms/measures is subject to good taste, as ever. Zealous, context-free drilling into the unit-test stone, without regard to the health of the adjacent nodes, may delay progress and the net result is a failed project. Time is not money, it is _an actual factor of production._ Saying that "time is money" is a clumsy reminder for people who aren't cognizant of resource allocation. You understand that electricity and servers are necessary finite components of software production; then you also understand that time is a finite resource that must be divided amongst programming tasks. Therefore, software correctness is not an end in itself. Feedback signal is not an end in itself; it is a means. Insofar as feedback latency reduction harms the product, it must be trimmed.

The takeaway, however, is that common practice, and inertia, errs on the side of feedback noise. There is a tangible, detectable business case for squeezing the feedback loop in many targetable concerns that usually go unattended _precisely because_ the feedback loop is so stretched that the result is not connected to the cause. If a ticket to fix a date validation bug occurs in the Time Entry screen, and two weeks later a similar bug is received on the Billing screen; and each fixed independently; and when the framework is upgraded, a similar bug occurs again on the Time Entry screen; and these bugs were escalated by different support personnel; and the developer fixes each with an ad-hoc fix that breaks the previously-fixed behavior: you have now wasted X hours of salary time; XX hours of customer time waiting for the fix (and N units of customer happiness), and you haven't even fixed the original bug, and it is impossible for anyone except the developer to recognize the pattern.
