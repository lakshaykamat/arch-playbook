# Architectural Patterns Guide

A reference guide for developers who want to understand, choose, and evolve their architecture — whether they're starting fresh or dealing with a codebase that's outgrown its original structure.

---

## Backend Architectural Patterns

### System Architectures

| Architecture          | Description                                  | When to Use                    | Pros                                      | Cons                                   | Frameworks (Node.js / Python)                          | Use Cases                         |
| --------------------- | -------------------------------------------- | ------------------------------ | ----------------------------------------- | -------------------------------------- | ------------------------------------------------------ | --------------------------------- |
| Monolith              | Single deployable app                        | MVPs, small apps               | Simple to build and deploy                | Hard to scale or isolate modules       | Express, Flask, FastAPI, Django                        | Dashboards, admin panels          |
| Modular Monolith      | Organized monolith with domains/modules      | Growing apps                   | Scalable within single codebase           | Shared runtime, still coupled          | NestJS, Django-Ninja, FastAPI (modular)                | Internal tools, feature-rich SaaS |
| Microservices         | Independently deployed services              | Enterprise apps                | Independently scalable, team autonomy     | Complex infra, inter-service comms     | Express, Fastify / FastAPI, Flask, Django REST         | E-commerce, marketplaces          |
| Serverless            | Functions triggered by events (Lambda, etc.) | Low-latency, event-based needs | Pay-per-use, scales automatically         | Cold starts, limited execution time    | Node.js (AWS Lambda, Vercel) / Python (Zappa, Chalice) | Auth APIs, webhooks               |
| Event-Driven          | Services interact via pub/sub messaging      | Real-time and async processing | Loose coupling, scalable                  | Debugging and state tracking is harder | Node.js + NATS/RabbitMQ / Python + Celery/Kafka        | Notifications, analytics          |
| CQRS + Event Sourcing | Split read/write + event stream persistence  | Audit logs, financial systems  | Query/read optimization, replayable state | High complexity, requires infra setup  | NestJS (CQRS) / FastAPI + Kafka, EventStore            | Ledgers, bookings                 |
| Backend For Frontend  | One backend per frontend (web/mobile)        | Apps with tailored UI needs    | Custom responses, less over-fetching      | Code duplication between backends      | Express, Fastify / FastAPI, Flask                      | Separate web and mobile apps      |
| GraphQL-Centric       | Unified GraphQL layer exposing services      | Dashboards, complex UIs        | Flexible client queries                   | Schema design, query complexity        | Apollo Server, Mercurius / Strawberry, Ariadne         | Multi-client APIs                 |
| JAMstack Backend      | Static frontend + backend APIs               | Lightweight marketing sites    | Fast, scalable hosting                    | All logic must go through APIs         | Next.js + API routes / Flask + Netlify Functions       | Landing pages, calculators        |
| Headless CMS-Based    | CMS with extendable logic via custom APIs    | Content-heavy platforms        | Easy content management                   | Flexibility limited by CMS             | Strapi (Node), Keystone / Wagtail, Netlify CMS         | Blogs, eCommerce                  |

---

### Codebase Architectures

