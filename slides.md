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
layout: two-cols
---

# Who we are

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

<!-- Each of us introduce ourselves -->
<!-- Include titles -->
<!-- Definitely somehow state that we are both engineers at Harper -->

---

Real quick... What is Harper? 

<!-- Serverful, all-in-one Node.js platform -->

<!-- Diagram with basic architecture -->

<!-- The transition will come from the top most part of the architecture diagram which stats "Application Hosting" -->

---
layout: center
---

## Hosting Next.js is surprisingly hard

<!-- There is a lot to it! -->

<!-- quick slide - say whats on screen and move on - because the hook comes from next slide -->

---

## Its not just `next start`

<!-- Say whats on the slide and then... -->
<!-- Our goal was not just to host Next.js apps, but provide a holistic Next.js application development experience -->

---

## Framework for supporting Next.js

<!-- So, we came up with a high-level framework to dictate what it means to support Next.js -->

<!-- <div class="grid grid-cols-4 gap-4 mt-16 text-center">
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
</div> -->


<v-clicks every="1">

- Develop
- Build
- Deploy
- Run

</v-clicks>

 <!-- either or - goal: display each step one at a time on the same slide. -->

---

## What is Next.js? 

<!-- logo, screenshot of Next website -->
<!-- Next.js is a full-stack react framework. -->
<!-- It has continue to popularize patterns such as SSR, SSG, and it focusses on an excellent developer experience.  -->

---

## What is Harper?

<!-- "We've already mentioned, Harper is a serverful, all-in-one Node.js platform" -->
<!-- "And more technically we are...." -->
<!-- A little more technical -->
<!-- Reiterate all-in-one Node.js platform -->
<!-- Single process, worker threads for performance, production scaling via clustering and replication -->
<!-- Built-in Database, Complete File System access, Application Hosting -->

<!-- "Lets dive in" "Lets get started" -->

---

# Develop

<!-- The first aspect of our framework is "Develop". What this meant to us was parity (or maintaining) Next.js' excellent developer experience -->

---

# Supporting Next.js dev mode

<!-- for most developers the first part of Next.js they interact with is the `next dev` command -->

---

# Dev mode can mean a lot of different things

There features like:
- Hot Module Reloading
- Fast Refresh
- Error Overlay

And there is tooling like
- `next dev` CLI interface
- Dev-Tools Integration

<!-- This is the high-level feature summary slide of "what is Dev Mode" -->
<!-- probably good enough to just flip through them quickly and continue moving on... -->

---

# Hot Module Reloading
<!-- We want to highlight one of the harder aspects of this which was supporting Hot Module Reloading -->

---

# HMR is powered by WebSockets

---

# Harper has an embedded networking middleware layer

---

# How do we integrate Next.js HMR with Harper's WebSocket server?

---

<!-- Show and explain Next.js server API and getUpgradeHandler method (1 min) -->

<!-- Show and explain (briefly) Harper's server middleware (1 min) -->

<!-- Show how to combine them together to support HMR via Harper -->

---

# Build


---

# Deploy


---

# Run


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


