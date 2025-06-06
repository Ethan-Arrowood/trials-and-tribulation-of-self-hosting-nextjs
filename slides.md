---
theme: default
background: ./images/harper-background.jpg
title: Trials and Tribulations of Self-Hosting Next.js
info: By Ethan Arrowood and Austin Akers
class: text-center
transition: slide-left
mdc: true
---

# Trials and Tribulations of Self-Hosting Next.js

By Ethan Arrowood and Austin Akers

---
layout: two-cols
---

# Who we are

<img
  class="rounded-full w-32 h-32 mt-24 justify-self-center"
  src="https://avatars.githubusercontent.com/u/16144158?v=4"
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

<!-- Each of us introduce ourselves. Include titles.
Whoever is kicking off should start with a transition along the lines of: "Lets get into it!" -->

---

<img src="./images/harper-simplified-architecture.png" />

<!--
Harper is a serverfull, all-in-one Node.js platform with full fil system access, an embedded database, networking middleware system, and application hosting support.
It is built on top of Node.js and operates in a single process, using worker threads for performance, and production scaling via clustering and replication.
The whole platform works together to provide an high performance, easy to use, enterprise-grade development experience.
-->

---

~HarperDB~ -> **Harper**

<!-- Oh yeah! You may have known us as HarperDB previously, recently we rebranded and our now entirely Harper! You may still see "HarperDB" floating around though as we slowly update the name across all our systems -->

---

<img src="./images/nextjs-org.png" />

<!--
"Next.js is a full-stack react framework for building modern web applications."
"It uses common patterns like server-side rendering, static site generation, and client-side rendering to provide a flexible and powerful development experience."
"But Next.js is more than just what it can create. The entire development experience from prototyping to production is what makes Next.js so powerful."
 -->

---
layout: center
---

# Hosting Next.js is surprisingly hard

<!-- quick slide - say whats on screen and move on - because the hook comes from next slide -->

---
layout: center
---

# Its much more than just `next start`

<!-- Say whats on the slide and then... -->
<!-- Our goal was not just to host Next.js apps, but provide a holistic Next.js application development experience -->

---
---

# Supporting Next.js

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

<!-- either or - goal: display each step one at a time on the same slide. -->

<v-clicks>

- Develop
- Build
- Deploy
- Run

</v-clicks>

<!--
So, we came up with a high-level framework to dictate what it means to support Next.js 
First, "Develop"
Then, "Build"
Next, "Deploy"
Finally, "Run"

"Let's dive right in"
-->

---
layout: center
transition: slide-up
---

# Develop

<!-- The first aspect of our framework is "Develop". For us, this meant parity with Next.js' excellent developer experience -->

---
layout: center
transition: slide-up
---

# `next dev`

<!-- for most developers the first part of Next.js they interact with is the `next dev` command -->

---
transition: slide-up
---

# Dev Mode

<v-clicks>

- Hot Module Reloading
  - _Instant feedback loop where code changes are reflected in the browser without a full page reload._
- Fast Refresh
  - _Preserves component state throughout refreshes, allowing for a smoother development experience._
- Error Overlay
  - _Displays errors and warnings in the browser, making it easier to debug issues._
- Dev-Tools Integration
  - _Displays errors and warnings in the browser, making it easier to debug issues._

</v-clicks>

<!-- This is the high-level feature summary slide of "what is Dev Mode" -->
<!-- probably good enough to just flip through them quickly and continue moving on... -->

---
transition: slide-up
---

# 🔎 Hot Module Reloading

<!-- 
We want to highlight one of the harder aspects of supporting dev mode which was integrating Next.js' Hot Module Reloading with Harper's WebSocket middleware
-->

---
transition: slide-up
---

# What is WebSocket?

<!-- TODO: Keep this brief. Maybe just do the Request/Response HTTP snippets (use the code line highlighter to focus on the `Upgrade & Connection` parts) -->

<!--
  "But what even is a websocket?"
  "Upgraded HTTP connection"
  "Allows for full-duplex, or real-time two-way communication between client and server"
  "The important technical bits is that WebSockets start out as a regular HTTP request that then upgrade to the WebSocket protocol"
  "The Client needs to send an Upgrade request (show upgrade request code snippet) and the server needs to respond with an Upgrade response (show upgrade response code snippet)"
  "Once established, the connection remains open, allowing for continuous data exchange without the overhead of HTTP request polling."
 -->
---
transition: slide-up
---

# Harper has an embedded networking middleware layer

<!-- TODO: Insert Harper's ws and upgrade middleware here. actually include some light code example so its not just an empty function handler -->

<!--
"Remember Harper's architecture diagram? We have a highly extensible networking middleware system"
"This system allows us to hook into the request/response lifecycle and do custom HTTP upgrade and WebSocket connection handling"
(walk through code snippet)
-->

---
transition: slide-up
---

# Next.js Server API

<!-- TODO: Insert Next.js server api code snippet here -->

<!--
"And like we said at the beginning, supporting Next.js is much more than just `next start`."
"It isn't very well known, but Next.js has a server API that allows for programmatic control over the server."
"Next.js provides a `getUpgradeHandler` method that can be used to handle WebSocket upgrades. (walk through code snippet)"
-->

---
transition: slide-left
---

# Putting it all together

<!-- TODO: Insert complete code snippet of Harper and Next apis together -->
<!--
  (Walk through code snippet step by step)
  Remember to keep it simple, focussed, and succinct.
-->

--- 

# And finally, don't forget about `harperdb-nextjs dev`

<!-- 
"Since Harper is a complete platform, and we run their application within the Harper process, we created our own CLI experience "
"It is quite simple, but it is important that it bootstraps Harper and ensures the right environment is set up for Next.js development (including enabling the HMR support!)."
-->

