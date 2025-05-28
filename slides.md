---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
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
layout: two-cols-header
---

# Who we are

::left::

<img
  class="rounded-full w-32 h-32 mb-4"
  src="https://ethanarrowood.com/_astro/carrying_lincoln_cropped.Bsqiu0N-_hBO9Y.webp"
  alt="Ethan Arrowood"
/>
## Ethan Arrowood

::right::

<img
  class="rounded-full w-32 h-32 mb-4"
  src="https://avatars.githubusercontent.com/u/11778717?v=4"
  alt="Austin Akers"
/>
## Austin Akers

---
layout: intro
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

---
layout: section
---

# 🎨 Next.js on Harper

Next.js is a React framework that enables server-side rendering and static site generation for React applications. It is designed to make building production-ready applications easier and faster.

---
layout: section
---

# 🧑‍💻 Dev Mode Support

Dev Mode === Developer Experience

<!-- 
  As discussed in the previous section, Harper is a complete, full-stack application platform.
  As we integrated Next.js we wanted to ensure a quality developer experience. 
  Next.js' dev mode is a critical part of that experience.
-->

---
layout: center
transition: slide-up
---

# What is _Dev Mode_?

---
layout: center
transition: slide-up
---

# Hot Module Reloading

_Instant feedback loop where code changes are reflected in the browser without a full page reload._

---
layout: center
transition: slide-up
---

# Fast Refresh

_Preserves component state throughout refreshes, allowing for a smoother development experience._

---
layout: center
transition: slide-up
---

# Error Overlay

_Displays errors and warnings in the browser, making it easier to debug issues._

---
layout: center
transition: slide-left
---

# Dev-Tools Integration

_Component inspection, performance profiling, and more._

---
layout: center
---

# 🔑 Improved developer experience

<!-- Again, the key to all of this is improving developer experience -->

---
layout: center
---

# 🔎 Hot Module Reloading

<!-- Today, we are going to focus on the Hot Module Reloading part and how we leveraged Harper's server middleware api to forward the necessary WebSocket requests through to the Next.js dev server -->

---
layout: center
---

```mermaid {scale: 0.6}
sequenceDiagram
  participant b as Browser
  participant s as Dev Server
  participant f as File System
  s -->> f: Watch Web App Source Files
  b ->> s: Request App
  s ->> b: Send App with HMR Injected
  b ->> b: Render App
  b -->> s: Establish WebSocket connection for HMR
  loop Hot Module Reload
    f -->> s: File change detected
    s -->> b: Send update event to Browser
    b ->> s: Request update data
    s ->> b: Send update data
  end
  b ->> b: Render updates
```

<!-- Go through the diagram step by step. Starting from the top -->

---
layout: center
---

# 🕸️ The WebSocket API 🔌

---

- RFC 6455 (December 2011)
- Enables real-time communication **without** traditional HTTP polling
- Client/Server architecture
- TCP based & HTTP Upgrade mechanism
  1. Client sends HTTP request with `Upgrade: websocket` header
  2. Server responds with `101 Switching Protocols`
  3. Connection is established
  4. Data can be sent in both directions

<!-- Note: maybe diagram here? Important to describe that WS works via HTTP Upgrade Request and then the two way connection is established -->

---
layout: center
---

# Remember: Harper is an integrated platform
It has its own HTTP and WebSocket support.

---

# Harper Server API

```javascript
// Custom TCP socket handling (similar to `net.createServer`)
server.socket(connectionListener, options);

// Custom HTTP request handling 
server.http(requestListener, options);

// Custom HTTP upgrade handling
server.upgrade(upgradeListener, options);

// Custom WebSocket connection handling
server.ws(webSocketConnectionListener, options);
```

These methods allow developers to define custom handlers for various networking operations, enabling the creation of custom protocols or the integration of existing ones.

<!-- Similar to other Node middlewares, the Harper server API allows you to define custom handlers for specific networking operations -->

---

# Next.js Server API

Most users only ever interact with Next.js through the `next` CLI (i.e. `next dev`, `next build`, `next start`).

However, Next.js can be used programmatically too!

```javascript {all|11-12}
// As of Next.js v13, v14, and v15:
import next from 'next';

const app = next({ dev: true });

await app.prepare();

const requestHandler = app.getRequestHandler();
// type RequestHandler = (req: IncomingMessage, res: ServerResponse, parsedUrl?: any) => Promise<void>;

const upgradeHandler = app.getUpgradeHandler();
// type UpgradeHandler = (req: IncomingMessage, socket: any, head: any) => Promise<void>;
```

<!-- Unfortunately, its not well documented, but the important parts are: -->

---

# All together now...

```javascript {1-3,11|4,8|5-7|10}
const upgradeHandler = app.getUpgradeHandler();

server.upgrade((req, socket, head, next) => {
  if (req.url === '/_next/webpack-hmr') {
    return upgradeHandler(req, socket, head).then(() => {
      return next(req, socket, head);
    })
  }

  return next(req, socket, head);
}, { runFirst: true });
```

<!-- 
1. Get the upgrade handler from Next.js and setup the Harper upgrade handler.
  a. Use the `runFirst` option to ensure that the Next.js upgrade handler runs first.
2. Then inspect the request URL to see if it matches the Webpack HMR endpoint.
3. If it does, call the Next.js upgrade handler to upgrade the connection.
  b. There is some additional nuance to this that I'm glossing over here, but the key is that even after upgrading its important to call `next` so that additional middleware can run.
4. And if it  doesn't match, just call `next` to continue processing the upgrade request.
 -->

---
layout: center
---

# 🎉

<!-- And just like that, we have enabled hot module reloading for Next.js dev mode! Short demo video maybe? -->

---

# 🧩 Version Compatibility

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

