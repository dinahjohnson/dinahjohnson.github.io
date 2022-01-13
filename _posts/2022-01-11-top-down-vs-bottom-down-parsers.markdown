---
layout: post
title:  "Top-Down vs Bottom-Up Parsers"
date:   2022-01-11 14:17:15 -0600
categories: [computer science, programming languages, compilers]
---

Compilation is the process of taking high-level source code and converting it to machine language that the computer can understand. The compilation process is a sequence of multiple phases.  

![Compilation Process](/assets/images/compilation-process.png)

In the syntax analysis phase, a parser takes the lexemes (a lexical unit represented by a token) output from the lexical analysis phase to determine their syntax structure according to the given rules of the grammar. A parser ensures that the token arrangements are syntactically correct.  

There are 2 categories of parsers: top-down parsers and bottom-up parsers.  

## Top-down parsers

Top-down parsers build parse trees from the root downward to the leaves in preorder (each node is visited first and then its branches are explored). The most common top-down parsing algorithms are in the LL algorithm family. LL algorithms scan the input from left-to-right and specify the leftmost derivation generated.  

One example of a top-down parser is a recursive descent parser which is coded directly from the Backus-Naur description of the language syntax.

| Advantages | Disadvantages |
| ----------- | ----------- |
| Simplicity | Doesn’t allow left recursion      |
| Easy to Code   | Ambiguity (can result in more than 1 parse tree from the same string)        |

An example of top-down parsing given the following assignment statement:

word = 2 * (5 - 10)

![Top-down parse example](/assets/images/top-down-parse.png)

## Bottom-up parsers

Bottom-up parsers build the tree from the leaves upward to the root. A bottom-up parser takes a sentential form 𝛼 and determines its handle, which is a substring of 𝛼, to determine which rule must
be reduced to its left-hand side (LHS) to produce the previous sentential form.  

Commonly used bottom-up parsing algorithms are in the LR family which performs a left-to-right scan and specifies the rightmost derivation generated. One example is the shift-reduce algorithm.

| Advantages | Disadvantages |
| ----------- | ----------- |
| Can detect syntax errors quicky | Difficult to produce an LR parsing table by hand |
| Can be built for all programming languages   | Complex – difficult to customize |
| More powerful than top-down parsers | Difficult to identify the unique handle of a sentential form if more than one production rule reduces the handle to its preceding sentential form |

Here's an example of bottom-up parsing given the following grammar and right sentinel:

*Grammar*

S → AB \| BC \| cB  
A → Ac \| aBB \| b  
B → Cb \| cBb \| a  
C → Ac \| bCA \| c  

*String*  
aBbcbcBb

![Bottom-Up Parse Tree Example](/assets/images/bottom-up-parse.png)

Once parsing is complete the resultant tree is then checked by the semantic analyzer to confirm it follows the rules specified by the language. Both methodologies should correctly verify the syntax structure based on the grammar of the language. Despite their complexity, bottom-up parsers tend to be the pre-dominant choice because of their power and deterministic output.  

## Resources
[Parsers](https://en.wikipedia.org/wiki/Parsing)  
[Compiler Design](https://www.tutorialspoint.com/compiler_design/compiler_design_phases_of_compiler.htm)  
[Parsing: Top-Down vs Bottom-Up](https://jeffreykegler.github.io/Ocean-of-Awareness-blog/individual/2014/11/ll.html)
