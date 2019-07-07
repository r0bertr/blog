---
title: Important Points of Compilers
date: 2019-07-03 13:54:13
mathjax: true
tags:
    - English
---

> The principles and techniques for compiler design are applicable to so many other domains that they are likely to be reused many times in the career of a computer scientist. The study of compiler writing touches upon programming languages, machine architecture, language theory, algorithms, and software engineering.

# The General Structure of a Compiler

{% asset_img phases-of-a-compiler.png %}

# Context-Free Grammars

A context-free grammar $G$ is defined by the 4-tuple

$$
G = (V, \Sigma, R, S)
$$

where

1. $V$ is a set of _nonterminals_, sometimes called \"syntactic variables.\" Each nonterminal represents a set of strings of terminals, in a manner we shall describe.
2. $\Sigma$ is a set of _terminal_ symbols, sometimes referred to as \"tokens.\" The terminals are the elementary symbols of the language defined by the grammar.
3. $R$ is a set of _productions_, where each production consists of a nonterminal, called the head or left side of the production, an arrow, and a sequence of terminals and/or nonterminals, called the body or right side of the production. In fact, $R$ is a finite relation from $V$ to $(V \cup \Sigma)^*$.
4. $S$ is designation of one of the nonterminals as the start symbol.

All the terminal strings that can be derived from the start symbol form the **language** defined by the grammar.

# Important Concepts in Lexical Analysis

> A _token_ is a pair consisting of a token name and an optional attribute value. The token name is an abstract symbol representing a kind of lexical unit, e.g., a particular keyword, or a sequence of input characters denoting an identifier. The token names are the input symbols that the parser processes. In what follows, we shall generally write the name of a token in boldface. We will often refer to a token by its token name.

> A _pattern_ is a description of the form that the lexemes of a token may take. In the case of a keyword as a token, the pattern is just the sequence of characters that form the keyword. For identifiers and some other tokens, the pattern is a more complex structure that is matched by many strings.

> A _lexeme_ is a sequence of characters in the source program that matches the pattern for a token and is identified by the lexical analyzer as an instance of that token.

> An _alphabet_ is any finite set of symbols.

> A _string_ over an alphabet is a finite sequence of symbols drawn from that alphabet.

> A _language_ is any countable set of strings over some fixed alphabet.

> A _regular set_ is a language that can be defined by a regular expression.

# Regular Expressions

Regular expressions are defined by the following rules:

1. $\epsilon$ is a regular expression, and $L(\epsilon)=\\{\epsilon\\}$.
2. If $a \in \Sigma$, then $a$ is a regular expression, and $L(a) = \\{a\\}$.

Suppose $r$ and $s$ are regular expressions.

3. $r|s$ is a regular expression and $L(r|s)=L(r) \cup L(s)$.
4. $rs$ is a regular expression and $L(rs)=L(r)L(s)$.
5. $r^\*$ is a regular expression and $L(r^\*)=L(r)^\*$.
6. $(r)$ is a regular expression and $L((r))=L(r)$.

There are also plenty of extensions of regular expressions.

7. $r^+$ is a regular expression and $L(r^+)=L(r)^+$.
8. $r?$ is a regular expression and $L(r?)=L(r) \cup \\{\epsilon\\}$.
9. $[a_1a_2\cdots a_n]$ is a regular expression with $a_1,a_2,\cdots,a_n$ are all regular expressions and it is a shorthand of $a_1|a_2|\cdots|a_n$.

Regular expressions have following properties:

1. $r|s=s|r$.
2. $r|(s|t)=(r|s)|t$.
3. $r(st)$=$(rs)t$.
4. $r(s|t)=rs|rt$; $(s|t)r=sr|tr$.
5. $\epsilon r=r\epsilon=r$.
6. $r^\*=(r|\epsilon)^\*$.
7. $r^{\*\*}=r^\*$.

# Finite Automata

> Finite automata are recognizers: they simply say \"yes\" or \"no\" about each possible input string.

## Nondeterministic Finite Automata

A **nondeterministic finite automaton(NFA)** is defined by a 5-tuple, $(S, \Sigma, T, s_0, F)$, consisting of

1. A finite set of states $S$.
2. A set of input symbols $\Sigma$, the input alphabet. We assume that $\epsilon$ is never a member of $\Sigma$.
3. A transition function $T$ that gives, for each state, and for each symbol in $\Sigma \cup \\{\epsilon\\}$ a set of next states.
4. A state $s_0$ from $S$ that is distinguished as the start state or the initial state.
5. A set of states $F$, a subset of $S$, that is distinguished as the accepting states or the final states.

## Deterministic Finite Automata

A **deterministic finite automaton(DFA)** is a special case of an NFA where:

1. There are no moves on input $\epsilon$.
2. For each state $s$ and input symbol $a$, there is exactly one edge out of $s$ labeled $a$.

## Algorithm: From NFA to DFA

**INPUT** An NFA $N$.

**OUTPUT** A DFA $D$ accepting the same language as $N$.

**STEPS**

1. Compute $\epsilon\text{-}closure(s_0)$ as the initial unmarked state $s_0'$ of $D$.
2. Do the following until all the states in $D$ is marked:
    1. Pick up an unmarked state $s'$ in $D$, mark $s'$.
    2. For all symbols $a$ in $\Sigma$, do the following:
        1. Compute $\epsilon\text{-}closure(move(S_{s'}, a))$ as a state $s_i'$ in $D$, where $S_{s'}$ is the set of states in $N$ corresponding to the the state $s'$ in $D$.
        2. If $s_i'$ is not a existed state of $D$, add it as an unmarked new state.
        3. Create an edge $a$ between $s'$ and $s_i'$.

**EXPLANATION**

1. $\epsilon\text{-}closure(s)=\\{s_0\\} \cup \\{s | s\text{ can be reached from }s_0\text{ through an edge labeled }\epsilon\\}$.
2. $move(S,a)=\\{s|s\text{ can be reached from states in }S\text{ through an edge labeled }a\\}$.

# References

Alfred V. A., et al. _Compilers Principles, Techniques, & Tools Second Edition_

Context-Free Grammar - Wikipedia. https://en.wikipedia.org/wiki/Context-free_grammar
