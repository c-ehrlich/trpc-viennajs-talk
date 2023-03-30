---
# try also 'default' to start simple
theme: default
class: "text-center flex flex-col items-center"
highlighter: shiki
# show line numbers in code blocks
lineNumbers: true
# persist drawings in exports and build
drawings:
  persist: false
# page transition
transition: slide-left
# use UnoCSS
css: unocss
layout: center
---

<img src="/img/logo.svg"><br />

## Move Fast & Break Nothing

---
layout: statement
---

# I'm going to talk about a new way of building APIs that...

<div class="flex flex-col items-center max-w-2xl ml-28">
<div class="flex flex-col gap-2 items-start w-auto text-2xl">
<br />
  <div>...doesn't let your app do anything new</div>
  <div>...doesn't improve performance</div>
  <div>...is a nonstarter for many projects</div>
</div>
</div>

---
layout: statement
---

# But it DOES let you...

<br />
<div class="flex flex-col items-center max-w-2xl ml-20">
<div class="flex flex-col gap-2 items-start w-auto text-2xl">
  <div>...build stuff fast</div>
  <div>...feel confident about making changes</div>
  <div>...not lose time to dumb mistakes</div>
  <div>...get an MVP out in record time!</div>
</div>
</div>

---
layout: center
---

<img src="/img/avatar.jpg" class="w-40 rounded-full">
<br />

## I'm Chris (@ccccjjjjeeee) 👋
<br />

* Designer turned teacher turned developer
* Currently doing full stack & DX stuff at Moody's Analytics
* **tRPC** (23k 🌟): Contributor, docs person, and educator
* **Create T3 App** (15k 🌟): Core maintainer
* Hobby is ...<span>*checks notes*</span>... devtools and DX
* This is my first meetup talk 🎉

---
layout: statement
---

# Raise your hand if you have
<div class="text-3xl flex flex-col gap-2">
<div v-click>🙋 used tRPC</div>
<div v-click>🙋 used GraphQL</div>
<div v-click>🙋 used REST</div>
<div v-click>🙋 Built a frontend that talks to an API</div>
<div v-click>🙋 Used React Query</div>
</div>


---
layout: center
---

# How should we build APIs?
- Everyone starts with REST
- It's pretty good
- Tanstack Query makes it even better
<div v-click> but...</div>

---

<br /><br /><br /><br />
# Have you ever lost an hour to something like this?

```ts
example.com/api/foo/bar?thing=this&otherthing=that&sonenumber=123&someconfig=true
                                                     ^
```

---

<h1 class="text-3xl">Have you ever struggled to find the <span class="text-blue-300">api logic</span> that some <span class="text-blue-300">component</span> calls?</h1>

```ts {1-2|4-7|9-10|12-13|15-20}
// frontend/SomeComponent.tsx
const someQuery = useQuery({ queryKey: ["some", "key"], fetchFn: someFetchFn })

// frontend/fetchFunctions.ts
export function someFetchFn() {
  fetch("/api/something/otherthing", { body: { id: "abc123" }}).then(parseRes);
}

// api/index.ts
app.use("/api", require("./something/router"));

// api/something/router.ts
router.post("/otherthing", otherThing);

// api/something/otherThing/handler.ts
async function otherThing(req, res) {
  const id = req.query.id as number;
  const item = await prisma.item.findUnique({ where: { id }});
  return res.status(200).json({ item });
}
```


---
layout: statement
---

## Do you remember if `id` on the previous slide was a string or a number?

and what happens if you send the wrong type?

---
layout: statement
---

## Does your backend logic need to check for every way the request could be _incorrect_, or can you just tell it what a _correct_ request looks like?

---
layout: statement
---

## Did you ever break something by changing backend behavior but not every frontend consumer?

---
layout: center
---

# We already have solutions for this
<br />

#### GraphQL is pretty great!!

- 🔨 Schema as a source of truth / contract
- 🪖 Once you go typesafe you can't go back
- 🤯 Your frontend KNOWS what your backend looks like!!!

<br />

#### (also OpenAPI)

---
layout: center
---

# But GraphQL is not ideal for every project

- Writing a schema is a lot of work
- Codegen is an extra build step
- Ecosystem is complex
- My data isn't very graph-like, I'm just building a more complicated REST API  

---
layout: center
---

# Can we do better?

