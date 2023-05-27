---
layout: page
title: Polyglot Programming
tagline:
---
{% include JB/setup %}

<span class="keyword">Polyglot Programming</span><sup><a href="#footnote-1">1</a></sup> explores the benefits, as well as drawbacks, of combining multiple programming languages and multiple *modularity paradigms* in application development. The "paradigms" include <span class="keyword">Functional Programming</span>, <span class="keyword">Object-Oriented Programming</span>, and [Aspect-Oriented Programming](/aspectprogramming). I call this combination <span class="keyword">polyglot and poly-paradigm programming</span> (PPP).

<span class="keyword">PPP</span> is not a new idea. One of the most successful applications of all time is [Emacs](https://www.gnu.org/software/emacs/), which is still widely used even though it is decades old. Emacs succeeded because it combines a *kernel* written in C, which gives Emacs speed and access to operating system services, combined with a *scripting engine* that uses a Lisp dialect called Emacs Lisp (or ELisp, for short). Most of the functionality of Emacs is implemented in ELisp. It is this scripting capability that has made Emacs so easy to adapt, even by end users, to meet changing needs over the past 30 years.

A more recent example is the Python-based ecosystem for data science, ML, and AI. While users write Python code, most of the performance-sensitive internal code is implemented in C, C++, or Rust.

You can read more about my ideas on polyglot programming in [this old presentation](papers/PolyglotPolyParadigm.pdf) <img src="/assets/images/page_white_acrobat.png" alt="Adobe PDF"/>.

<div class="footnote">
<p>
  <a name="footnote-1"></a>
  <sup>1</sup> Neal Ford may have been the first person to use this term.
</p>
</div>
