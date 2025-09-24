---
vim: shiftwidth=2 nolist
author: Justin M. Keyes
date: "2025-09-23"
title: Should Neovim support transitive plugin dependencies?
description: Dependencies considered carnival
slug: neovim-plugin-deps
---

Neovim 0.12 ships with a *plugin manager* [`vim.pack`](https://github.com/neovim/neovim/pull/34009),
which automatically installs plugins. But plugins don't yet have a way to
declare *dependencies*. Some sort of package "spec" is needed for that. And
we're [considering it](https://github.com/neovim/neovim/issues/34778).

I was excited to solve the "deps" problem with a [simple spec](https://github.com/neovim/packspec/):
essentially, "a list of URLs which may each have its own list... of... URLs".

Great, we did it!

But wait. Vim and Neovim plugins have been cursed/blessed with an accidental
constraint for 20+ years: they must be *self-contained* (where "must" means
"incessantly encouraged").

"Self-contained" begets duplication of "libraries", and potentially missing
bugfixes or upgrades. So to encourage high-quality libraries, we want plugins to
have dependencies.

Yet also, "Self-contained" is a natural, *passive* (the best kind of)
back-pressure against unending, thoughtless accumulation. Small is beautiful,
and while nothing is forbidden or impossible in Neovim plugins, we should not
encourage a particular kind of `node_modules/` brain-damage to dominate the
marketplace if we can avoid it.

## Zero-day risk vs Supply-chain risk

The choice between "vendoring" vs "dependencies" is a choice of [*zero-day* risk vs *supply-chain* risk](https://x.com/justinmk/status/1965224415857377310).

[Dependency Hell](https://www.gingerbill.org/article/2025/09/08/package-managers-are-evil/)
is the patient, inevitable byproduct of all good package management systems.
The security argument in favor of Dependency Hell is that it allows all
participating applications to get security fixes as soon as the updated
library is pushed through the dependency manager. This avoids "zero day"
vulnerabilities.

On the other hand, supply-chain attacks ([npm 2025](https://news.ycombinator.com/item?id=45169657), [Jia Tan](https://news.ycombinator.com/item?id=39914981))
appear to be more common, easier to implement, and exponentially bigger in terms
of blast-radius.

So we have what Alan Greenspan might call *a conundrum*.

## Neovim _is_ the libraries

Neovim provides the standard library (we refer to it as the "stdlib"). When
concepts and toil become commonplace, mature, routine, and finally, tedious, we
[upstream them](https://github.com/neovim/neovim/issues/20893#issuecomment-1723453602) into the stdlib.

During the period of library/technique "incubation" in the ecosystem, yes, there
is duplication and toil, but it gets subtracted when it matures. And that toil
reminds plugin authors to pause before they decide to vendor in an
over-architected implementation of `string_join()`.

If your plugin is more than just a plugin, rather a big application, or even
an [inner platform](https://www.lazyvim.org/) itself, then it probably has many
dependencies, but they can be specified in a flat list in your application:

```
vim.pack.add{
  'https://example.com/dep1',
  'https://example.com/dep2',
  'https://example.com/dep3',
}
```

This flat list, btw, is `package-lock.json` with fewer steps.

So yes, your mega-application needs to list all of its (transitive) dependencies
in one flat list. I'm not seeing a problem here.

## I read the plugin source

This is not the old "js minification, and wasm, have broken the openness of the
web" argument. But it may sound similar.

I adore minimalism. It feels like art. If I see a plugin and it has 20+ `lua/*`
modules or (gasp) many nested subdirectories with even more code, I generally
will avoid it (exceptions: where the value is high, and ostensibly the
complexity is required by the domain, e.g. [render-markdown.nvim](https://github.com/MeanderingProgrammer/render-markdown.nvim)).

For these reasons:

- more code = more bugs
- lots of code is often a signal of "bad taste". Most plugins with 10k lines of
  code are making... questionable choices, which reduces my confidence in their
  stewardship.
- supply-chain attacks are more dangerous than ever (agentic AI tools)

But that's just me.

## Counterpoint: Supply-chain attacks reveal your own weak opsec

If you are worried about supply-chain attacks, you probably have your keys and
personal data in the same filesystem as your tools and code. Maybe you
shouldn't. Maybe you should always code in a virtual machine, or some other
isolation chamber.

(Ideally, OS vendors would stop fucking around, and allow you to mark any
directory as "require touch-id before reading this", without any way for
malicious tools to silently unset that flag. Ahem.)

## Counterpoint: "It can't get any worse, there's nothing we can do anyway"

The Realists would like to interject with an announcement: "C'est la vie, we
can't do anything, it's already out of control", for example:

- https://fredrikaverpil.github.io/neotest-golang/install/
- neorocks, luarocks, lazyvim
- Neovim plugins can themselves depend on NPM, pypi, etc: https://github.com/neoclide/coc.nvim

And "We'll just get bug reports". Or "LazyVim will just monkey-patch checkhealth".

## We have levers

It can always get worse. In particular, the proportional _rate_ of good vs bad
can change. We don't have `node_modules/` level of careless/hopeless transitive
deps yet. Yes, it's true that Neovim plugins can take on arbitrary deps,
including npm and pip.

But back-pressure matters. Because, while other parts of the system are
advancing at rate 1, backpressure can adjust excessive input by a factor of 0.5
or 0.9. So even though the Neovim ecosystem might have tons of cruft, we can
tune the probability distribution so that 30% of what you find is beautiful,
instead of only 5%.

`vim.deprecate()` is an example of back-pressure. It warns about deprecated
usages. Users can ignore it by defining `vim.deprecate` to be a no-op.

Likewise, you can ignore warnings from your compiler. But do you? You can choose not to
enable type-checking. Yet Python users choose to introduce Mypy into their
projects.

So yes, back-pressure works, even if it's optional. It terraforms the landscape,
it sends a signal that reaches enough receptors to decide "canon".

## Backpressure

We can introduce backpressure via `:checkhealth` and runtime perf checks.

* Active performance checks
  * Nvim can perform *active* periodic, governor-style checks at runtime to
    decide if a plugin sucks. If a plugin spawns to many slow processes or Lua
    tasks, Nvim can [track stats](https://github.com/neovim/neovim/issues/26861)
    and eventually reveal it to the user.
* Structural improvements
  * We are developing the [concept of a plugin *name*](https://github.com/neovim/neovim/issues/34704)
    (weird but true) so that Nvim knows which thing to *blame* when it detects
    problems. (Stacktraces are too low-level.)
* Passive checks
  * Nvim 0.11.4 documents plugin *hygiene* at `:help lua-plugin`, and we
    continue to refine it. The next step is to [start flagging sus
    plugins](https://github.com/neovim/neovim/pull/35854) in `:checkhealth`.
    Part of that hygiene story will be "the plugin's transitive deps should not
    be a [black hole](https://www.reddit.com/r/ProgrammerHumor/comments/6s0wov/heaviest_objects_in_the_universe/)".
* Capabilities-based security
  * [Integrating with webassembly](https://github.com/neovim/neovim/issues/23579)
    gives us a path towards limiting plugin permissions.

If you have other ideas [let us know](https://github.com/neovim/neovim/discussions).

## Conclusion

I don't have any real preference about whether `vim.pack` supports deps or not.
As a minimalist I want careful plugins and good taste, but as a Neovim lead
I want to unleash unforeseen epic wins. I think we can have both. And if we
arrange things properly, they will feed into each other.
