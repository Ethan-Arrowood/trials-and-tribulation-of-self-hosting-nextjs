---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Trials and Tribulations of Self-Hosting Next.js
info: By Ethan Arrowood and Austin Akers

  Learn more at [Sli.dev](https://sli.dev)
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
transition: fade-out
---

# Who we are

<div class="grid grid-cols-2 gap-4 mt-32">

<div class="text-center">
  <img
    class="rounded-full w-32 h-32 mx-auto mb-4"
    src="https://ethanarrowood.com/_astro/carrying_lincoln_cropped.Bsqiu0N-_hBO9Y.webp"
    alt="Ethan Arrowood"
  />
  <h2>Ethan Arrowood</h2>
</div>
<div class="text-center">
  <img
    class="rounded-full w-32 h-32 mx-auto mb-4"
    src="https://avatars.githubusercontent.com/u/11778717?v=4"
    alt="Austin Akers"
  />
  <h2>Austin Akers</h2>
</div>
</div>

---
transition: fade-out
---

# Overview

- 🎨 **Next.js on Harper**
- 🧑‍💻 **Dev Mode Support**
  <!-- WebSocket connection handling. Harper middleware system. -->
- 🧩 **Version Compatibility**
  <!-- Fairly simple section, but highlight how we use dynamic imports. Throw in there the like high-level idea of using 
  defensive code patterns to support backwards compatibility. We can particularly highlight that we had to demonstrate 
  compatibility with old Next.js versions just as a function of business. And then point to the exact line where we made 
  sure to defensively check that the dynamically imported Next server actually had the websocket/devmode hooks that we needed. -->
- 🚧 **Working Directory Platform Limitations** 
  <!-- While relevant to deployments too, this is particularly about how Harper is 
  itself a platform and thus the working directory may not able available to be set to the Next project. 
  Next itself is fine with this, but not all dependencies are. i.e. react-storefront -->
- 🚀 **Deployment Experience**
  <!-- Build mode support, analyzing the build output before pushing to production. integration with CI systems. 
  We can talk about Harper's particular component application deployment process, but relate it back to the expected 
  default experience for deploying Next.js i.e. Vercel and Netlify's default experience. ANd how we emulated that. -->
- 🗄️ **Custom Cache Handling**
  <!-- We actually didn't totally do this meaning we didn't add a custom Next.js cache handler (which is apart of the 
  Next.js server api). But as a platform we did investigate and add support for general request caching. i.e. plain http 
  request caching. harperdb/http-cache module. If time permits we can go into this part, but can also omit since we haven't 
  actually solved it or fully implemented it yet. May just be good to mention as like what we want to do next! -->
<br>
<br>

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/features/slide-scope-style
-->

<!--
Here is another comment.
-->

---
transition: slide-up
---

# Next.js on Harper

Next.js is a React framework that enables server-side rendering and static site generation for React applications. It is designed to make building production-ready applications easier and faster.

---
transition: slide-up
---

## Dev Mode Support

- WebSocket connection handling
- Harper middleware system

---
transition: slide-down
---

## Version Compatibility

- Dynamic imports
- Defensive code patterns
- Backwards compatibility
- WebSocket and dev mode hooks
- Harper middleware system
- HarperDB HTTP cache module

---
transition: slide-left
---

## Working Directory Platform Limitations

- Harper is a platform
- Working directory may not be available
- Next.js is fine with this

---
transition: slide-right
---

## Deployment Experience
- Build mode support
- Analyzing the build output
- Integration with CI systems
- Harper's component application deployment process
- Vercel and Netlify's default experience
- Emulating that experience

---
transition: slide-up
---

## Custom Cache Handling
- HarperDB HTTP cache module
- General request caching
- Next.js cache handler
- Future work
- What we want to do next

---
transition: slide-up
---

## Conclusion
- Trials and tribulations of self-hosting Next.js

