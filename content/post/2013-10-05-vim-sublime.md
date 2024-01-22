---
date: "2013-10-05T00:00:00Z"
published: true
title: How to make Vim like Sublime Text
slug: vim-sublime
---

disadvantages:<br />
<ul>
<li>synchronous</li>
<li>no graphics support</li>
<li>no HTML rendering support (like Javadoc in Eclipse)</li>
<li>vim "on-boarding" needs a lot of work:&nbsp;</li>
<ul>
<li>vim.org is ugly and disorganized</li>
<li>vim.wikia.com is ugly and spammy (yet helpful)</li>
<li>not at all obvious where to find a recent windows build</li>
</ul>
<li>needs a <a href="http://dribbble.com/shots/337065-MacVim-Icon-Updated">new icon</a></li>
</ul>
bright spots:<br />

<ul>
<li>http://vimeo.com/4456458</li>
<li><a href="http://vimcasts.org/">vimcasts.org</a></li>
<li><a href="http://usevim.org/">usevim.org</a></li>
<li>the brain damage is forwards- and backwards- compatible:&nbsp;</li>
<ul>
<li>besides vim being on every OS, vim key bindings are available in every major environment:&nbsp;</li>
<ul>
<li>gmail, calendar, finance (!)</li>
<li>github</li>
<li>man; tmux</li>
<li>emacs (evil mode)</li>
<li>sublime</li>
<li>eclipse (vrapper, now a very active project)</li>
<li>visual studio (vsvim)</li>
<li>even bash, chrome, and firefox, if you're into that</li>
<li>non-standard key bindings come with you, too: "find all references", "go to definition", "toggle breakpoint"</li>
</ul>
<li>snippets/templates</li>
</ul>
</ul>

I became familiar with Vim very gradually. By the time I found out about pico, I had long been editing X86Config with `i` and `:wq`. I was thankful for the syntax highlighting and remote availability writing C and Java on a Solaris ssh account. I embraced visual-block-mode during late nights at the CMOS lab after editing hspice input vectors manually became intolerable. I internalized [hjkl] by using Gmail shortcuts. And on OS X, actively-developed open source editors are few.<br />

So, Vim does not repulse me.

On the contrary, <a href="http://kien.github.com/ctrlp.vim/">ctrlp</a>&nbsp;is actually the first time I've been excited by Vim.<br />

unite.vim is a clever and thoughtful project. The unix approach of "everything is a file" has numerous advantages that have been extolled well enough; similarly, unite.vim harmonizes Vim's "everything is a buffer" approach with the idea of "sources"--basically, lists of things. In unite.vim, you can search for files, then query the set of unite.vim actions available on the selected files! That's awesome. You can query the set of vim commands, then query the output of the vim command. It removes the friction of :redir and registers. It replaces pulldown (GUI) menus, yankring, minibufexpl, :ls, ...<br />

