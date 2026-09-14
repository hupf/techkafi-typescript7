---
# try also 'default' to start simple
theme: "@puzzleitc/slidev-theme-puzzle"
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
# background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Tech Kafi TypeScript 7
info: |
  ## Tech Kafi TypeScript 7
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable Comark Syntax: https://comark.dev/syntax/markdown
comark: true
# duration of the presentation
duration: 35min
layout: cover
---

# <span class="highlight">TypeScript 7</span><br>A Fast & Native Port

Puzzle Tech Kafi, 17.09.2026 \
Mathis Hofer \
<a href="mailto:hofer@puzzle.ch">hofer@puzzle.ch</a>

---
layout: quote
class: text-center
---

# <span style="font-size: 3em">125.7s → 10.6s</span>

VSCode (1.5M LOC) built with TypeScript 7.0: \
11.9x faster, -18% less memory usage ([Source](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/))

---
layout: agenda
---

# Agenda

- Act I — Why port TypeScript to Go?
- Act II — What changes with TypeScript 7.0?
- Act III — Can you use it?

---
layout: intro
---

# Act I<br><span class="highlight">Why port TypeScript to Go?</span>

---

# A self-hosted compiler

`tsc` has always been TypeScript, compiling TypeScript on Node.js since development started in 2010

- Eat you own dogfood → part of ecosystem
- Runs everywhere

---

# The limits of self-hosting

- No shared memory concurrency (single-threaded)
- Performance penalty (2-3x worse than native code)
- Memory usage (GC pressure from millions of short-lived objects)

→ It scaled with the codebases — until it didn't.

---

# The slow part was never the build

What people mean when they say "TypeScript is slow":

- Open a file in a large codebase → **17.5s**<sup>*</sup> before you see the first error
- Autocomplete that arrives after you've finished typing
- Language server crashes, "TypeScript server restarted"

<div style="font-size: 0.5em">

<sup>*</sup> VSCode code base with TS6

</div>

---

# Meanwhile, everything else got fast 🚀

esbuild, swc, oxc, Biome, Vite, Bun — a whole generation of native tooling.

→ Bundling and transpiling went from seconds to milliseconds.

**But none of them type-check, they only strip types!**

Erasing `: string` is easy. Deciding whether `: string` is _correct_
is the hard part, and it is the entire value of TypeScript.

→ `tsc` ended up the slowest step in a toolchain that was otherwise instant.

---

# Native implementation

<div style="display: flex; gap: 1rem; margin-bottom: 4rem">

<div style="flex: 1">

**Rewrite**

- huge effort
- new structure
- new logic
- different output

</div>

<div style="flex: 1">

**Port**

- managable effort
- same structure
- same logic
- same output

</div>

</div>

> Boring, compatible and shipped beats exciting and subtly different.

---

# A native TypeScript compiler

- No new type-system features
- No new syntax
- A faithful port, deliberately

→ Code that compiles cleanly under TS 6.0 (with `stableTypeOrdering` on, no `ignoreDeprecations`) compiles identically under 7.0.

---

# Why Go? And not Rust?

The existing compiler is closures, higher-order functions and **cyclic graphs**,
all **garbage-collected**.

- **Go** maps onto that almost 1:1 — GC, structural types, first-class functions, real concurrency
- **Rust's** ownership model would have forced a redesign of exactly those data structures

→ This is not a dunk on Rust. Different tool, different job.

---
layout: center
---

# Why Go? And not Rust?

<iframe width="560" height="315" src="https://www.youtube.com/embed/cywK3XYYJ2o?si=QK5UiEvZt5V6PhNA&amp;start=635" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---
layout: center
---

# Did they port it all with AI?

<iframe width="560" height="315" src="https://www.youtube.com/embed/cywK3XYYJ2o?si=1LtSNgcJiNwIKR-K&amp;start=904" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

# 16 months, exactly as announced

|              |                                                                                      |
| ------------ | ------------------------------------------------------------------------------------ |
| **Mar 2025** | "A 10x Faster TypeScript" — Anders Hejlsberg announces the Go port, codename _Corsa_ |
| **May 2025** | `npx tsgo` — anyone can try the preview                                              |
| **Dec 2025** | Public progress report                                                               |
| **Mar 2026** | **TypeScript 6.0** — the bridge release                                              |
| **Jul 2026** | **TypeScript 7.0** — generally available                                             |

→ 6.0 exists to carry the deprecations and give everyone a migration step.

<!--
The story beat here: they said exactly what they were going to do on day one — native port ships
as 7.0, JS codebase continues as 6.0 — and then did that, on that schedule. For a compiler rewrite
that is genuinely unusual.

Hold the 6.0 point. It's the setup for Act III: the migration work you actually have to do is the
6.0 work, not the 7.0 work.
-->

---
layout: center
class: text-center
---

# `tsgo` → `tsc`

For 16 months the preview binary was called **`tsgo`**

In 7.0 `tsgo` is gone, `tsc` _is_ the native compiler now.

---
layout: intro
---

# Act II<br><span class="highlight">What changes with TS 7.0?</span>

---
layout: intro
---

# Act III<br><span class="highlight">Can you use it?</span>

---

# Sources

- https://devblogs.microsoft.com/typescript/typescript-native-port/
- https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/
- https://www.youtube.com/watch?v=cywK3XYYJ2o
