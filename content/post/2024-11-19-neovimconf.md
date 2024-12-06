---
vim: textwidth=53 shiftwidth=2 nolist
author: Justin M. Keyes
date: "2024-11-19"
title: Neovimconf 2024
description: Keynote presentation for Neovimconf.live 2024
slug: neovimconf-2024
---


# State of Neovim 2024: The Giants Have No Timeouts

![](/img/neovimconf-2024/neovimconf-twitch-frontpage.png)

# What will you get out of this talk?

- backstory
- motivation
- current status
- plans

![](/img/neovimconf-2024/neovim-mascot-1.png)

# Ten years

> No commits for 4 days. Is Neovim dead?
> – reddit user (2015)


![](/img/neovimconf-2024/anchorman-escalated-quickly.png)

# Me

- Justin M. Keyes  https://sink.io/
- Nvim maintainer since 2014.
    - Roadmap https://neovim.io/roadmap/
    - Vision https://neovim.io/charter/
    - Docs
    - Release management
    - Decision fatigue
- Previous talks
    - 2016: https://youtu.be/9Yf3dJSYdEA
    - 2017: https://youtu.be/wQh7saOHE5g
    - 2019: Vimconf keynote https://youtu.be/Bt-vmPC_-Ho
    - 2021: Neovim roadmap https://youtu.be/4bTj0yBzrLU
    - 2022: State of Neovim https://youtu.be/IFACLbpqRB4
    - 2023: State of Neovim https://youtu.be/pmLn5pFu27k

![](/img/neovimconf-2024/koolaid.png)

# Outline

• [WHENCE]: WHERE DID WE COME FROM?

• WHY

• WHAT

• WHITHER

![](/img/neovimconf-2024/neovim-hq-1.png)

# Lore

- 2019: the case for legacy; Nvim vs Vim
    + "more Lua in core, more stdlib, more ergonomics"
    + features, UI ext, GUIs
- 2021: all-in Lua, no Vim9script
    + Deletion is synthesis. Don't accumulate indiscriminately.
- 2022: asteroid mining: channel the energy
    + Automation, tech debt
- 2023: space colony: scaling contribs, human coord
    + Architecture, interfaces
- 2024: external factors: warp drive for space colony
    + Move fast

# Lore: Neovim legacy, next steps

- ed: line-addressable editing language
- vi: normal-mode (AKA ex 2.0)
- vim: +textobjects, +eval (Vimscript)
- nvim:
  - `--embed` (hosted)
  - API (host)
  - nvim -l (composable)
  - org (humans)

# Lore: Vim way (2019)

Which of these will be relevant in 5-10 years?

- ✅ Modes
- ✅ Macros
- ✅ Common conventions (re-use concepts)
- ✅ Optimize ad-hoc: :nn instead of set_mapping()
- ✅ Leverage external tools
- ❌ DWIS not DWIM
- ❌ Keystroke-driven: gj instead of move_cursor()

# Outline

• WHENCE

• [WHY]: WHY ARE WE WORKING ON NVIM?

• WHAT

• WHITHER

![](/img/neovimconf-2024/neovim-hq-2.png)

# Why

- minimalism
- text UI
- control/extensibility
- optionality
- ecosystem (legacy/inertia)
- excellent people

...idk

# Why

> Beauty is our business.
> — Dijkstra

(leverage)

# Why indeed

High-demand products, derided as frivolous:

- video games => GPU
- internet => search
- ecommerce => logistics
- social media => datacenters, training data
- vim fork => :terminal, async
- nvim lua => 10x plugins
- bitcoin => ?

IOW: just do it.
- "Inverse vandalism."
- "Beauty is our business."

![](/img/neovimconf-2024/mtg-homunculus-scholar.png)

https://x.com/justinmk/status/1805593478371430402

# Why: optionality

- Don't need to guess the future, only prepare
- Hosted in vscode editor (or any non-modal editor)
- Hosted in firefox/chrome (firenvim)
- Extensible into a full app (lazy, nvchad, astro)

be a capybara
or tardigrade