(by using something that doesn't need to solve EVERY problem)

- REST but with all the best parts of GraphQL
- Frontend knows about the backend
- Input validation in FE and BE (with Zod)
- No codegen / extra steps
- Full-stack Next.js + React Query ❤️

---
layout: statement
---

# I'll build the library for this 🚀
(famous last words)

---
layout: center
---

# ...it kind of worked, but not very well - I'm not a 🧙

(sharing types and validation is easy, letting your frontend know what your backend looks like is HARD)

---
layout: statement
---

# Then I found tRPC

Someone else had already solved my exact problem

---

# What is RPC?

<br />
<img src="/img/rpc.png" class="max-w-150">

<p v-click>Calling a function on one computer from another computer</p>
<p v-click>...isn't that what a REST API is?</p>

---
layout: center
---

# ...and what is `t`?
<br />
<p v-click class="text-4xl">Typesafety</p>

---
layout: center
---

# How does it work?

- Define the backend as a big object
- Export it as a type, import in frontend
- ???
- React Query knows your backend

---

# Vocabulary

- **<span class="text-green-400">Procedure</span>**: API endpoint - can be a **<span class="text-teal-400">query</span>** or **<span class="text-teal-400">mutation</span>**
- **<span class="text-teal-400">Query</span>**: get some data
- **<span class="text-teal-400">Mutation</span>**: change some data
- **<span class="text-blue-400">Router</span>**: a collection of **<span class="text-green-400">procedures</span>** (and/or other routers)
- **<span class="text-purple-400">Context</span>**: stuff that every **<span class="text-green-400">procedure</span>** has access to (db, session)
- **<span class="text-blue-400">Middleware</span>**: do stuff before and after a **<span class="text-green-400">procedure</span>**, can modify **<span class="text-purple-400">context</span>**
- **<span class="text-blue-400">Validation</span>**: "Does this input data contain the right stuff?"

---
layout: statement
---

# Let's look at some code 🤓

---
layour: center
---

# ...there is so much more!

- Form input validation (Zod + hook-form ❤️)
- Easy optimistic UI updates
- Everything else React Query has
- Great error handling
- Easy SSR and SSG
- Procedures are easy to test (using context as "DI")
- tRPC Panel (like GraphQL Playground)
- tRPC-OpenAPI (if you need external consumers after all)
- msw-trpc (easily mock endpoints from your frontend)

---
layout: two-cols
---

# Works with

## Server

- Express
- Fastify
- Next.js
- 3rd party adapters for some others
- Vanilla node
- All major edge solutions

<br />

## Notes
- You should use a monorepo (or a Next.js app)
- One backend can have multiple consumers
- Different backends can call each other's procedures

::right::

<div class="h-14"></div>

## Client

- React / Next.js
- React Native
- Vanilla client runs anywhere JavaScript can run
- 3rd party adapters
  - Vue / Nuxt
  - Solid Start
  - SvelteKit
  - Qwik


---
layout: center
---

# Performance
- tRPC with Next.js is:
  - Faster than Express
  - Slower than Fastify
  - (but seems like Express is fast enough for most people)

<br />
<img src="/img/perf.png" class="max-w-180">

---
layout: center
---

# Who is it for, and who is it not for?

<img src="/img/whofor.png" class="w-160">

---

# Similar projects ("Typesafe REST/RPC")

- **ts-rest, Zodios, Remult**: Explicitly declare the API schema
  - Difference to tRPC: Do you want to declare a contract programmatically, or do you want your backend logic to _be_ the contract?
- **Blitz.js**: Rails-like framework
  - Difference to tRPC: Blitz is more ambitious (full framework, compiler, etc), but you need to commit to it more

---

# Companies using tRPC

* Netflix (for internal tools)
* Stately.ai (XState etc)
* Cal.com (OSS codebase)
* Skill Recordings (Total TypeScript, Kent C Dodds, Dan Abramov, etc courses - OSS codebase)
* Ping.gg
* ...many more

---
layout: center
---

# What about React Server Components?
<br />
<p v-click>- We're working on it! (and waiting for the mutations RFC)</p>
<p v-click>- Are you builing an app or a website? Or something inbetween?</p>
<p v-click>- Most things are inbetween!</p>
<p v-click>- tRPC as an answer to the "Two Applications Problem"</p>
<p v-click>- Will be wild west of competing patterns for a while</p>
<p v-click>- Talk to me afterwards if you want to nerd out about RSCs</p>

---
layout: statement
---

# Questions?

<div class="flex w-full flex-col items-center">
<br />
<br />
<br />
<img src="/img/qrcode.png" class="w-50">
</div>

(scan for resources, videos, etc.)