| Architecture                       | Description                                     | When to Use                     | Pros                      | Cons                               | Node.js / Python Compatibility                      | Used In                                   |
| ---------------------------------- | ----------------------------------------------- | ------------------------------- | ------------------------- | ---------------------------------- | --------------------------------------------------- | ----------------------------------------- |
| MVC                                | Model, View, Controller layers                  | Small to mid apps               | Easy to grasp             | Not great at scale                 | Express, Sails / Flask, Django                      | Monoliths, CRUD apps                      |
| Layered                            | Separate controller, service, repo layers       | General-purpose apps            | Logical separation        | Layers may become shallow          | Express, NestJS / FastAPI, Django                   | API backends                              |
| Clean Architecture                 | Entities → UseCases → Interfaces → Framework    | Testable, scalable apps         | Highly decoupled          | Verbose, complex setup             | NestJS / FastAPI (manual), CleanPy                  | Large SaaS, microservices                 |
| Hexagonal Architecture             | Core logic via ports/adapters                   | Integration-heavy apps          | Pluggable, isolated logic | Setup complexity                   | Express / Python Hexagonal Boilerplates             | Plugin systems, external API integrations |
| Onion Architecture                 | Centered on domain, outer layers add infra      | Domain-centric logic            | Controlled dependencies   | Verbose for small projects         | NestJS / FastAPI, Django (manual)                   | Enterprise apps                           |
| CQRS                               | Separate command and query logic                | Write-heavy, real-time flows    | Efficient and scalable    | Needs coordination logic           | NestJS CQRS / Python (custom FastAPI + command bus) | Financial or booking systems              |
| DDD (Domain Driven)                | Structure based on domain logic                 | Complex business logic          | Matches business language | Needs domain expertise             | NestJS / Python (Dependency Injector, DDD libs)     | Multi-entity SaaS                         |
| Event Sourcing                     | Store state as events                           | Audit and history-focused apps  | Full traceability         | Harder consistency management      | Kafka/EventStore with Node or Python                | CRMs, financial systems                   |
| Plugin-Based                       | Core + dynamic plugins for features             | Dev tools, extensible backends  | Easy to add features      | Plugin lifecycle management needed | Strapi / Django CMS, Flask plugins                  | CMS, IDEs                                 |
| Functional Core / Imperative Shell | Logic in pure functions, I/O handled separately | Test-driven, FP-influenced apps | High testability          | Harder for imperative logic        | Any Node.js or Python app                           | Functional-style backends                 |

---

### Codebase Architectures with Real Folder Structures

#### 1. MVC (Model-View-Controller)

```
/app
├── controllers/
│   └── user_controller.*
├── models/
│   └── user_model.*
├── views/
│   └── user_view.*       # HTML, templates, etc.
├── routes/
│   └── user_routes.*
└── app.*
```

---

#### 2. Layered Architecture

```
/app
├── controllers/
│   └── user_controller.*
├── services/
│   └── user_service.*
├── repositories/
│   └── user_repository.*
├── models/
│   └── user_model.*
└── main.*
```

---

#### 3. Clean Architecture

```
/app
├── domain/
│   ├── entities/
│   └── value_objects/
├── use_cases/
│   └── create_user.*
├── interfaces/
│   ├── controllers/
│   └── routes/
├── infrastructure/
│   ├── database/
│   └── external_services/
└── main.*
```

---

#### 4. Hexagonal Architecture (Ports and Adapters)

```
/app
├── core/
│   └── domain_logic.*
├── ports/
│   └── user_repository_port.*
├── adapters/
│   ├── database/
│   └── external_api/
├── interfaces/
│   └── http/
└── main.*
```

---

#### 5. Onion Architecture

```
/app
├── domain/
│   └── entities/
├── application/
│   └── use_cases/
├── infrastructure/
│   ├── database/
│   └── messaging/
├── interfaces/
│   └── controllers/
└── main.*
```

---

#### 6. CQRS + Event Sourcing

```
/app
├── commands/
│   └── handlers/
├── queries/
│   └── handlers/
├── events/
│   ├── emitters/
│   └── listeners/
├── projections/
├── infrastructure/
│   └── event_store/
└── main.*
```

---

#### 7. DDD (Domain Driven Design)

```
/app
├── users/
│   ├── domain/
│   ├── application/
│   ├── infrastructure/
│   └── interfaces/
├── auth/
│   ├── domain/
│   ├── application/
│   └── interfaces/
└── main.*
```

---

#### 8. Plugin-Based Architecture

```
/app
├── core/
│   └── engine.*
├── plugins/
│   ├── analytics/
│   ├── auth/
│   └── payments/
├── interfaces/
│   └── api/
└── main.*
```

---

#### 9. Functional Core / Imperative Shell

```
/app
├── core/
│   └── pure_logic/
├── shell/
│   └── http_handlers/
└── main.*
```