https://x.com/fluffypony/status/1630954206298292232

![](/img/neovimconf-2024/capybara.png)

# Why: big things have low optionality

- vscode
- intellij

![](/img/neovimconf-2024/mtg-low-optionality.png)

# Why: optionality, Nvim chooses "power"

The expression problem: conflicting "design duality"
between *power* vs *properties*.

- Adding a property limits power.
- Constrain "system boundary".

https://www.tedinski.com/2018/02/27/the-expression-problem.html
https://blog.higher-order.com/blog/2014/12/21/maximally-powerful/

![](/img/neovimconf-2024/mtg-high-optionality.png)

# Why: optionality, Nvim chooses "power"

- Nvim plugins are maximally unconstrained (ad-hoc).
  - But we have an organism for lifting properties
    into the system boundary.
    - Examples: extmarks, lsp, shada
    - TODO: plugin namespaces, perf monitoring, packspec
- OTOH, Nvim itself is an "eldritch object" (not
  a "cuddly object"). But it's small enough to be
  a primitive (narrow waist).
- The system boundary of Nvim is:
  - architecture
  - invariants
  - documented patterns/practices

![](/img/neovimconf-2024/vacuum.png)






























# Why? compared to other projects

* vscode:
    + ✅ isolated plugins, UI protocol
    - ⚠️ TUI, modes(!), ad-hoc, macros
    - data science (telemetry) ~ agile
      consulting 2.0
* zed:
    + ✅ CRDT, "vim macros"
    - ⚠️ TUI, modes(!), ad-hoc (no Lua),
      20+ mins build-time
* ?
    + brain–computer interface (BCI)?

(irrelevant: AI)

![](/img/neovimconf-2024/contact-sales.png)


# Why

Why fork vim / why start from scratch (2019)

- Text editing is hard: multibyte, layout, cursor
  positioning, line-wrapping.
- Vim iceberg: shell handling, encoding, completion,
  Vim regex, quickfix ... Massive plugin archive.
- Nvim: Focus on usability and extensibility =>
  remove anti-features, dead-ends.

If "text editing" becomes a primitive (e.g. a high-quality
rust library, anaologous to "rg")... Nvim can consume it.
That exercise will tell us what remains.

# Why: harmony

- "Regular" syntax (nested, homoiconic, (foo(bar(…))))
  vs irregular free-form/"command"/"chat" syntax
  (DWIM, ad-hoc, throwaway)
    - vim/vi DSL = ad-hoc, interactive
        - Example: `:foo`
        - Example: `nvim --remote +'foo'`
        - Homoiconic macro DSL (even when keyboards are
          obsolete, need "roundtrippable",
          human-readable format for macros).
            - Used for repeat, mappings (saving to
              config), throwaway scripting, macros.
    - "regular" syntax for programming, not for
      interactive.
    - For interactive: most systems (except emacs)
      end up reinventing an interactive DSL.
        - Example: IRC/Discord `/foo` commands.

# Why: "start over" vs "harmony" (legacy leverage)

- "start over"
    - helix (kinda both: editor from scratch, but leverages lua/ts/cargo)
    - rust (hostile to C legacy)
    - Xi editor
    - Smalltalk
    - urbit (reinvents everything)
- "harmony"
    - zig (harmonious with C, fully replaces cmake)
    - lua (FFI harmonious with C)
    - nvim

# Effing the ineffable

- Who cares about extensible tools?
  (editor/vim/emacs)? (hackable)
    - A: non-extensible tools are like "shopping",
      not "developing". (build vs buy)
    - Why is it so hard to automate my OS?
    - vscode: create a mapping that automates a set
      of actions?
      ```
      { "foo" : [
        "cmd1": [ "arg1", "arg2" ],
        "cmd2": [ "arg1", "arg2" ],
      ]}
      ```
    - hackable: linux, raspi

# Effing the ineffable

