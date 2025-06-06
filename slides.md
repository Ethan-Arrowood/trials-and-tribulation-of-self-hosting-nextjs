---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
# background: https://cover.sli.dev
background: ./images/harper-background.jpg
# some information about your slides (markdown enabled)
title: Trials and Tribulations of Self-Hosting Next.js
info: By Ethan Arrowood and Austin Akers
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# open graph
# seoMeta:
#  ogImage: https://cover.sli.dev
---

# Trials and Tribulations of Self-Hosting Next.js

By Ethan Arrowood and Austin Akers


---
layout: intro
---

# Overview

- 🎨 **Introduction**
- 🧑‍💻 **What does it mean to support Next.js?**
- 🧑‍💻 **What's Next?**
- 🧩 **Conclusion**


---
layout: section
---

# 👋 Introduction

---
layout: two-cols
---

# Who are we?

<img
  class="rounded-full w-32 h-32 mt-24 justify-self-center"
  src="https://ethanarrowood.com/_astro/carrying_lincoln_cropped.Bsqiu0N-_hBO9Y.webp"
  alt="Ethan Arrowood"
/>

<h2 class="text-center">Ethan Arrowood</h2>

::right::

<img
  class="rounded-full w-32 h-32 mt-32 justify-self-center"
  src="https://avatars.githubusercontent.com/u/11778717?v=4"
  alt="Austin Akers"
/>

<h2 class="text-center">Austin Akers</h2>

---
---

## The Problem?

<!-- State the problem: self-hosting Next.js is surprisingly hard.

Set expectations: “We’ll walk through how we support the entire Next.js lifecycle on our platform.”
 -->



---
layout: section
---

# 🎨 What Does It Mean to Host Next.js?


--- 
---

# 4 Key Areas

<div class="grid grid-cols-4 gap-4 mt-16 text-center">

<div>
Develop
</div>
<div>
 Build
</div>
<div>
 Deploy
</div>
<div>
 Run
</div>

</div>


---
---

# Develop
Goal: Make dev experience feel native.

Challenge: Next.js dev mode requires WebSocket + HMR + file watching.

Solution:

How you used Harper’s networking middleware to inject upgrade handlers.

WebSocket support, HMR, filesystem watching.

Show code sample, graphics.

Mention the custom CLI here—harper dev bootstraps Harper + Next.js cleanly.

---
---

# Build

Not serverless = no ephemeral builds.

You run next build directly, but inside Harper’s process.

CLI again: supports harper build.

Mention experimental cache integration here (build output caching, potential future enhancements).

---
---

# Deploy

Harper’s model: Always-on, long-lived process with rollouts.

Challenge: Users may deploy raw source or built apps.

Solution:

Extension system can run either.

Rolling deployment to clusters with zero downtime.

Briefly touch on CLI support (harper deploy).

---
---

# Run

Actually serving the Next.js app, SSR, API routes, etc.

Version Compatibility:

Dynamic import of correct Next.js version.

Support from v9 to latest.

WIP: testing matrix, feature support.

Multi-Zone support:

Harper can support multiple apps.

Routing, isolation, challenges with working directories.

Custom Cache Handling (bonus/experimental):

Next.js custom cache handlers.

How Harper's internal DB integration avoids round-trips.

---
layout: section
---

# 🎨 What's Next?

Open issues: working directory limitation, full test coverage, feature parity.

What excites you: pushing custom caching deeper, extending to other frameworks.

---
layout: section
---

# 🎨 Conclusion

Recap the lifecycle: Dev → Build → Deploy → Run.

Key takeaway: You can self-host Next.js, but it’s more than just npm start.

Invite folks to dig into the GitHub repo, ask questions.