---
transition: slide-up
layout: center
---

# Build

---
transition: slide-up
---

`harperdb-nextjs build`

<!--
The next step in our framework is to support building Next.js apps
This was where our CLI experience really started to take shape
Just like the `next dev` command, we created a `harperdb-nextjs build` command that would run the Next.js build process and then cleanly exit
Since Harper is a whole platform, we needed to ensure the system was started up, the build ran, and then the process exited cleanly
-->

---
transition: slide-up
---

# Single Process, Multi-Threaded

<!-- TODO: Diagram the file lock - this might be good for a mermaid flow chart? the logic is pretty easy to follow when documented -->

<!-- 
Given that Harper is a single process, multi-threaded platform, we needed to ensure that the build process was isolated
Threads in Harper generally all do the same thing unless we specify otherwise.
We are actively improving this experience, but at the beginning we created a custom file-based thread locking mechanism to ensure that only one thread would run the build process.
(step through diagram of file locking mechanism - highlight the key steps like checking mod time and pieces like that)
 -->

---
transition: slide-up
---

<!-- Consider starting the Deploy section here? This is definitely the best transition -->

# Unlike Serverless, No Ephemeral Builds/Deployments

<!-- Now transition into the Deploy section by first talking about architecture -->

<!-- 
Unlike serverless platforms that treat each build and deployment as a unit of work, Harper is a long running process.
We couldn't really provide the same ephemeral build experience without completely ~blowing up~ (use better word here) our users's Harper instances (turns out build artifacts can add up quite quickly!!).
So instead, at least for now, we kept things simple. Builds can happen either locally or remotely, but they will always override what is currently running.
And this leads us to the next step in our framework: Deploy.
 -->

---

# Deploy
<!--
Naturally, after development its time to get your application live
-->

---

<!-- TODO diagram of cluster/replication -->

<!-- Now we briefly mentioned that Harper is a long running process, and for our production systems often clustered and replicated across regions. -->

---

Rollouts

<!-- Going back to the idea of Ephemeral deployments, remember that Harper is all-in-one platform. 
You wouldn't want your entire database to be reset every time you deployed your web app, right? 
So our solution is to use a rolling deployment system. This is particularly linked to our replication system where the function of _deploying_ the app is simply an operation like any other on the platform.
And so as long as we indicate it should be replicated, then every instance in the network will receive the deploy operation.
-->
---

`harperdb deploy`

<!-- 
Note: this isn't tied to the Next.js CLI anymore
Reiterate the idea that deployments is a general operation on Harper, not just Next.js
Make the point that we discovered this part of the framework was easiest to abstract and generalize to support all kinds of applications, not just Next.js.

Depending on time... this might be a good place to expand a little bit into Harper as a platform, but remember no sales pitches!
-->

---

# Run

<!-- Finally, the app is build and deployed, now its time to run it! -->

---

No, we still aren't using `next start`

<!-- Relate back to the joke at the beginning -->

---

Next.js Server API again

<!-- INSERT code snippet about Harper HTTP middleware and next server getRequestHandler. Do it all at once this time since its simpler than WS/Upgrade -->

<!-- 
So just like we used the Next.js Server API to handle WebSocket upgrades, we can also use it to run the application.

We can use the `getRequestHandler` method to get a request handler that can be used to handle incoming requests.

(walk through code snippet of using getRequestHandler to create a request handler and then use it with Harper's HTTP server)
 -->

---

Version Compatibility

<!-- INSERT code snippet showing off dynamic import based on component directory -->

<!-- 
Next.js has many versions, and honestly it takes a lot of time for users to migrate and upgrade. so it was imperitive we support multiple versions of Next.js.

We do this by using the applications included Next package and dynamically importing it. 

(walk through code snippet and touch on both dynamic imports in Node as well as the fact that our extension system gets the file system of the application and can use that to its advantage)

 -->

---

Short story about Next 9

<!-- Maybe funny graphic or gif or meme? -->

<!-- Furthermore, we had the unique challenge of supporting a very old Next.js version!
With Next 9 it actually worked surprisingly well, but unfortunately we had to drop support for the dev mode because it didn't have any upgrade handler API.
Kudos to the Next team for maintaining backwards compatibility for so long!
 -->

---

Multi-Zone support

<!--
We probably sound like a broken record at this point, but Harper isn't your isolated, serverless environment host.
We have the ability to run multiple applications in the same process, and this naturally lead us to exploring support for Next.js multi-zone applications (micro-frontends).
 -->

---

<!-- Insert diagram of handler flow -->

<!-- 
One of the key details is that our handler system naturally supported simplified, url based passthroughs.
So we load the apps in reverse specificity order, and as the request flows through the handler chain, it will match the first app that has a handler for the request.
This allows us to support multi-zone applications without any additional configuration.
-->

---

... With one exception

---

Working Directory Limitation

<!--
One of the biggest limitations of being a single process platofrm is that we have a single working directory.

This means that we cannot run multiple Next.js applications in the same process if they have conflicting working directories.

Now Next.js itself doesn't have this limitation, but some application dependencies do.

We haven't really found a workaround for this, but its generally avoidable with good application design and implementation
-->

---

Custom Cache Handling
<!-- Bonus future work section if we have the time for it -->

---

---
layout: section
---

# What's Next?

Open issues: working directory limitation, full test coverage, feature parity.

What excites you: pushing custom caching deeper, extending to other frameworks.

---
layout: section
---

# Conclusion

Recap the lifecycle: Dev → Build → Deploy → Run.

Key takeaway: You can self-host Next.js, but it’s more than just npm start.

Invite folks to dig into the GitHub repo, ask questions.