- Who cares about minimalism (essentialism)?
  - x86 variable-length ISA greatly complicates its pipeline, vs ARM: https://youtu.be/Zr09I5OlOjs
      - ARM's open licensing helps to avoid "innovators dilemma".
  - format => presentation, not vice-versa!
  - why was Knuth preoccupied with typesetting?
  - why was Apple preoccupied with typography?
  - vscode/vs/eclipse represent the best that $1B can offer.
    Minimalist = "bloat skeptics". Want to avoid depending on
    a incomprehensible blob.
  - every node goes dark over time.
    After 4 years, this page has expired ssl cert
    and 10+ broken links: https://blog.darknedgy.net/technology/2020/05/02/0/index.html
  - net will denormalize to avoid censorship/control => your life's work needs
    to fit in a git repo, stored in "forever formats".

# Who cares about minimalism (essentialism)?

- Joe Armstrong: more software = more entropy
    - "The Mess We're In" https://www.youtube.com/watch?v=lKXe3HUG2l4
- Rich Hickey: simple is not easy
    - https://www.infoq.com/presentations/Simple-Made-Easy/
- Gary Bernhardt: Destroy All Software
- Alan Kay: "Computers are beautiful. But we have
  a know-nothing culture trying to use them."
- Jonathan Blow: revisit software foundations/history
    - https://www.youtube.com/watch?v=pW-SOdj4Kkk

Vscode is a loooooooooooot of software, and rapidly growing.

- Minimalism = Usability "via negativa"
- Perlis-Thompson Principle: Software with fewer concepts composes, scales, and evolves more easily.
  https://www.oilshell.org/cross-ref.html?tag=perlis-thompson#perlis-thompson

# Effing the ineffable

- Why text UI (TUI)?
  - "remote desktop" UI to any machine
  - kitty/ghostty are raising the bar of the
    "lowest common denominator".
  - macros (predictability, accessibility)
  - no DRM, no ads. it's the speakeasy of the
    information age. escape-hatch from the browser hegemony.
  - https://www.terminal.shop/

# Why: what motivates me?

- 2019 central questions:
  1. does vim need a fork/lua/terminal/jobs/treesitter?
  2. does the world need more vim?
- 2024 central thesis: we have...
  1. excellent people
  2. about to unlock huge potential. want to see
     where it leads.

# Outline

• WHENCE

• WHY

• [WHAT]: STATUS OF THE PROJECT

• WHITHER

![](/img/neovimconf-2024/neovim-hq-3.png)

# Status

- Build from source in <5 min (`make distclean && make` => 57s!)
- GitHub downloads: 5.8M (2023: 3.2M, 2019: 310k)
- Homebrew: nvim=373k vim=296k (2023: nvim=220k vim=238k)
- Stackoverflow survey: "most loved" (4 years!)
- /r/neovim 107K (2023: 69k, 2019: 11k)
- /r/vim 178K (2023: 163k, 2019: 90k)
- Vibe: ...a lot of users
- Contributors: 1386 (200 from Vim) (2023: 1160)

https://formulae.brew.sh/
https://survey.stackoverflow.co/2024/technology#2-integrated-development-environment

# Status

- https://dotfyle.com/neovim/plugins/trending 
- mindshare/marketshare:
  - neopopes
    - echasnovski
    - folke
    - mfussenegger
    - nvchad

# Status: Funding

- https://github.com/sponsors/neovim
- Where do donations go?
    - 100% WORK.
    - Full-time dev 1+ month
    - Zero overhead
    - 13+ developers since 2014
- 2024 work:
    - unicode handling, redraw, msgpack, arena allocator
    - options subsystem
    - UI extensibility (messages grid)

# Org: Maintenance

> Maintenance lacks the glamour of innovation. It is
> mostly noticed in its absence.
> https://www.economist.com/finance-and-economics/2018/10/20/repair-is-as-important-as-innovation

> "Tradition is not the worship of ashes, but the
> transmission of fire." — Gustav Mahler
 
maintenance
latent knowledge
theory building
legacy asteroid
leverage

# Status: Nvim STABLE (0.10)