---

## Frontend Architectural Patterns

### System Architectures

| Architecture                          | Description                                           | When to Use                                        | Pros                               | Cons                                        | Frameworks / Tools                                | Use Cases                                |
| ------------------------------------- | ----------------------------------------------------- | -------------------------------------------------- | ---------------------------------- | ------------------------------------------- | ------------------------------------------------- | ---------------------------------------- |
| SPA (Single Page App)                 | Full app loaded once, routes handled client-side      | Rich interactive apps                              | Fast navigation, smooth UX         | Poor SEO, heavy initial load                | React, Vue, Angular, Svelte                       | Dashboards, admin panels, social feeds   |
| MPA (Multi Page App)                  | Server renders a new HTML page per route              | Content-heavy, SEO-critical sites                  | Great SEO, simpler mental model    | Full page reload on each navigation         | Next.js (pages), Nuxt, Astro                      | Blogs, marketing sites, news portals     |
| SSR (Server-Side Rendering)           | HTML rendered on server per request                   | Dynamic, SEO-sensitive apps                        | Fast first paint, SEO-friendly     | Higher server load, more infra              | Next.js, Nuxt, SvelteKit, Remix                   | E-commerce, news, product pages          |
| SSG (Static Site Generation)          | HTML pre-built at build time                          | Static or infrequently updated content             | Blazing fast, CDN-cacheable        | Not suited for highly dynamic data          | Next.js, Astro, Gatsby, Eleventy                  | Docs, landing pages, portfolios          |
| ISR (Incremental Static Regeneration) | SSG with background page regeneration                 | Mostly static with periodic updates                | Combines SSG speed with fresh data | Complexity in cache invalidation            | Next.js, Nuxt                                     | Product listings, news with TTL          |
| JAMstack                              | Static frontend + decoupled APIs + CDN delivery       | Scalable, low-maintenance sites                    | Fast, secure, low hosting cost     | Dynamic features need API workarounds       | Gatsby, Astro, Eleventy + Netlify/Vercel          | Marketing sites, documentation           |
| Micro Frontends                       | Frontend split into independently deployable UI apps  | Large teams, independent deployments               | Team autonomy, independent deploys | Integration complexity, style inconsistency | Module Federation (Webpack), single-spa, Nx       | Enterprise portals, large e-commerce     |
| Islands Architecture                  | Static HTML with isolated interactive islands         | Mostly static content with selective interactivity | Minimal JS shipped to client       | Less suitable for highly interactive UIs    | Astro, Marko, Fresh (Deno)                        | Blogs with interactive widgets, docs     |
| Edge Rendering                        | HTML rendered at CDN edge nodes near the user         | Low-latency, global audiences                      | Ultra-fast TTFB, geo-aware logic   | Limited compute at edge, cold starts        | Next.js (Edge Runtime), Remix, Cloudflare Workers | Personalized landing pages, A/B testing  |
| PWA (Progressive Web App)             | Web app with offline support and native-like features | Mobile-first, offline-capable apps                 | Works offline, installable         | Limited native API access vs native apps    | React + Workbox, Vue PWA plugin                   | Field tools, ride-sharing, delivery apps |

---

### Codebase Architectures

