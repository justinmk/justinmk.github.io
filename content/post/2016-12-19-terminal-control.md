---
date: 2016-12-19
lastmod: 2024-01-23
author: Justin M. Keyes
title: Terminal Control
description: 'Practical introduction to terminal programming + 2-minute tutorial'
slug: terminal-control
---

Unix "TTY" terminal programming is a hodgepodge of tribal lore. There's no "Best
Practices" guide nor canonical "quick start" tutorial. Even Bash (the shell) has
[wooledge](https://mywiki.wooledge.org/BashGuide) and `set -x`. But terminal
programming is a tenuous détente of undocumented conventions.

This post is a low-friction, self-contained tutorial for tinkering with your TTY
and quickly seeing results. You can skip to the [Two-minute tutorial](#two-minute-tutorial)
to start trying things immediately.

Tooling
-------

The tools for working with terminals are essentially:

- `printf` (or `echo`, but `printf` works better)
- `script`
- `expect`
- `tmux`/`screen`

References
----------

The resources for learning about terminal details are:

- manpages: `man 7 pty`
- XTerm [ctlseqs reference]
    - ...and terminal-specific references such as [kitty's protocol extensions](https://sw.kovidgoyal.net/kitty/protocol-extensions/)
- source code of xterm / [ncurses](https://github.com/mirror/ncurses) / [libtickit](https://github.com/leonerd/libtickit) / etc.
- Paige Ruten's excellent [Raw input and output](http://viewsourcecode.org/snaptoken/kilo/03.rawInputAndOutput.html) walkthrough.
- Aivosto Oy's excellent [Control characters](http://www.aivosto.com/articles/control-characters.html) guide.

Concepts
--------

A "terminal emulator" sends user input and responds to terminal applications
(such as `bash` or `vim`). See [TTY demystified] and [Introduction to Termios]
for details.

Control sequences can be used to:

1. Control the terminal
2. Request info from the terminal
3. Set terminal modes/options

Terminal emulators implement one or more historical terminal models. For
example, XTerm emulates VT102 and [VT220](https://en.wikipedia.org/wiki/VT220).
The situation is not unlike HTML, where different web browsers may implement
different HTML features or DOM functions.

Applications and terminals communicate via stdin and stdout—the _same_ channels
that receive _user_ input and display _user_ output. That "conflict of interest"
is the reason your terminal gets messed up if you `cat` a binary file, or if you
paste binary data into the terminal. The implications may be surprising:

- You can control your terminal by the mere act of _causing output_ (i.e.,
  writing to stdout).
- The terminal responds by _causing input_ (i.e., writing to stdin).
    - It's up to the application to interpret the response.
- Applications use those mechanisms to work with the terminal. There's no
  lower-level API. (There are high-level libraries, such as ncurses, libtickit,
  [golang/term](https://github.com/golang/term), etc.)

The OS has various concepts/objects which I call "OS primitives": pipes, regular files, etc.
One of those OS primitives is (pseudo) "terminal". It's like a pipe with extra
behaviors (move the cursor, change current text color, etc.) controlled
_in-band_ with the "data" stream via special ESC sequences
(hence why terminals get messed up when you `cat` a binary file.)

File operations (`open()`, `read()`, `write()`, …) are [handled by](https://www.quora.com/In-Linux-UNIX-type-systems-what-is-the-concept-of-terminal-devices/answer/Brian-Bi?share=1)
a kernel driver. If you `open()` a regular file on an ext2 partition, it's
handled by the ext2 driver. If you `open()` a named pipe, it's handled by the
pipe driver (`fs/pipe.c` in Linux). And if you open a terminal device node, the
terminal device driver handles the request.

### pseudoterminal (pty)

From `man pty`:

> The slave end of the pseudoterminal provides an interface that behaves
> exactly like a classical terminal.

pty driver has job control and other features, just like a tty driver, but it
does not directly have access to the keyboard and display.
- Key difference: tty displays output; pty (master end) merely holds data for reading.
- In a normal tty, keyboard input caused data to become available for reading
  on the terminal device handle (file descriptor); in contrast writing to a
  pty master only makes the data available on the slave.
- Writing to a normal (tty) terminal device handle prints to the screen;
  writing to a pty slave only makes the data available for reading on the
  master end. (Similar to a socket pair.)
- For example if you echo CTRL-C (`\x03`) to the slave end of a pty, the master
  end receives it verbatim. But if `\x03` is written to the master end, the pty
  _driver_ sends SIGINT to the foreground process-group of the session controlled
  by the slave end, and the character itself cannot be seen on the slave end.
- And just like a normal tty driver, if `"\n"` is written to the pty _slave_, the
  pty driver converts it to `"\r\n"` before the master sees it (because "line
  feed" is a control code that means "go to next line, column 1"); if
  you write `"\n"` to the pty _master_, it passes through verbatim (because
  this keyboard input has no special meaning, unlike CTRL-C).
- Lifecyle of xterm (or any other terminal-emulator): xterm opens the pty master,
  then launches a shell that initially has the slave end as its stdin,
  stdout, and stderr. xterm forwards your keyboard input directly to the master
  end; that data goes through the kernel pty driver, which removes job control
  characters and and acts on them, and translates newlines and so on; then the
  shell, or whatever process is running in the shell, gets what's left as
  input. When output is written into the slave end, the pty driver doesn't
  touch job control characters, but still translates newlines; xterm reads it,
  parses the terminal control characters (color codes, cursor movement), acts
  on them, and prints out the rest in the window.
- Notice that the pty _driver_ is responsible for some parts of the protocol,
  but the terminal-emulator does the rest. It's inconsistent and imperfect, but
  that's how it is.



Controlling the terminal
------------------------

### Display the "a" byte

The most familiar feature of a terminal is _display_. Bytes sent to stdout are
displayed if they aren't "special".

1. Open a terminal and type "a".
2. The terminal writes the "a" byte (`0x61`) to stdin.
3. The shell reads the input from stdin.
4. The shell echoes "a" to stdout.
5. The terminal displays it.

Sending printable characters is easy, because they're mapped to your keyboard.
To send _control sequences_ we can use a tool like `printf` (shell) or `write()` (C).

### Send a control sequence

This OSC _control sequence_ sets the terminal title to "foo":

    ESC ] 0 ; f o o BEL

Your keyboard doesn't have a button for that. (You could type "Escape, ], 0,
...", but it would be too slow: the meaning of "Escape" is time-sensitive.)
Instead you can use the `printf` command to output the bytes as a batch.

    printf '\033]0;foo\007'

Now your terminal title is "foo".

    printf '\033[1;31mwe did it.\033[0m' > /dev/pts/6

### Craft a control sequence

The `\033]0;foo\007` (`OSC 0 ; f o o BEL`) sequence was derived by inspecting the XTerm
[ctlseqs reference].  Here are some guidelines for reading the reference:

- The [Control Characters] section defines the code names:
    - OSC is `ESC ]`, CSI is `ESC [`, ...
    - The C0 (7-bit) controls are in the leftmost column. For example `ESC ]`
      is the 7-bit OSC control code, `0x9d` is the 8-bit code.
- 7-bit vs 8-bit mode:
    - `OSC` for example is `ESC ]` _or_ `0x9d` depending on "whether you requested 8-bit controls (S8C1T)".
    - **7-bit controls (S7C1T) is most common,** but 8-bit (S8C1T) is more "modern".
    - Terminals by default don't expect 7-bit controls unless you [enable S8C1T mode].
- `ESC` and other [single-byte codes] (ESC, BEL, CR) are ASCII characters. Find their octal or
  hexidecimal representation in `man ascii` or [online][ASCII table].
    - ESC is octal `\033`, which is how we write ESC in a string.

For example using the reference for [Operating System Commands](https://invisible-island.net/xterm/ctlseqs/ctlseqs.html#h3-Operating-System-Commands):

    OSC Ps ; Pt BEL

we can translate it to something we can write in a string:

- The [Control Characters] section tells us that OSC is `ESC ]` (`\033` followed by `]`).
- `Ps` can be 0, 1, 2, 3 (see the reference for their meanings).
- `;` is a literal ";".
- `Pt` can be a literal "?" depending on the kind of code.
- `BEL` is an single-code ASCII code, thus `0x7` (or octal `\07`).

Now let's send some control-sequences to the TTY.

Two-minute tutorial
-------------------

In the following exercises you will _control_ your terminal and _request info_
from it, using tools you _already have_ on your system. You can (mostly)
copy-and-paste the commands and see results immediately.

### Exercise 1: Move the cursor and write some (colored) text

Remember the notes about pseudoterminals (pty). Linux implements "Unix98" ("System V style") ptys,
this means opening `/dev/ptmx` returns a new pty master file descriptor
and the slave end appears as `/dev/pts/N`.

Open a terminal and run the `tty` command:

    tty
    /dev/pts/6

There it tells us that the pty slave is `/dev/pts/6`. Now in a _different_
terminal we can write to the pty slave, to control its pty master.

In the [ctlseqs reference] we find:

- `ESC [ 4 B` means "cursor down four times".
- `ESC [ 4 C` means "cursor forward four times".

Let's use that info to show "foo" in the middle of the tty:

    printf '\033[4B\033[4Cfoo' > /dev/pts/6

From the [ctlseqs reference]:

- `ESC [ 45m` means "set background color to magenta".

If you set a color and write more text, it will print after "foo" (the terminal
is stateful—it remembers the last cursor position).

    printf '\033[45m' > /dev/pts/6
    printf ' we did it' > /dev/pts/6

The terminal should look like this:

```
$



      foo we did it
```

### Exercise 2: Send a status request

Some terminals (xterm, Vim/Nvim `:terminal`) support _requests_ (DECRQSS). Open
the terminal and input this command:

    printf '\033P$qm\033\\' ; xxd

Hit Enter a few times until xxd shows the result, like this:

    ^[P1$rm^[\

    00000000: 1b50 3124 726d 1b5c 0a0a 0a0a 0a0a 0a0a  .P1$rm.\........
              ^ ^  ^ ^  ^ ^  ^ ^  ^
              | P  1 $  r m  | \  |
              ESC            ESC  Enter (linefeed)

The [ctlseqs reference] "Request Status String (DECRQSS)" section explains
what happened (DCS = `ESC P`):

    DCS $ q Pt ST
          Request Status String (DECRQSS).  The string following the "q"
          is one of the following:
            " q     -> DECSCA
            " p     -> DECSCL
            r       -> DECSTBM
            s       -> DECSLRM
            m       -> SGR
            SP q    -> DECSCUSR
          xterm responds with DCS 1 $ r Pt ST for valid requests,
          replacing the Pt with the corresponding CSI string, or DCS 0 $
          r Pt ST for invalid requests.

- We sent `\033P$qm\033\\` and the terminal responded with `^[P1$rm^[\` (`^[` means ESC).
- Our request sent ST = `m`, so the response also gives ST = `m`.
- The response gives `DCS 1` (not `DSC 0`), so our request was valid.

### Exercise 3: Set a terminal _mode_

Exercise for the reader: try to set S8C1T mode in your terminal, by using the
reference.

Conclusion
----------

In this post we learned that "terminal programming" involves sending special
codes to the same(!) pipe as regular output. Any tool such as `echo` or `printf`
(or the file-writing capabilities of any programming language) can control,
request, and configure the terminal.

The details about the code sequences are documented in the xterm ctlseqs
reference. Non-xterm terminals are mostly xterm-compatible, but also have their
own custom codes. Every terminal application must "bring its own library" of
terminal codes and do its best to emit the correct codes for the terminal it
happens to be running in. There is no single standard (but xterm is the _de
facto_ standard).

Further reading is listed in [References](#references).


[TTY demystified]: httsp://www.linusakesson.net/programming/tty/
[Introduction to Termios]: https://blog.nelhage.com/2009/12/a-brief-introduction-to-termios-termios3-and-stty/
[ctlseqs reference]: https://invisible-island.net/xterm/ctlseqs/ctlseqs.html
[single-byte codes]: https://invisible-island.net/xterm/ctlseqs/ctlseqs.html#h3-Single-character-functions
[ASCII table]: https://en.wikipedia.org/wiki/ASCII#Control_characters
[Control Characters]: https://invisible-island.net/xterm/ctlseqs/ctlseqs.html#h3-C1-lparen-8-Bit-rparen-Control-Characters
[enable S8C1T mode]: https://github.com/neovim/neovim/issues/176#issuecomment-148488679
