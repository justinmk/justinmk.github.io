---
date: 2018-05-22
lastmod: 2018-05-22
author: Justin M. Keyes
title: Terminal emulation is a primitive
---

Terminal emulation is primitive feature.  It is not a premium feature.
Terminal emulation is a _basic facility_ that should, and will, be available as
a scriptable object in every mature application.

Some people still insist that terminal emulation is a premium feature, but
they're confused (or distracted in a haze of lifetime "multitasking").

Terminal emulation seems big and bloaty because

1. terminal protocol is fragile and often breaks in alarming fashion (thus users
   assume it's big because of the difficultly that leaks through to them), and
2. terminals are miserable to configure (terminfo, termcap, termios, $TERM,
   $TERMINFO, .Xdefaults, readline, …) so, again, users assume that implementing
   a terminal is a sacred domain that must not be triffled with by mere
   applications.

Unix already did this: it's called PTY.
Libraries like libvterm plumb the next layer: TTY.

"Power as a primitive" has already been shown to be useful, nay, it's the
inevitable direction of the industry:

- virtual machines
- containers (docker, LXM)
- referential transparency
- database as a value

You can wear your hairshirt while the rest of the industry zig zags towards Alan
Kay's computer-as-a-value.

It's so _obvious_, that this post is more of a historical comment than
a foreshadowing.

However, there's still a latent echo of "this shouldn't exist" (I wonder what
these people think of VMs, Docker, Datomic, Erlang, …?).

When tarruda added `:terminal` to Nvim it wasn't "obvious".
Now that Vim added `:terminal` there's nary a peep.

libvterm is a _library_. Libraries exist for code reuse.  Integrating it into
Nvim was a matter of 1300 LOC. (in Vim it's 6000 LOC, not to mention the
entirety of libvterm was forked and pasted into the tree, but the point is you
_could_ do it in 1-2 thousand lines of code. We can't do anything about Vim's
aw-shucks methodology.)

So "terminal emulation" has already been implemented.  There are _libraries_
available to _provide_ terminal-emulation-as-a-service (TaaS).

So applications that support embedded terminals aren't bloated, sinful, extravagant.


This observation/prediction is not surprising, and mostly boring. When Yegge
predicted in 2008 that Firefox would co-opt Emacs or Emacs will embed Firefox,
or both, it seemed hyperbolic or maybe 20 years out.  But it was only ~8 years
out (VScode).

Browsers delivered isolation, coordination, security, and automation where OSes
failed.

Takeaways:

- Emacs should integrate libvterm
- Terminal emulation will be a standard feature in every mature, scriptable development tool.
