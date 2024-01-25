---
date: "2013-06-27T00:00:00Z"
title: Java parser
slug: java-parser
---

I wrote a toy parser (or rather a *parser specification* in the syntax of a <a href="http://www.cs.princeton.edu/~appel/modern/java/CUP/manual.html">parser-generator</a>) for [COOL](https://theory.stanford.edu/~aiken/software/cool/cool.html), and I'm wondering if mature, popular languages actually use these things to generate their parsers.

The PMD project makes use of a (modified) [copy of the JavaCC parser-generator definition for the Java grammar](https://github.com/pmd/pmd/blob/bdc08e44fd92380d59cdc50cf9d8dd36e537c369/pmd-java/etc/grammar/Java.jjt). Inspection of that file reveals the initial 1997 timestamp by author Sriram Sankar. According to <i>Advances in Software Engineering</i>, “The Java 1.1 grammar was developed by Sriram Sankar at Sun Microsystems and a copy of this grammar can be found in the distribution”<sup>1</sup>. The syntax of [JavaCC](https://javacc.github.io/javacc/) at a glance is similar to yacc, CUP, bison. In the PMD `Java.jjt` file, the familiar token declarations start around <a href="https://github.com/pmd/pmd/blob/0344555956dc574e8d6ce09ddba7497d31ccc4dd/pmd/etc/grammar/Java.jjt#L234">line 234</a>, and the grammar starts at <a href="https://github.com/pmd/pmd/blob/0344555956dc574e8d6ce09ddba7497d31ccc4dd/pmd/etc/grammar/Java.jjt#L1122">line 1122</a>.

So it turns out that the parser for at least one popular, non-trivial grammar started (persists?) life in a .jjt file.

_Update, 9 Sept 2013:_ Josh Haberman <a href="http://blog.reverberate.org/2013/09/ll-and-lr-in-context-why-parsing-tools.html">on LL and LR parsers</a>:

> Despite this vast body of theoretical knowledge, few of the parsers that are in production systems today make use of any of this theory. Many opt instead for hand-written parsers that are not based on any formalism. [...] GCC moved away from their Bison-based parser to a handwritten recursive descent parser. The Ruby interpreter MRI may be one of the few remaining mainstream language implementations that does still use Bison (an LR-based tool) for parsing.&nbsp;</blockquote>
>
> [...] pure LL and LR parsers have proven to be largely inadequate for real-world use cases. Many grammars that you'd naturally write for real-world use cases are not LL or LR, as we will see. The two most popular LL and LR-based parsing tools (ANTLR and Bison, respectively) both extend the pure LL and LR algorithms in various ways, adding features such as operator precedence, syntactic/semantic predicates, optional backtracking, and generalized parsing.</blockquote>

_Update, 2023:_ These days we have [tree-sitter](https://tree-sitter.github.io/tree-sitter/) and [Lezer](https://lezer.codemirror.net/). Things are getting [interesting](https://neovim.io/doc/user/treesitter.html).


---

1. Hankan Erdogmus, Oryal Tanir. <i>Advances in Software Engineering.</i>&nbsp;p. 426. Springer, 2002