| Architecture                           | Description                                                               | When to Use                                    | Pros                                | Cons                                        | Framework Compatibility                 | Used In                                     |
| -------------------------------------- | ------------------------------------------------------------------------- | ---------------------------------------------- | ----------------------------------- | ------------------------------------------- | --------------------------------------- | ------------------------------------------- |
| Component-Based                        | UI broken into reusable, self-contained components                        | All modern frontends                           | Reusable, composable, testable      | Component explosion at scale                | React, Vue, Svelte, Angular             | Every modern UI                             |
| Feature-Sliced Design (FSD)            | Code split by feature with strict layer rules                             | Mid to large apps needing scalability          | Strong boundaries, team-scalable    | Learning curve, strict conventions          | React, Vue (any framework)              | SaaS apps, enterprise dashboards            |
| Atomic Design                          | Components organized as atoms → molecules → organisms → templates → pages | Design-system-first apps                       | UI consistency, reusable primitives | Overhead for simple apps                    | React + Storybook, Vue + Storybook      | Design systems, UI libraries                |
| MVC on Frontend                        | Model manages data, View renders UI, Controller handles input             | Angular-style apps                             | Familiar structure for backend devs | Verbose, less idiomatic in React/Vue        | Angular, Backbone.js                    | Legacy SPAs, Angular enterprise apps        |
| Flux / Unidirectional Data Flow        | State flows in one direction: Action → Dispatcher → Store → View          | Complex shared state apps                      | Predictable state, easy debugging   | Boilerplate heavy                           | Redux (React), Vuex, NgRx               | Large SPAs, real-time dashboards            |
| MVVM (Model-View-ViewModel)            | ViewModel binds data reactively to the View                               | Reactive, data-bound UIs                       | Clean separation of UI logic        | Two-way binding can obscure flow            | Vue (natural fit), Angular, Knockout.js | Form-heavy apps, enterprise UIs             |
| Layered (Presentation + Domain + Data) | UI layer sits over domain logic and a data access layer                   | Domain-heavy frontends                         | Mirrors backend clean architecture  | More files and abstraction                  | React, Vue (manual setup)               | Complex SPAs, fintech dashboards            |
| Micro Frontend Architecture            | Independent mini-apps composed into one shell                             | Large orgs, separate team ownership            | Independent deploy and tech stack   | Shared state and style coordination is hard | Module Federation, single-spa           | Enterprise portals, multi-product platforms |
| Monorepo-Based                         | Multiple apps/packages in one repo with shared libs                       | Shared design systems or multi-app orgs        | Code sharing, unified tooling       | Tooling complexity, slower CI               | Nx, Turborepo, pnpm workspaces          | Multi-product SaaS, design systems          |
| Server Components (RSC)                | Components rendered server-side, zero JS sent to client                   | Data-heavy components needing no interactivity | Smaller bundles, server data access | Mental model shift, limited interactivity   | Next.js (App Router), React 19+         | Dashboards, data-fetching-heavy pages       |

---

### Codebase Architectures with Real Folder Structures

#### 1. Component-Based Architecture

```
/src
├── components/
│   ├── Button/
│   │   ├── Button.tsx
│   │   ├── Button.test.tsx
│   │   └── Button.module.css
│   └── Card/
├── pages/
│   └── Home.tsx
├── hooks/
│   └── useAuth.ts
└── main.tsx
```

---

#### 2. Feature-Sliced Design (FSD)

```
/src
├── app/               # App-wide setup (providers, router, store)
├── pages/             # Route-level pages composed from features
├── features/          # User interactions: auth, cart, search
│   └── auth/
│       ├── ui/
│       ├── model/
│       └── api/
├── entities/          # Business objects: User, Product, Order
│   └── user/
├── shared/            # Reusable utils, UI kit, configs
│   ├── ui/
│   └── lib/
└── main.tsx
```

---

#### 3. Atomic Design

```
/src
├── components/
│   ├── atoms/
│   │   ├── Button/
│   │   └── Input/
│   ├── molecules/
│   │   └── SearchBar/
│   ├── organisms/
│   │   └── Navbar/
│   ├── templates/
│   │   └── DashboardLayout/
│   └── pages/
│       └── HomePage/
└── main.tsx
```

---

#### 4. Flux / Unidirectional Data Flow (Redux-style)

```
/src
├── store/
│   ├── actions/
│   │   └── userActions.ts
│   ├── reducers/
│   │   └── userReducer.ts
│   └── index.ts
├── components/
│   └── UserProfile/
├── pages/
│   └── Dashboard.tsx
└── main.tsx
```

---

#### 5. MVVM