- 0.9
  - 'exrc', "trust database"
  - 'statuscolumn'
  - `$NVIM_APPNAME`
  - Lua script runner: `nvim -l`
- 0.10
  - Concept consistency win:
    - nvim_open_win
      ✓ supports all window kinds (float and split)
      ✓ floating windows work just like regular windows
  - Type annotations for vimscript, api
  - Default colorscheme
  - LSP inlay hints
  - TermRequest, TermResponse
  - treesitter: builtin parsers for bash, markdown, python
  - comment plugin
  - vim.snippet, vim.iter, vim.system
  - vim.lpeg, vim.re, vim.glob, vim.base64
  - url hyperlinks (OSC 8)
  - `:[range]lua`
    ```lua
    vim.api.nvim_open_win(0, false,
      {relative='win', border='double', row=3, col=3, width=60, height=10})
    ```

# Status: Nvim HEAD (0.11)

- `vim._with()` (lewis + dundar + echa!)
    - Unblock via internal/private functions.
- UX: right-click "Go to def", "Open in browser"
- improved unicode/emoji/casefolding, utf8proc, drop src/unicode/ (40k loc) #29628 #30014 #30042
- every successful app eventually gains a markdown parser... why?
    - answer: just like ASICs (sha256, h264), the cooling phase of simulated annealing forms solved problems into hardware instead of software.
        - xterm is "hardware", markdown is "hardware", ...
        - Alan Kay: "Nothing less soft than a legacy
          system; nothing softer than a buggy system"
            - soft = ad-hoc
            - hard = rust
        - "Ontogeny Recapitulates Phylogeny" (Tanenbaum)
          = "embryos go through all of stages of evolution"

# Status: remote TUI

- `nvim --remote-ui`

![](/img/neovimconf-2024/neo-hacker-remote-tui.png)

# Status: "daemon mode" `[count]<c-z>`

- "daemon mode" `[count]<c-z>`

![](/img/neovimconf-2024/neo-hacker-1ctrlz.png)

# Status: progress in 2024

- Lspconfig nearing maturity.
  - ✅ cleanup, migrate to checkhealth, automate/generate more.
  - WIP: lift lspconfig into core (@gpanders)
  - WIP: `vim.lsp.config`, configs on 'runtimepath'.
- Lsp autocompletion
- Lsp multi-client (same buffer)
- Performance
  - rpc/arena allocator
  - redraw
  - treesitter
  - lsp semantic tokens #31117
  - vim.validate
  - vim.str_byteindex, vim.str_utfindex
- Improved paste handling for redo/macros.
- Defaults: LSP, snippets, vim-unimpaired
- :TOhtml Lua+ts rewrite

# Status: progress in 2024

- WIP: Options refactor+extend (@famiu, examples: #31111 #30993)
  - Enables us to fully leverage options instead of
    module-specific inner platforms.
    - Example: default floating window style: #20202
- Builtin UI declares nvim_set_client_info() on its channel.
- WIP: in-process LSP #24338
- WIP: remote TUI "detach"/reattach
- EXPERIMENTAL: treesitter WASM

# Status: writing things down

> Humans are exposed to neurobiological failures
> (attention engineering, rage farming). But our
> superpower is the ability to coordinate with
> narratives.  — David Shapiro

Alan Kay:
- systems aren't stories (no start/finish, just
  processes), thus hard for human brain to grasp.
  - Human inventions (systems) accelerating >> faster
    than human genetics/collective wisdom (stories).
  - cf. Tony Hoare "Debugging harder than programming"
  - cf. Einstein: "We cannot solve our problems with
    the same levels of thinking we used to create them".

https://www.youtube.com/watch?v=iyMk7Aa76qM&t=788s

# Status: writing things down

> "Understanding variation is the beginning of
> knowledge."
> * routine variation is present in every natural process
> * setting a target does not in itself help achieve that target
>
> – W. Edwards Deming / Statistical Process Control

# Status: writing things down (2024)

- treesitter distribution @clason #22313
  - upstream coord: versioning/conventions
  - capture groups
  - queries
  - dist: WASM
