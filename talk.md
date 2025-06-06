I'm having a hard time writing a conference talk. I have a lot of information and many ideas for structure/flow but just nothing is quite sticking or coming together. I need help organizing my thoughts.

I'm going to share as much information as I can about the talk, including the title, abstract, learning outcomes, background information, and what I've come up with so far. I will also share some of my additional ideas for what the talk could cover. 

Your task is to help me organize this information into a coherent outline for the talk. You can ask me questions to clarify any points or to get more information.

The talk is titled "Trials and Tribulations of Self-Hosting Next.js".

Abstract:
When we set out to support Next.js applications on HarperDB, we quickly discovered that the path from next dev to a production-ready hosting solution was far from straightforward. This talk dives deep into the architectural decisions, technical challenges, and engineering solutions that went into building a flexible, scalable Next.js hosting platform. You'll learn how we tackled version compatibility (going back as far as Next.js v9!), engineered for scalability using worker threads, and all the meanwhile maintained local development experience parity. Whether you're considering self-hosting Next.js applications or interested in advanced deployment architectures, this session will equip you with practical insights and battle-tested strategies.

Learning Outcomes:
Attendees will learn many things from this talk. At a high level, they will walk away with the key architectural considerations and technical challenges involved in self-hosting Next.js applications at scale. More concretely, they will learn about the technical complexities of supporting multiple Next.js versions, supporting dev mode via WebSocket servers, and optimizing build operations with worker threads. They will learn all of this while maintaining compatibility and performance.

Additional Information:
The talk length should be no more than 25 minutes.

I am co-presenting this talk with another engineer from Harper.

The audience is developers with a variety of experience levels. We want the talk to be accessible to everyone, but we don't want it to be too basic for experienced developers. Nor do we want to waste time explaining concepts that are not relevant to the core of the talk.

Background Information:
It is a story of how we at Harper figured out how to self-host Next.js, the challenges we faced, and the solutions we found. We also highlight parts we are still working on.

What makes Harper unique compared to other, more common hosts like Vercel, Netlify, or self-hosted on AWS or Cloudflare, is that Harper is an all-in-one Node.js platform. It has a built-in database, a complete networking middleware system (net, http, ws, mqtt, etc), direct access to the file system, and more. It is single-process and uses worker threads for concurrency. The actual platform feature set and networking middleware system is run on all threads. Application support is growing. It start as a simply a mechanism for like declaring database schemas and some custom database resources (essentially functional database tables). Now we've expanded the application system extensively so we can create "extensions" that can provide support for things like Next.js, Apollo, Astro, etc. The way these work is the extension hooks into the platform, generally the networking middleware, and then provided the actual application directory, can run that application. You can see the source code for the Next.js extension here: https://github.com/HarperDB/nextjs/blob/main/extension.js

Furthermore, unlike the same common Next.js hosts, Harper is not a serverless platform. We don't do ephemeral builds or deployments. Generally, the application is always running and we make us of clustering and replication to scale the platform globally. For application deployment, the user can send the source files or a built application to the platform, and using the extension, we can run it. We rollout the deployments so that there is zero downtime.

The outline we have is:
- Introduction
- Next.js on Harper
  - What is Harper?
    - Talk about the technical architecture of Harper as an all-in-one Node.js platform
    - How does Harper differ from other platforms?
    - Extension system
  - What is Next.js? What does it mean to "host Next.js"?
    - Next.js is a full-stack framework. Need to support server side execution
    - What is core to Next.js app development? 
      - Develop, Build, Deploy, Run
    - Developer Experience is core to Next.js - how can we maintain that?
  - What specific aspects are we focussing on in this talk
- Dev Mode Support
  - The story of Dev Mode Support is a great technical journey
  - Next.js provides an upgrade handler for dev mode. How did we hook into this with Harper's networking middleware?
  - What is WebSockets? How does HMR work?
- Version Compatibility
  - We demonstrated that we are using the included Next.js server to run Next.js applications.
  - How do we ensure we are running the right version?
    - We used dynamic imports to load the Next.js server at runtime.
  - We experimented with a testing matrix, but it is very hard! Still a WIP
  - What are the challenges of maintaining compatibility with older versions?
    - EOL Node.js versions, security vulnerabilities, missing features
    - In particular, Next.js v9 doesn't have the same dev mode support as later versions. In our code we just use a version check to determine if we can use the dev mode support. 
- Multi-Zone Support
  - Multi-Zone is a niche feature of Next.js, but very important to enterprise users.
  - Traditionally, each part would have to be deployed separately, but Harper can support multiple apps simultaneously.
  - How does Harper's extension system support this? Middleware system!
  - But issue with working directory
    - Most don't really think about it, but some Next apps actually care about the working directory being the root of the app.
    - On Harper this isn't possible. We don't really have a work around for this. But luckily this doesn't affect most apps; only those using dependencies that are dependent on the working directory.
- Deployment Experience
  - A key aspect to other platforms, how did we handle it?
  - We don't do ephemeral builds or deployments, so how does that work?
  - We use the extension system to run the application directory.
  - We can run the application directory directly, or we can run a built application.
  - Rolling deployments for zero downtime
- Custom Cache Handling
  - How does Harper handle caching for Next.js applications?
  - We have built basic agnostic request/response caching that sits in front of the HTTP handlers
  - But how can we take this to the next level?
  - Next.js server provides support for custom cache handlers, how are we experimenting with this?
  - We have direct access to the database system on Harper so we can do cache reads/writes without any network overhead!
- Conclusion

Work Done So Far:

I've written the Dev Mode Support section and Multi-Zone Support sections. They are okay. I have lots of graphics, some code samples, did my best to lightly cover the larger concepts. I'm most of the way done with the Deployment Experience section, but it is quite rough and almost seems too light on content. I'm worried the versions compat section would be similarly too simple. I'm excited for the Custom Cache part even though its experimental work. There will just be some good code samples for that one. One aspect to the implementation I'm having a hard time fitting in is the custom CLI we created. We built it to emulate the Next.js CLI so that build, dev, and start all also kickstart and configure Harper correctly. And that brings me to my other, larger concern, is the outline okay? I feel like maybe I'm approaching the talk wrong and reorganizing the ideas could result in a better flow. While collaborating on the Next.js On Harper section I came up with a simple breakdown for what it means to support Next.js. I came up with the fact that for Next or for any web app framework, there are four core aspects: Develop, Build, Deploy, and Run. I think this could be a good way to structure the talk, but I'm not sure how to fit it in. But also, I'm not entirely confident with upending the talk at this point. 

I'm trying to keep in mind that this is a shorter, 25-minute talk. The audience is a wide range of experience levels, and honestly we have quite the interesting story to tell being a full platform instead of the traditional microservice approach. I want to stay away from comparisons or this becoming too much of a sales pitch for Harper, but I'm excited about the tech we built and I want that to shine.

So, what do you think? Do you have any other questions or ideas? Where can I go from here? 



Austin Show:

Two avenues:
1. Telling showing presenting the challenges we encountered.
   1. What to be aware of
   2. Related to platform like ours
   3. And here is our solutions low and high level
   4. Example: enabling HMR so we match dev exp of regular Next.js development
2. What are we looking for takeaways
   1. integration in _your_ platform
   2. 