```
/src
├── models/
│   └── UserModel.ts        # Raw data shape / API response type
├── viewmodels/
│   └── UserViewModel.ts    # State, computed props, actions
├── views/
│   └── UserView.tsx        # Pure rendering component
├── services/
│   └── userService.ts
└── main.tsx
```

---

#### 6. Layered Architecture (Presentation + Domain + Data)

```
/src
├── presentation/
│   ├── components/
│   └── pages/
├── domain/
│   ├── models/
│   └── use-cases/
├── data/
│   ├── repositories/
│   └── api/
└── main.tsx
```

---

#### 7. Micro Frontend (Shell + Remotes)

```
# Shell App
/shell
├── src/
│   ├── App.tsx
│   └── bootstrap.tsx
└── webpack.config.js   # Module Federation host

# Remote App (e.g. Checkout)
/checkout-mfe
├── src/
│   ├── CheckoutApp.tsx
│   └── bootstrap.tsx
└── webpack.config.js   # Module Federation remote
```

---

#### 8. Monorepo-Based (Nx / Turborepo)

```
/apps
├── web/                # Main web app
├── mobile/             # React Native or Expo
└── admin/              # Admin dashboard

/packages
├── ui/                 # Shared design system
├── utils/              # Shared utilities
├── config/             # Shared ESLint, TS config
└── api-client/         # Shared typed API client

turbo.json / nx.json
```

---

#### 9. Server Components Architecture (Next.js App Router)

```
/app
├── layout.tsx              # Root server layout
├── page.tsx                # Server component (no "use client")
├── dashboard/
│   ├── page.tsx            # Server-rendered dashboard
│   └── StatsChart.tsx      # "use client" — interactive island
├── components/
│   ├── ServerCard.tsx      # Fetches data directly from DB
│   └── ClientButton.tsx    # "use client" — handles clicks
└── lib/
    └── db.ts               # Direct DB access (server only)
```

---

## Architecture Decision Tree

Not sure where to start? Answer these five questions and follow the path.

```
Q1. Are you building from scratch or working on an existing app?
│
├── Existing app → skip to Migration Paths
│
└── From scratch → Q2
        │
        ├── Is it a content or marketing site? (blog, docs, landing page)
        │       └── YES → SSG (Astro/Next.js) + JAMstack Backend
        │
        └── NO → Q3
                │
                ├── Does it need SEO and dynamic data? (e-commerce, news)
                │       └── YES → SSR (Next.js/Nuxt) + Modular Monolith or Microservices
                │
                └── NO → Q4
                        │
                        ├── Is it a dashboard, internal tool, or admin panel?
                        │       └── YES → SPA + Layered Backend (MVC or Layered Arch)
                        │
                        └── NO → Q5
                                │
                                ├── Are multiple teams working independently?
                                │       └── YES → Micro Frontends + Microservices + DDD
                                │
                                └── NO → Modular Monolith + Component-Based SPA
```

---

## Real-World Company Examples

How companies you know actually use these patterns in production.

### Backend

| Company  | Architecture Used                   | Why                                                      |
| -------- | ----------------------------------- | -------------------------------------------------------- |
| Netflix  | Microservices + Event-Driven        | 500+ services, independent scaling per feature           |
| Uber     | Microservices + CQRS                | Separate read (trip tracking) from write (booking) paths |
| Airbnb   | Modular Monolith → Microservices    | Started with a monolith, migrated as scale demanded      |
| Stripe   | Layered + Event Sourcing            | Every payment event is stored and replayable for audits  |
| GitHub   | Monolith (Rails) → Modular Monolith | Kept the monolith long-term with disciplined modularity  |
| Discord  | Event-Driven + Serverless           | Millions of real-time messages routed via pub/sub        |
| Shopify  | Modular Monolith (Rails)            | One of the most successful monoliths at scale            |
| LinkedIn | CQRS + Event-Driven                 | Feed generation decoupled from user write actions        |

### Frontend