- lsp.buf "continutation" proposal #31206
- vscode-remote ssh
- Lua guidelines (also useful for helix/mle/vis/roblox)
- Event interface
- RPC callbacks #8658
- Lua structured concurrency, promise/future
- simplify remote plugins, massively #27949
- "culling of vim.lsp.util": #25272 @gpanders narrative
  => mature interface for `vim.str_utfindex()`
- "vim.ui enhancements": #25514 @gpanders narrative
  => fully-formed `nvim_open_win`, etc.
- lsp handlers/multiclient redesign #30982

# Status: Engineering, mf

"Programming as Theory Building", Peter Naur
https://pages.cs.wisc.edu/~remzi/Naur.pdf

The mind-map of a complex system can't be written down.

Code = compressed form of numerous interactions.
Extracting to non-code is so verbose, *consuming* it
would take more time than grokking the code.

https://x.com/justinmk/status/1750335701059596659

# Status: Engineering, mf

- eliminate rbuffer.c (unnecessary indirection): @bfredl #29141 #29106
- eliminate libmsgpack, favor libmpack @bfredl #29358 #29467
    - eliminate msgpack_sbuffer #29241
    - use mpack in shada, update conversion logic, remove libmsgpack: #29540
    - positions shada for more "shared database" things.
- gen_vimdoc.lua, replace lua2dox (lewis)
- echa found that vim.deprecate was slow in hot loops.
  Narrowed to vim.validate. gpanders/echa/bfredl made them fast.
- build.zig => cross-compile magic
- treesitter wasm/webassembly

# Status: plugins 2024:

- render markdown in-editor: https://github.com/OXY2DEV/markview.nvim
- https://nui-components.grapp.dev/
- showkeys
- Things you can* do with a text UI (TUI): NUI

![](/img/neovimconf-2024/nvchad-ui-colorpicker.png)

# Lore: no third ecosystem

Ecosystems tend to be winner-takes-all (80% of users
will use the top few, the rest is "long tail")

> Windows Phone was actually an amazing platform for
> both users and developers, and shows a fundamental
> rule of technology: There Is No Third Ecosystem."
> - former Nokia employee
>   https://news.ycombinator.com/item?id=16370602

Vscode is the first ecosystem.
Vim has always been the second ecosystem.
But it's first for the "modal" subcategory.

# Lore: no third ecosystem

Hemant Mohapatra https://x.com/MohapatraHemant/status/1258361814481375234
> When distribution is proprietary, distribution wins
> (Comcast vs Netflix), when distribution is
> commoditized, best product wins (chrome vs IE),
> when product is commoditized, best service wins
> (Amazon vs others), when service is commoditized,
> best network wins.

- vscode marketplace is proprietary (no forks can use it)
- legacy/community has "network" effects
- vscode/zed/nvim are competing on "product" axis
- but vscode/zed are offering services (collab
  editing, settings sync, AI)

# Status: risks, lingering problems, where are we failing?

- Still no Windows "owner"
- Too many topics (horizontal instead of vertical)
- Macros are not seamless.
  - Potentially helped by multicursor internals.
- Concept creep:
  - extmarks ~ "buffer annotations" ~ decorations ~ hl_attrs.
    - cf. text properties (vim/emacs), overlays (emacs)

# Outline

• WHENCE

• WHY

• WHAT

• [WHITHER]: FUTURE

![](/img/neovimconf-2024/neovim-hq-4.png)

# Future: positioning/strategy

- --embed (hosted)
- API (host)
- nvim -l (composable)
- org (humans)

![](/img/neovimconf-2024/neovim-parallel-roles.png)

# Future: positioning/strategy

Doesn't this apply to all projects (vscode,
intellij)?

- yes but,
  - they are big => hard to pivot
  - commercial projects tend to be goal-oriented
    instead of system-oriented
  - employee turnover: low institutional
    memory/tradition, low idealism
  - design-by-committee ("leadership")

![](/img/neovimconf-2024/neovim-parallel-roles.png)

# Future: constraints

