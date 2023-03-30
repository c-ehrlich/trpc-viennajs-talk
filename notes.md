# Intro / Background

- About me

  - Chris
  - Self-taught developer, I started going to these meetups when I barely knew anything and they helped me level up fast
  - Currently working for Moody's Analytics. We might be hiring? Not sure.
    - Mostly frontend stuff, and by mostly FE I mean I also do backend and infra
    - But the thing I'm into the most is pushing DX: anything that lets me go faster, and anything that makes me feel smarter

- tRPC doesn't allow you to do anything that you couldn't do with a normal REST api. And using it means worse performance than not using it. But I still love it. It's about making things nicer for you, the developer. And maybe this will allow you to build better things.

# My story

- I'm a self taught developer, and started building my APIs with REST because that's the default

- BUT `/api/foo/bar?thing=this&otherthing=that&somenumber=123&someconfig=true`

- Started hearing about GraphQL

  - Queries and Mutations make way more sense than REST
  - some of the GraphQL tooling is pretty great
  - once you get used to type safety it's hard to go back

- BUT

  - I'm not making multiple REST calls per page
  - Over- or under-fetching isn't a problem I have
  - My data isn't very Graph-like
  - I don't like codegen
  - I was using Apollo and kept incorrectly updating my cache
  - My solution to versioning is refreshing everyone's page when the API changes and that's good enough
  - Building a really good GraphQL setup requires several PhDs. There's a whole industry built on it now.

... is this worth it for a project with a tightly coupled frontend and backend?

- Then I tried OpenAPI

  - Promise: "REST with typesafety"
  - It's a great choice for a stable outwards-facing API
    - "We'll build Version 2 of the API over the next 6 months and then release it"
    - ...I'm releasing new API versions several times a day
  - Tooling is clunky
  - Codegen

- Let's stop and look at what I like

  - end-to-end type safety
  - schema based validation
  - the frontend should know the shape of the backend
  - no codegen
  - I want to use React Query

- "I'll build the solution" ... started building my own thing

  - Was already using Next.js and TypeScript
  - Had just fallen in love with Zod
  - Let's share types ... not that hard
  - Let's validate Next.js API endpoints ... not that hard (curried function that takes Zod scheme)
  - Let's share validators ... not that hard
  - Realization: I'm not good enough at library level TypeScript to build this
  - Also still need to share everything manually,

- ...then I found tRPC and it solved all of my problems

# What is tRPC

# Who is it for

whofor.png

# Demo

# RSCs

# Resources (QR Code)

- these slides
- trpc docs
- ct3a
- RSCs repo
- trash dev talk
- my videos

# Questions

======

GQL: schema as a single source of truth is powerful, but it also adds a lot of overhead... and that's where trpc really shines

Small vs Large: you should always use the smallest thing you can

BEST OF BOTH WORLDS (for some types of projects)
=> and some of its own benefits

"types get lost as they cross the wire"
"if there is a web app and one comsumer, there might be easier ways to do things"
- blitz, redwood, remult


- VANILLA EXAMPLE with one route
    - dont need to write any api schema
    - export the type, not the whole router => the type doesnt exist at runtime
    - show it live? "like you're in the same project"

- probably nice for
    - full stack typescripters
    - apps where graphql is overkill
    - existing apps with traditional api-endpoints
- don't use it if
    - you already have a graphql api
    - you have consumers that aren't written in TS (use graphql)
    - want your app to be consumed by people outside of your company



tRPC vs Remult
similar: typesafe RPC with validation

using TS classes as "single source of truth" ... my single source of truth is the database schema
remult advantages: more "automatic"
crud based on model
frontend functions that correspond to this, they do their own invalidation/caching/optimistic updates

I dont like:
- codegen
- decorators
- custom stuff - how hard is it to switch back to Next api routes, express, or fastify? 


LAST SLIDE "wait theres more"
- invalidation
- optimistic updates
- schema validation on forms

server components
compilers etc
- frameworks are doing similar things without using trpc similarly
- cross pollination between trpc, solid, remix contributors
- dan abramov "we're working to allow these ideas to win"
- ideally tRPC will become obsolete one day because all the frameworks get so good at this stuff that we no longer need it, but i actually think there will be a future for it