| Company      | Architecture Used          | Why                                                             |
| ------------ | -------------------------- | --------------------------------------------------------------- |
| Spotify      | Micro Frontends            | Teams own their feature slice end-to-end (player, search, etc.) |
| Amazon       | Micro Frontends + SSR      | Each product section is independently deployed                  |
| Twitter / X  | SPA → SSR (Next.js)        | Migrated to SSR for faster initial loads and SEO                |
| Airbnb       | SSR + Atomic Design        | Consistent design system across web and native                  |
| Notion       | SPA + Component-Based      | Rich collaborative editor demands full client control           |
| Vercel       | SSG + ISR                  | Mostly static docs with periodic content regeneration           |
| IKEA         | JAMstack + Headless CMS    | Content managed separately, storefront deployed to CDN          |
| The Guardian | SSR + Islands Architecture | Fast news pages with small interactive components               |

---

## Anti-Patterns — What Not to Do

The most common mistakes developers make with each architecture. Knowing these upfront saves months of pain.

### Backend

| Architecture       | Common Mistake                                     | What Happens                                                | Do This Instead                                                                      |
| ------------------ | -------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| Monolith           | Never modularizing as the app grows                | Everything becomes a tangled ball of logic                  | Apply module boundaries early — treat it like a Modular Monolith                     |
| Microservices      | Starting with microservices on day one             | Premature complexity before you know your domain boundaries | Start with a monolith, extract services only when the pain is real                   |
| Serverless         | Putting stateful logic in functions                | Inconsistent state, race conditions                         | Use serverless only for stateless, short-lived tasks                                 |
| Event-Driven       | No dead letter queue or retry logic                | Silent failures — events vanish, no one notices             | Always add error queues and event logging from day one                               |
| CQRS               | Applying it everywhere                             | Over-engineered CRUD with pointless complexity              | Use CQRS only when read and write loads are genuinely different                      |
| DDD                | Letting the domain model leak into the HTTP layer  | Tight coupling, messy controllers                           | Strictly keep domain logic inside domain boundaries                                  |
| Clean Architecture | Creating layers without enforcing dependency rules | Just a renamed Layered Architecture with no real benefit    | Use interfaces and enforce the rule: outer layers depend on inner, never the reverse |

### Frontend

| Architecture          | Common Mistake                           | What Happens                                                | Do This Instead                                                               |
| --------------------- | ---------------------------------------- | ----------------------------------------------------------- | ----------------------------------------------------------------------------- |
| SPA                   | Putting everything in a global store     | State becomes a shared nightmare everyone touches           | Co-locate state near the component that owns it                               |
| SSR                   | Fetching data client-side on an SSR page | Defeats the whole purpose — users still see spinners        | Move data fetching to the server (getServerSideProps, loaders)                |
| Micro Frontends       | Sharing too much state between apps      | Tight coupling defeats the point of independent deployments | Use events or a shared contract (URL params, custom events)                   |
| Atomic Design         | Treating every tiny thing as an atom     | 200 atoms in Storybook that no one can find or reuse        | Only extract atoms when they are genuinely reused across three or more places |
| Flux / Redux          | Using Redux for every piece of UI state  | Massive boilerplate for toggling a dropdown                 | Use local state for UI, Redux only for shared cross-feature state             |
| Component-Based       | Deeply nested prop drilling              | Components become brittle and impossible to refactor        | Use Context API, Zustand, or composition to break the chain                   |
| Feature-Sliced Design | Importing directly across feature slices | Circular dependencies, tight cross-feature coupling         | Features should only communicate through shared entities or events            |

---

## Migration Paths

Already have an app and need to evolve it? These are the most proven step-by-step paths.

### Monolith → Modular Monolith

```
Step 1: Identify natural domain boundaries (users, billing, products)
Step 2: Move files into domain folders — /users, /billing, /products
Step 3: Ban cross-domain direct imports — enforce via ESLint rules or module boundaries
Step 4: Each domain gets its own service layer and repository
Step 5: Add internal interfaces between domains (dependency inversion)

Timeline: 2–8 weeks for a mid-size codebase
Risk: Low
```