- Cursor (enhanced context), AI chat (feature, not product)
    - all editors will get this feature
- UI components (NUI)
- layout engine (instead: integrate primitives (e.g.
  image protocol), but don't reinvent web browsers).

# Future: 2025

- Press Enter is the primary UX evil.
  - vscode feels nicer because it fails (errors) less noisily.
    - try looking at Extension Host logs in vscode!
      ```
      Error: telemetry-v2: recorder used before initialization
          at CallbackTelemetryProcessor2.callback (/…/sourcegraph.cody-ai-1.38.1/dist/extension.node.js:55629:17)
      ```
  - @luukvbaal working on this. #27811
    > Excise the technical debt of
    > "need_wait_return/msg_scrolled global
    > juggling". Instead Lua code decides in one
    > place what to do with routed messages.

# Future: 2025

- It should "just work".
  - drag/drop files/images into the editor
  - paste images/urls into the editor
  - long lines / big buffers
  - 'autoread'
  - Lua profiling
  - Lua debugger
  - Lua REPL
  - shada/context
  - Note: this is application-level stuff, instead of system-level

# Future: 2025

- repeatable visual-first operations (kakoune/object-verb).
- multicursor
- debug/profile story for plugin development
- "online" plugin performance tracing
  - detect and report badly-behaving plugins (plugin namespaces?)
- primitives for AI assistant
  - gathering context to send to LLMs
  - displaying responses
  - using responses (copy-paste, completion)
  - chat
- "presentation mode":
  - markdown: render hyperlinks, images
  - keymap: `z<tab>` or `<backspace>`
- image API #30889
- stdlib
  - forming an "async" story #19624

# Future: packspec (pkg.json)

https://github.com/neovim/packspec

# Future: builtin plugin manager

minimal git-based plugin manager.
like "paq".

# Future: remote plugins v2

connect to more stuff

# Future: deprecation strategy

Problem:
- some users exhausted by deprecation frequency
- some core devs exhausted by api maturation phase
- some core devs exhausted by legacy maintenance (opposite!)

# Future: deprecation strategy

https://daniel.haxx.se/blog/2024/10/30/eighteen-years-of-abi-stability/
- "Application authors everywhere can always and
  without risk keep upgrading to the latest libcurl".
- "Examples, documentation, applications, can just
  always upgrade and continue. ... Possibly *the*
  single most important property of curl."

# Future: deprecation strategy

The cost of deprecations is:

1. HIGH: managing the migration lifecycle (for hard deprecations)
2. HIGH: downstream breakage (for hard deprecations)
3. HIGH: face-huggers: permanent cognitive/discoverability
   cost of documentation/api/autocompletion surface-area (for
   both users and maintainers).
   - Developers use old wrong thing as "precedent".
4. HIGH: bug fixes
5. HIGH: presence of old "dead" code amongst "live" code.
6. LOW:  size/performance cost + presence of the code "somewhere".

For "low-dependency/complexity" interfaces, we can totally
eliminate 1-5, while paying the minor cost of 6, by:

1. Refusing all maintenance / bug fixes
2. Hiding the implementation in a "radioactive waste" area.

# Future: deprecation strategy

`:help api-contract` has been very successful:

- Keep the old impl, but remove it from docs+autocomplete.
- That gets it mentally out of the way without breaking downstream.
- Works perfectly for "flat" (low-dependency/cyclic
  complexity) interfaces such as the API.
- Could work for flat lua functions/modules (treesitter, `vim.*`).

# Future: deprecation strategy

Upside:
1. Greatly reduces the urge to remove the *implementation*,
   so legacy programmatic consumers can worry less.
2. Eliminates face-huggers.

FAQ:

- "how is this different than vim's never-remove-anything?"
  - This applies to low-dependency/low-complexity
    things, not "subsystems" (e.g. cscope).
  - Vim doesn't remove docs/completion/etc.

# Future: deprecation strategy

DEPRECATION STAGES:

1. soft
2. radioactive waste <= we are adding this stage
3. hard (removal)

EXAMPLE USE-CASES:

- rpcnotify/rpcrequest. Should be shadow-banned but
  not removed, it's too disruptive to remove them.
- Vimscript `timer_start()` vs `uv.timer_start()`.
  `timer_start()` is needed for back-compat, but
  showing it in docs and completion is noise, because
  it is redundant with `uv.timer_start()`.

# Future: deprecation strategy

Example: if `vim.lsp.foo` gets deprecated,
- it is unsupported, won't get fixes/improvements
- it no longer shows in autocomplete
- removed from documentation
- removed from meta
- its impl is moved to `deprecated.lua`
# Future: 20XX

- multihead:
    - q: if you can share shada/buffers/layouts
      amongst peers, do you actually need multihead?
      a: optimize server discovery + 'autoread' + shada

# Future: actual macros

If a UI session can emit itself as code, system feels
alive, composable. Narrow waist, more leverage.

Various attempts:
* vim/emacs macros
* macOS Automator
* curl --libcurl
* AWS Console-to-Code https://infoq.com/news/2024/01/aws-console-code-preview/

https://x.com/justinmk/status/1752088378873356557

# Future: positioning/strategy

- Something interesting happened since 2014: while
  Nvim was focusing on better TUI-GUI parity (from
  both directions), the terminals kept improving.
    - Even Windows Terminal supports xterm protocol.
    - tui-modifyOtherKeys/CSIu fixed some old
      pain-points (<c-i> vs <tab>, "super"/cmd key, …)
    - performance, clipboard, truecolor, mouse hover, ….
- But Neovide still feels *astonishingly* "smooth and fast".
    - We must support GUIs ("parallel tracks").

# Bundling vs unbundling

- Do both (host + hosted) = leverage
- Apps tend to excel at their own stack:
    - emacs = elisp
    - nvim = c, lua, treesitter (:InspectTree, :EditQuery)
    - intellij = java, kotlin
    - vscode = js/ts, LSP
- ...so embedding-into those hosts is very powerful.
- Dropping :cscope caused worry, but
  https://github.com/dhananjaylatkar/cscope_maps.nvim
  2 years later is actively maintained, and has more
  features than the old :cscope.
    - Arranging topics for flexibility (outsourcing
      non-core concerns to plugins) allows faster
      iteration, higher quality. => "system boundary"
- cmake (shrink C core…) => zig build (shrink
  C core…) => rust?


# Harness/jacket

- Examples:
  - json, OCI containers
  - raspi
  - joint-stock company, limited liability
  - ISO 6346 (shipping containers!)

# Harness/jacket dimensions

iow: Neovim is a PDU (protocol data unit) that
harnesses:
- modal user interface
- programmatic interface (Lua/RPC)
- continuous integration (CI) system to support
  rapid evolution
- human system (docs + chat + lore) to learn from the
  outside world
- engineering to connect libraries
  (treesit/zig/utf8proc/libuv)
  ...and adapt to change

![](/img/neovimconf-2024/neovim-driving-forces.png)

# Harness/jacket: primitives

- :terminal is an elementary component (like buffer, pipe, pty). Not bloat.
- TUI features are primitives (image display)
- CLI composability is a primitive ("echo foo | nvim")

# Positioning: AI?

- LLMs = personal exocortex (search engine).
- like a personal nuclear-powered assistant
- SEO = good for humans, good for AIs.
    - If you don't explain/document things for humans
      the AI will also be weaker.

- 2024:
    - ":help vimscript-functions" lists return + parameter types.

![](/img/neovimconf-2024/brain-offering.png)

# Positioning: AI

> write Nvim lua function that accepts a Lua pattern
> and searches buffer text surrounding the current
> cursor position, delimited by the previous and next
> occurrence of the pattern "^#".

- how can we make it very easy/attractive for 3P AI plugins?
    - improved RPC story (for nodejs/etc)
    - wasm

# AI fixes this?

Things that don't exist:
1. "compile" UI interactions to code.
2. micro-language with single-file impl in java/.net/js/…. https://scrapscript.org only has a python impl.
3. builtin instrumentation instead of mocks/spies.
4. Can AI *delete* code?



# Conclusion

![](/img/neovimconf-2024/neovim-driving-forces.png)


# END

![](/img/neovimconf-2024/neovim-marketing.png)

---

{{< rawhtml >}}
<script type="text/javascript">
//<![CDATA[

"futuristic, yet harmonious with legacy" (aka: sexy with leverage)

> Inverse vandalism: making things because we can.
> - Alan Kay

# Status: new batch of engineering leaders

- zig mafia (narrow waist)
  - zig, nvim/zed/helix, cloudflare, tailscale
  - curl, ripgrep, imagemagick, ffmpeg

# generalization is hard...
- generalization is HARD bc it requires general knowledge/experience/cooking.
    - requires contemplation.
    - requires you to stfu and let 'em cook. stop, hammock time.
    - takes time, 20 different topics to solve while KTLO
- examples:
    - kitty keyboard protocol (leonerd + kovid)

# Thesis

Thesis: each editor/IDE project is a competition to
see who has the best model for:
- integrating stuff
    - notice and then absorb primitives (libraries)
- deleting stuff
    - error correction (deprecation)

Balance:
- Small groups can leverage fast error-correction.
- Massive corp can leverage a few well-chosen
  alignments (protocols).

- who cares if an interface is generalized?
  - generalization allows focusing on narrow waist, fixing subtle bugs
    - e.g.: 'close' event not emitting
  - generalization allows _deleting_ code, which reveals the real foundation that can be trusted.
    - lua closures are a generalization that deletes all the noise of a new language (side quest: Vim9script)
    - eliminates whole class of unknowns.

# Effing the ineffable

- Why text editor when mind-machine interface (BCI) is imminent?
  - notice that the "text editing" part of vim is essentially a solved
    problem. mappings, extensibility, tree walking, architecture, etc, are
    long-term topics.
  - if BCI turns programming into editing an AST instead of raw text, we
    get... the same thing lisp has offered for 40 years. And that part of Nvim
    becomes a presentation layer, the "raw text" mode will be a fallback (like xxd).

- vscode is becoming a (very big) primitive, deployed
  as the standard "web ide" solution.
  - but not embedded in other editors.

# Progress since 2019: Vim worse-is-better

which parts of vim did we fix since 2019?

Vim's missing 50%:
- Imperfect design =>
    - ⚠️ bad perf: macros, long lines
    - ✅ syntax => treesitter, lsp
- ✅ Vimscript is slow => Lua
- ✅ :vimgrep is slow => use "rg"
- ✅ Legacy arch: 600+ globals, high coupling, TUI assumption
- ⚠️ Inconsistent UX: cmdline (:), execute(), :source


jackets / containing structures

    graph TD
        MarketForces[Market Forces] --> NeoVimEcosystem[Neovim Project Knowledge/People/Docs/CI]
        NeoVimEcosystem --> NvimArchitecture[Nvim Technical Architecture]
        NvimArchitecture -->|Inherits from| VimLegacy[Vim Legacy]
        NvimArchitecture -->|Uses| Libuv[libuv]
        NvimArchitecture -->|Integrates| Libraries[Libraries]

        style MarketForces fill:#f9f,stroke:#333,stroke-width:2px
        style NeoVimEcosystem fill:#bbf,stroke:#333,stroke-width:2px
        style NvimArchitecture fill:#bfb,stroke:#333,stroke-width:2px
        style VimLegacy fill:#ffb,stroke:#333,stroke-width:2px
        style Libuv fill:#fbf,stroke:#333,stroke-width:2px
        style Libraries fill:#fff,stroke:#333,stroke-width:2px

parallel tracks...

        mindmap
          root(nvim)
            IDE/PDE
            modal editor hosted in vscode
            platform for applications lazy/nvchad/astro
            portable multitool embedded systems
            multihead/p2p remote attach/detach
            hackable mentally-tractable starting point
            org/institutional knowledge

//]]>
</script>
{{< /rawhtml >}}
