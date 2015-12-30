+++
categories = ["setup", "tools", "atom", "markdown", "latex"]
date = "2015-12-29T23:55:08-03:00"
description = "Lint your spelling while writing"
keywords = ["setup", "tools", "atom", "markdown", "latex"]
title = "LaTeX and Markdown spell check in Atom"
+++

{{% figure src="/images/atom-spellcheck.gif"%}}

I use Atom for everything. In the last days, I have wrote a lot of text in Markdown and LaTeX, but english is not my main language so I spend some time searching lint tools and plugins to help me out.

Atoms comes with a spell-check plugin, but it does not lint every file type.

To enable Markdown and LaTeX:

```
text.tex.latex, source.gfm
```

{{% figure src="/images/atom-spellcheck-settings.png"%}}

> `CMD+SHIFT+.` to show the suggestions