### Modular Monolith → Microservices

```
Step 1: Identify the highest-pain domain (most traffic or most team conflict)
Step 2: Extract its database tables to a separate schema
Step 3: Replace in-process calls with HTTP or message queue calls
Step 4: Deploy the extracted service independently
Step 5: Repeat domain by domain — never do a big bang rewrite

Timeline: 3–12 months per service extraction
Risk: Medium — do it one service at a time
```

### SPA → SSR

```
Step 1: Migrate to Next.js (pages router first — easier migration path)
Step 2: Move data fetching into getServerSideProps or getStaticProps
Step 3: Audit which pages need SSR (SEO/dynamic) vs SSG (static)
Step 4: Move client-only components behind dynamic imports
Step 5: Validate hydration — fix any client/server mismatch errors

Timeline: 2–6 weeks depending on app size
Risk: Medium — hydration bugs are subtle
```

### Component-Based → Feature-Sliced Design

```
Step 1: Create the /features, /entities, /shared top-level folders
Step 2: Move shared UI components into /shared/ui
Step 3: Group by feature — move auth components, hooks, and API calls into /features/auth
Step 4: Enforce import rules — features cannot import from other features
Step 5: Gradually migrate remaining components

Timeline: 1–4 weeks
Risk: Low — incremental by nature
```

---

## Team Size Guide

The right architecture isn't just about the app. It's about the people building it.

| Team Size        | Backend Sweet Spot                      | Frontend Sweet Spot             | Avoid                                        |
| ---------------- | --------------------------------------- | ------------------------------- | -------------------------------------------- |
| Solo Developer   | Monolith + MVC or Layered               | SPA + Component-Based           | Microservices, Micro Frontends               |
| 2–5 Developers   | Modular Monolith + Layered              | SPA + FSD or Atomic Design      | Full CQRS, Event Sourcing                    |
| 5–15 Developers  | Modular Monolith or light Microservices | FSD + Monorepo                  | Micro Frontends (too early)                  |
| 15–50 Developers | Microservices + DDD                     | Micro Frontends + Monorepo      | Pure Monolith (will eventually collapse)     |
| 50+ Developers   | Microservices + Event-Driven + CQRS     | Micro Frontends + Design System | Shared global state, tightly coupled deploys |

### Rules to keep in mind

**Don't over-architect early.** A solo developer building microservices is burning time on infrastructure instead of the product itself.

**Feel the pain first.** If the monolith isn't slowing you down yet, it's not time to break it apart. Migrate when the problem is real, not imagined.

**Conway's Law is real.** Your architecture will naturally mirror your team structure. Think about how you organise people and how you organise code at the same time.

**One service per team, not per feature.** Microservices and Micro Frontends work best when an entire team owns an entire slice end to end — not when every small feature has its own service.

---

## Choosing the Right Architecture

| Concern                      | Recommended Backend            | Recommended Frontend                 |
| ---------------------------- | ------------------------------ | ------------------------------------ |
| MVP / Prototype              | Monolith + MVC                 | SPA + Component-Based                |
| SEO-Critical Site            | JAMstack / SSR Backend         | SSG / SSR (Next.js, Nuxt)            |
| Large Team / Enterprise      | Microservices + DDD            | Micro Frontends / FSD                |
| Real-Time Features           | Event-Driven + CQRS            | SPA + Flux / Unidirectional State    |
| Content-Heavy Platform       | Headless CMS                   | JAMstack + Islands Architecture      |
| Design System Focus          | Modular Monolith               | Atomic Design + Monorepo             |
| Audit / Financial Data       | CQRS + Event Sourcing          | Layered Frontend + Server Components |
| High-Performance Global App  | Serverless + BFF               | Edge Rendering + SSG + ISR           |
| Offline / Mobile-First       | REST/GraphQL Backend           | PWA + Component-Based                |
| Testable / Maintainable Core | Clean / Hexagonal Architecture | Layered + MVVM / FSD                 |