<div style="background-color: #ffffcc; font-family: verdana, arial, helvetica, sans-serif; font-size: small; margin-bottom: 5px; margin-top: 5px; padding: 0px;">
After installing&nbsp;<a href="https://github.com/kshenoy/vim-signature" rel="nofollow" style="color: #336699; text-decoration: none;">vim-signature</a>, marks are actually awesome and fun to use. I use them when&nbsp;<code>ctrl-o</code>starts to get out of control.</div>
<div style="background-color: #ffffcc; font-family: verdana, arial, helvetica, sans-serif; font-size: small; margin-bottom: 5px; margin-top: 5px; padding: 0px;">
With&nbsp;<a href="https://github.com/airblade/vim-gitgutter" rel="nofollow" style="color: #336699; text-decoration: none;">vim-gitgutter</a>, I use&nbsp;<code>]h</code>&nbsp;all the time now (move to the next "hunk" of changes). (<a href="https://github.com/mhinz/vim-signify" rel="nofollow" style="color: #336699; text-decoration: none;">vim-signify</a>&nbsp;is a high-quality alternative that works with more than just git.)</div>
<div style="background-color: #ffffcc; font-family: verdana, arial, helvetica, sans-serif; font-size: small; margin-bottom: 5px; margin-top: 5px; padding: 0px;">
<code>]}</code>&nbsp;and&nbsp;<code>]m</code>&nbsp;are great; I wish there were a plugin that extended it for languages that don't use curly braces (def, end).</div>
<div style="background-color: #ffffcc; font-family: verdana, arial, helvetica, sans-serif; font-size: small; margin-bottom: 5px; margin-top: 5px; padding: 0px;">
<code>%</code>&nbsp;of course.</div>
<div style="background-color: #ffffcc; font-family: verdana, arial, helvetica, sans-serif; font-size: small; margin-bottom: 5px; margin-top: 5px; padding: 0px;">
As other mentioned,&nbsp;<code>zz zt zb</code>&nbsp;are wonderful and I abuse them all the time.</div>
<div style="background-color: #ffffcc; font-family: verdana, arial, helvetica, sans-serif; font-size: small; margin-bottom: 5px; margin-top: 5px; padding: 0px;">
<a href="https://github.com/Shougo/unite-outline" rel="nofollow" style="color: #336699; text-decoration: none;">unite-outline</a>&nbsp;is great if you use unite. (For ctrlp, use&nbsp;<code>:CtrlPBufTagAll</code>)</div>
<div style="background-color: #ffffcc; font-family: verdana, arial, helvetica, sans-serif; font-size: small; margin-bottom: 5px; margin-top: 5px; padding: 0px;">
I would love it if&nbsp;<a href="https://github.com/goldfeld/vim-seek" rel="nofollow" style="color: #336699; text-decoration: none;">vim-seek</a>&nbsp;had a way to seek vertically. I haven't thought carefully about whether this makes sense.</div>
<br />
Sublime Text has clear attention to detail. Emphasis on text-file configuration files is notable and signals that version control and portability is a priority. The multi-platform support is an achievement that is under-rated and remarkable in its consistency.<br />
<br />
But before buying it, I decided I would donate $50 to each Vim and Notepad++. And then I started to wonder if maybe the features I liked in ST2 actually existed in the editors I had been&nbsp;unconsciously&nbsp;using for all of my non-IDE editing.<br />
<br />
After pondering vim for a few months, I now realize that vim could be regarded as a starter kit for building your own custom text editor.<br />
<br />
Vintage mode is something that puzzles me. If you want Vim behavior, use Vim.<br />
<br />
When a system is made of things that are all the "same"--that is, of the same (derived) type, or having the same interface, or having some shared subset of predictable characteristics--something exciting happens: you can <a href="http://en.wikipedia.org/wiki/Composition_(number_theory)">combine the things</a> into really interesting things.<br />

- reddit "thing" table
- tmux
- i3wm
- unix file descriptor
- unix pipe
- powershell object
- monad
- vim buffer
- unite.vim source
- modes / modal editing
- composition of: commands; buffers; sources/lists;

You can't combine a vector with scalar, a milligram with a tesseract, or a Word document with a puppy.<br />

on the idea of <a href="http://www.reddit.com/r/vim/comments/1n3x4o/ide_features_i_havent_been_able_to_duplicate_in/ccfkm80">persistent REPL state</a>:<br />
<blockquote class="tr_bq">
one of smalltalk's language characteristics is the idea of images, something which&nbsp;<strong>has</strong>&nbsp;to be mandated within the language itself to exist.<br />
A smalltalk image essentially captures the program's state exactly, much like a virtual machine image in VMWare/Parallels/Virtual Box.<br />
This happens to turn every development session into a deliverable product, and more ambitiously every delivered product's error state can be captured exactly, making debugging end user problems be much easier than what having a coredump or error report of the situation available.</blockquote>

Stock vim sucks. Install <a href="https://github.com/tpope/vim-sensible">sensible.vim</a>. There, now you have an editor with sane defaults.<br />

If you have added preferences to your .vimrc, then you've written a plugin. That's it. There is nothing vain about adding plugins to vim: composition isn't much of an advantage if you want everything baked in. On the other hand, it doesn't make sense to <a href="http://cream.sourceforge.net/">Frankenstein</a> vim out of its scope.<br />

Go further: you can approximate some of the compelling features of <a href="http://research.swtch.com/acme">acme</a>:<br />

<ul>
<li>interact with shell commands using <span style="font-family: Courier New, Courier, monospace;">:r!cmd</span>&nbsp;</li>
<li>interact with editor functions and state using <span style="font-family: Courier New, Courier, monospace;">ctrl-r=</span></li>
</ul>
