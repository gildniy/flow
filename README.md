# FLOW Framework

**Focused. Layered. Owned. Waypointed.**

A universal, agent-agnostic framework for AI-assisted development that scales from tiny scripts to enterprise platforms.

---

## What is FLOW?

FLOW is a file-based development framework that works with **any** AI coding assistant (Claude, GPT, Copilot, local models) through simple markdown files. No CLI tools, no vendor lock-in, no magic commands.

**The core idea:** Give your AI a clear mental model through structured files, and it will build better software.

---

## Philosophy: The Four Principles

### 🎯 Focused
**One concern per layer.** Never mix intent, planning, execution, and memory in the same file.

### 📚 Layered
**Clear separation of concerns.** Each file type has a specific role in the development flow.

### 👤 Owned
**You control everything.** No black boxes, no automation you can't see, no hidden state.

### 🗺️ Waypointed
**Always know where you are.** Clear checkpoints, dependency tracking, and progress visibility.

---

## File Structure

```
project/
├── FLOW/
│   ├── INTENT.md           # What and why
│   ├── CONTEXT.md          # Constraints and preferences
│   ├── LAYERS.md           # System architecture
│   ├── GRAPH.md            # Dependency map
│   ├── WAYPOINTS.md        # Milestones and checkpoints
│   ├── MEMORY.md           # Living knowledge base
│   └── WORK/
│       ├── 01-setup.md
│       ├── 02-auth.md
│       └── 03-api.md
└── src/                    # Your actual code
```

---

## The 7 Files

### 1. `INTENT.md` - Vision & Goals

**Purpose:** Define what you're building and why.

**Template:**
```markdown
# Project Intent

## Problem
What problem does this solve?

## Solution
What are you building?

## Users
Who is this for?

## Success
What does success look like?

## Constraints
What are the hard limits? (time, budget, tech, etc.)
```

**Example:**
```markdown
# Project Intent

## Problem
Developers waste hours context-switching between tasks and lose track of project state.

## Solution
A universal framework that structures AI-assisted development through simple markdown files.

## Users
Solo developers and small teams using AI coding assistants.

## Success
- Developers can pause/resume work without losing context
- AI understands project structure immediately
- Works with any AI tool

## Constraints
- Must work without any CLI installation
- Must be readable by humans AND AI
- Must scale from solo projects to teams
```

---

### 2. `CONTEXT.md` - Constraints & Preferences

**Purpose:** Define technical preferences, constraints, and resources that apply to ALL work.

**Template:**
```markdown
# Project Context

## Stack
- Language: [e.g., TypeScript, Python]
- Framework: [e.g., React, Next.js, FastAPI]
- Database: [e.g., PostgreSQL, MongoDB]
- Deployment: [e.g., Vercel, AWS, Docker]

## Required Packages
List packages that MUST be used:
- Authentication: [e.g., next-auth, passport]
- State: [e.g., zustand, redux]
- UI: [e.g., shadcn/ui, tailwind]
- Testing: [e.g., vitest, jest]

## Forbidden Packages
List packages to NEVER use:
- [package-name]: [reason why]

## Code Conventions
- [Convention 1]
- [Convention 2]

## File Structure Rules
- [Rule 1]
- [Rule 2]

## Performance Requirements
- [Requirement 1]
- [Requirement 2]

## Security Requirements
- [Requirement 1]
- [Requirement 2]
```

**Example:**
```markdown
# Project Context

## Stack
- Language: TypeScript
- Framework: Next.js 14 (App Router)
- Database: PostgreSQL with Prisma
- Deployment: Vercel

## Required Packages
- Authentication: next-auth v5
- State: zustand (NOT Redux - simpler for this scale)
- UI: shadcn/ui + tailwind
- Forms: react-hook-form + zod
- Testing: vitest + testing-library

## Forbidden Packages
- class-validator: Use zod instead (already in stack)
- axios: Use native fetch (built into Next.js)
- moment: Use date-fns (smaller bundle)

## Code Conventions
- Use server components by default, client only when needed
- All API routes in app/api/
- All components in components/ (flat structure until >20 components)
- Prefer composition over inheritance
- Always use TypeScript strict mode

## File Structure Rules
- One component per file
- Colocate tests with components (*.test.tsx)
- Group related utilities in /lib

## Performance Requirements
- Lighthouse score >90
- First Contentful Paint <1.5s
- Time to Interactive <3s

## Security Requirements
- All inputs validated with zod
- SQL injection prevention via Prisma
- XSS prevention via React's default escaping
- CSRF protection via next-auth
```

**Key Idea:** This file ensures your AI never suggests the wrong tools or violates your constraints. It's your "guard rails."

---

### 3. `LAYERS.md` - System Architecture

**Purpose:** Break the system into logical components and their relationships.

**Template:**
```markdown
# System Layers

## [Layer Name]
**Purpose:** [What this layer does]

**Components:**
- [Component 1]: [Brief description]
- [Component 2]: [Brief description]

**Depends On:** [Other layers this needs]

**Used By:** [Other layers that use this]

---

## [Next Layer Name]
...
```

**Example:**
```markdown
# System Layers

## Data Layer
**Purpose:** Handle all data persistence and retrieval

**Components:**
- Prisma Schema: Database models and relations
- Repositories: Data access patterns
- Migrations: Database version control

**Depends On:** None (bottom layer)

**Used By:** Service Layer

---

## Service Layer
**Purpose:** Business logic and orchestration

**Components:**
- UserService: User management logic
- AuthService: Authentication/authorization
- PostService: Content management

**Depends On:** Data Layer

**Used By:** API Layer, Server Components

---

## API Layer
**Purpose:** HTTP endpoints for client interactions

**Components:**
- REST Routes: /api/* endpoints
- Validation: Request/response schemas
- Error Handling: Standardized error responses

**Depends On:** Service Layer

**Used By:** Client Components, External Clients

---

## UI Layer
**Purpose:** User interface and interactions

**Components:**
- Server Components: Static/cached UI
- Client Components: Interactive elements
- Layouts: Page structure
- Forms: User input handling

**Depends On:** API Layer (for client components), Service Layer (for server components)

**Used By:** End Users
```

---

### 4. `GRAPH.md` - Dependency Map

**Purpose:** Define waypoint dependencies to prevent build failures and enable parallel work.

**Format:**
```markdown
# Dependency Graph

## Format
[waypoint-id] -> [depends-on-waypoint-id, ...]

## Graph
01 -> []              # No dependencies
02 -> [01]            # Requires 01 to be complete
03 -> [01]            # Requires 01 (can be parallel with 02)
04 -> [02, 03]        # Requires both 02 AND 03
05 -> [04]            # Linear dependency
06 -> [03]            # Only depends on 03, not 04 or 05

## Parallel Groups
Group A: [02, 03]     # Can be done in parallel
Group B: [06]         # Can be done while Group A or after 03

## Critical Path
01 -> 02 -> 04 -> 05  # Longest dependency chain
```

**Example:**
```markdown
# Dependency Graph

## Format
[waypoint-id] -> [depends-on-waypoint-id, ...]

## Graph
01 -> []              # Project setup (nothing depends on)
02 -> [01]            # Database schema (needs project setup)
03 -> [01]            # Auth UI components (needs project setup)
04 -> [02]            # Database migrations (needs schema)
05 -> [02, 03]        # Auth API (needs schema + UI components)
06 -> [05]            # Protected routes (needs auth API)
07 -> [02]            # Posts schema (needs initial DB)
08 -> [07]            # Posts API (needs posts schema)
09 -> [06, 08]        # Posts UI (needs auth + posts API)

## Parallel Groups
Group A: [02, 03]     # Schema and UI can be built together
Group B: [07]         # Posts schema can be built during auth work
Group C: [04, 08]     # Migrations can run in parallel

## Critical Path
01 -> 02 -> 05 -> 06 -> 09

## Notes
- Waypoints 07-09 (posts feature) completely independent of auth after 01-02
- UI work (03, 09) can be done by different AI sessions in parallel with API work
```

**Key Benefits:**
- **AI knows what it can build:** No attempting waypoint 5 before 2 is done
- **Enables parallelization:** You can run multiple AI sessions on independent waypoints
- **Prevents failures:** No "missing dependency" errors mid-implementation
- **Shows critical path:** Focus on blockers first

---

### 5. `WAYPOINTS.md` - Milestones & Checkpoints

**Purpose:** Break work into verifiable, atomic units with clear dependencies.

**Template:**
```markdown
# Waypoints

## WP-[ID]: [Name]
**Goal:** [What success looks like]

**Deliverables:**
- [ ] [Specific output 1]
- [ ] [Specific output 2]

**Verification:**
- [ ] [How to verify it works]

**Depends On:** [WP-XX, WP-YY] (or [] for none)

**Estimated Complexity:** Low | Medium | High

**Status:** Not Started | In Progress | Blocked | Done

---

## WP-[NEXT-ID]: [Name]
...
```

**Example:**
```markdown
# Waypoints

## WP-01: Project Setup
**Goal:** Initialize Next.js project with all required tooling

**Deliverables:**
- [ ] Next.js 14 project initialized
- [ ] Prisma configured with PostgreSQL
- [ ] Tailwind + shadcn/ui installed
- [ ] ESLint + Prettier configured
- [ ] Git repository initialized

**Verification:**
- [ ] `npm run dev` starts without errors
- [ ] `npx prisma studio` opens
- [ ] Tailwind classes work in a test component

**Depends On:** []

**Estimated Complexity:** Low

**Status:** Done

---

## WP-02: Database Schema
**Goal:** Define all database models and relationships

**Deliverables:**
- [ ] User model (id, email, name, password)
- [ ] Session model (for next-auth)
- [ ] Post model (id, title, content, authorId, createdAt)
- [ ] Relations defined (User -> Posts)

**Verification:**
- [ ] `npx prisma validate` passes
- [ ] Migration runs successfully
- [ ] Prisma Studio shows all tables

**Depends On:** [WP-01]

**Estimated Complexity:** Low

**Status:** Done

---

## WP-03: Auth UI Components
**Goal:** Build reusable auth form components

**Deliverables:**
- [ ] LoginForm component
- [ ] SignupForm component
- [ ] Form validation with zod
- [ ] Loading and error states

**Verification:**
- [ ] Forms render without errors
- [ ] Validation shows appropriate errors
- [ ] TypeScript types are correct

**Depends On:** [WP-01]

**Estimated Complexity:** Medium

**Status:** In Progress

---

## WP-04: Database Migrations
**Goal:** Apply schema to database

**Deliverables:**
- [ ] Initial migration created
- [ ] Migration applied to dev DB
- [ ] Seed script for test data

**Verification:**
- [ ] Database has all tables
- [ ] Seed data loads successfully

**Depends On:** [WP-02]

**Estimated Complexity:** Low

**Status:** Not Started

---

## WP-05: Auth API
**Goal:** Implement authentication endpoints

**Deliverables:**
- [ ] next-auth configured
- [ ] Login endpoint
- [ ] Signup endpoint
- [ ] Session management

**Verification:**
- [ ] Can create account
- [ ] Can log in
- [ ] Session persists across requests
- [ ] Protected API routes work

**Depends On:** [WP-02, WP-03]

**Estimated Complexity:** High

**Status:** Not Started
```

**Key Rules:**
- One waypoint = one coherent feature/task
- All deliverables must be verifiable
- Keep complexity estimation realistic
- Update status as you progress
- Reference GRAPH.md for dependencies

---

### 6. `MEMORY.md` - Living Knowledge Base

**Purpose:** Capture decisions, learnings, and patterns as they emerge. This is the AI's long-term memory.

**Template:**
```markdown
# Project Memory

## Decisions Made

### [Date] - [Decision Title]
**Context:** [Why did this come up?]

**Decision:** [What was decided?]

**Rationale:** [Why this choice?]

**Impact:** [What changed as a result?]

---

## Patterns Discovered

### [Pattern Name]
**When:** [When to use this pattern]

**How:** [Implementation approach]

**Example:** [Code or file reference]

---

## Problems & Solutions

### [Problem Description]
**Symptoms:** [What went wrong]

**Root Cause:** [Why it happened]

**Solution:** [How it was fixed]

**Prevention:** [How to avoid in future]

---

## Gotchas

### [Gotcha Title]
**Issue:** [What's the trap?]

**Why:** [Why does this happen?]

**Solution:** [How to handle it?]

---

## Resources

### [Resource Name]
**URL:** [Link]

**Purpose:** [When is this useful?]

**Notes:** [Key takeaways]
```

**Example:**
```markdown
# Project Memory

## Decisions Made

### 2024-01-15 - Use Zustand over Redux
**Context:** Needed global state management for user session and UI state

**Decision:** Use Zustand instead of Redux

**Rationale:** 
- Simpler API (no boilerplate)
- Better TypeScript support
- Smaller bundle size (3kb vs 15kb)
- Project complexity doesn't warrant Redux

**Impact:** 
- Faster development
- Less code to maintain
- Team can learn state management in <1 hour

---

### 2024-01-16 - Server Components by Default
**Context:** Next.js 14 App Router supports server components

**Decision:** Default to server components, use 'use client' only when necessary

**Rationale:**
- Better performance (less JavaScript to client)
- Automatic code splitting
- Direct database access in components
- Better SEO

**Impact:**
- Need to be conscious of interactivity boundaries
- Some components split into server + client pairs
- Overall bundle size reduced by 40%

---

## Patterns Discovered

### Server + Client Component Pairs
**When:** Need server data with client interactivity

**How:**
1. Create server component for data fetching
2. Create client component for interactivity
3. Pass data as props from server to client

**Example:**
```tsx
// app/posts/page.tsx (Server Component)
import { PostList } from './PostList'
import { getPosts } from '@/lib/posts'

export default async function PostsPage() {
  const posts = await getPosts()
  return <PostList posts={posts} />
}

// app/posts/PostList.tsx (Client Component)
'use client'
import { useState } from 'react'

export function PostList({ posts }) {
  const [filter, setFilter] = useState('all')
  // ... interactive logic
}
```

---

### Form Validation Pattern
**When:** Any form that accepts user input

**How:**
1. Define zod schema
2. Use react-hook-form with zodResolver
3. Extract reusable field components

**Example:**
```tsx
const loginSchema = z.object({
  email: z.string().email(),
  password: z.string().min(8)
})

const form = useForm({
  resolver: zodResolver(loginSchema)
})
```

---

## Problems & Solutions

### Prisma Client in Development
**Symptoms:** 
- Multiple Prisma Client instances created
- Hot reload causes database connection errors

**Root Cause:** 
Next.js hot reload reinitializes modules, creating new Prisma instances

**Solution:**
```typescript
// lib/prisma.ts
import { PrismaClient } from '@prisma/client'

const globalForPrisma = global as unknown as { prisma: PrismaClient }

export const prisma = globalForPrisma.prisma || new PrismaClient()

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma
```

**Prevention:** 
Always use singleton pattern for database clients in Next.js

---

### next-auth Session Type Errors
**Symptoms:**
TypeScript errors when accessing `session.user.id`

**Root Cause:**
Default next-auth types don't include custom user fields

**Solution:**
Create `types/next-auth.d.ts`:
```typescript
import NextAuth from "next-auth"

declare module "next-auth" {
  interface Session {
    user: {
      id: string
      email: string
      name: string
    }
  }
}
```

**Prevention:**
Always extend next-auth types when adding custom fields

---

## Gotchas

### Server Actions Need 'use server'
**Issue:** 
Server actions fail silently without 'use server' directive

**Why:**
Next.js can't identify server-only code without explicit directive

**Solution:**
Always add 'use server' at the top of server action files

---

### Prisma Schema Changes Require Migration
**Issue:**
Changing schema.prisma doesn't update database automatically

**Why:**
Prisma requires explicit migrations to modify database

**Solution:**
After schema changes:
1. `npx prisma migrate dev --name description`
2. Commit migration files to git

---

## Resources

### Prisma Best Practices
**URL:** https://www.prisma.io/docs/guides/performance-and-optimization

**Purpose:** Optimizing database queries

**Notes:**
- Always use `select` to limit fields
- Use `include` sparingly (N+1 queries)
- Consider caching for frequent reads

---

### Next.js App Router Patterns
**URL:** https://nextjs.org/docs/app/building-your-application/routing

**Purpose:** Understanding App Router architecture

**Notes:**
- Layouts are shared across routes
- Loading.tsx for automatic loading states
- Error.tsx for error boundaries
```

**Key Benefits:**
- AI learns from past work
- Prevents repeating mistakes
- Documents "why" not just "what"
- Builds team knowledge base

---

### 7. `WORK/*.md` - Execution Plans

**Purpose:** Detailed implementation plans for individual waypoints. One file per waypoint.

**Naming:** `WP-[ID]-[slug].md` (e.g., `WP-05-auth-api.md`)

**Template:**
```markdown
# WP-[ID]: [Waypoint Name]

## Goal
[One sentence: what does success look like?]

## Context
**Why This Matters:** [Business/technical justification]

**Dependencies:**
- [List waypoints that must be complete first]
- Reference GRAPH.md for full dependency tree

**Related Files from CONTEXT.md:**
- Stack: [Relevant stack items]
- Required Packages: [Packages to use]
- Conventions: [Conventions to follow]

## Changes

### Files to Create
- `path/to/file.ext`: [Purpose]

### Files to Modify
- `path/to/file.ext`: [What changes]

### Files to Delete
- `path/to/file.ext`: [Why removing]

## Implementation

### Step 1: [Action]
```language
// Code or commands
```
**Why:** [Justification]

### Step 2: [Action]
```language
// Code or commands
```
**Why:** [Justification]

## Verification

### Unit Tests
- [ ] [Test description]
- [ ] [Test description]

### Integration Tests
- [ ] [Test description]

### Manual Verification
- [ ] [Step-by-step verification]

### Success Criteria
All of these must be true:
- [ ] [Criterion 1]
- [ ] [Criterion 2]

## Rollback Plan
If this waypoint fails:
1. [Rollback step 1]
2. [Rollback step 2]

## Status
- [ ] Not Started
- [ ] In Progress
- [ ] Blocked (reason: _______)
- [ ] Done
- [ ] Verified

## Notes
[Space for implementation notes, discoveries, or deviations from plan]
```

**Example:**
```markdown
# WP-05: Auth API

## Goal
Users can create accounts, log in, and maintain authenticated sessions.

## Context
**Why This Matters:** 
Authentication is the foundation for all user-specific features. Without it, we can't protect resources or personalize experiences.

**Dependencies:**
- WP-02: Database Schema (need User and Session models)
- WP-03: Auth UI Components (need forms to test with)
- Reference GRAPH.md for full dependency tree

**Related Files from CONTEXT.md:**
- Stack: Next.js 14, PostgreSQL, Prisma
- Required Packages: next-auth v5
- Conventions: All API routes in app/api/, use TypeScript strict mode
- Security: All inputs validated with zod, CSRF protection via next-auth

## Changes

### Files to Create
- `app/api/auth/[...nextauth]/route.ts`: next-auth handler
- `lib/auth.ts`: Auth configuration and helpers
- `lib/auth/providers.ts`: Authentication providers
- `lib/auth/callbacks.ts`: Session and JWT callbacks
- `app/api/auth/signup/route.ts`: User registration endpoint

### Files to Modify
- `prisma/schema.prisma`: Add Session model for next-auth
- `.env.local`: Add NEXTAUTH_SECRET and NEXTAUTH_URL

### Files to Delete
None

## Implementation

### Step 1: Add Session Model to Prisma
```prisma
// prisma/schema.prisma
model Session {
  id           String   @id @default(cuid())
  sessionToken String   @unique
  userId       String
  expires      DateTime
  user         User     @relation(fields: [userId], references: [id], onDelete: Cascade)
}

model User {
  id       String    @id @default(cuid())
  email    String    @unique
  name     String?
  password String
  posts    Post[]
  sessions Session[]
}
```
**Why:** next-auth requires session storage for persistent login

### Step 2: Run Migration
```bash
npx prisma migrate dev --name add-session-model
```
**Why:** Apply schema changes to database

### Step 3: Configure next-auth
```typescript
// lib/auth.ts
import { NextAuthOptions } from 'next-auth'
import CredentialsProvider from 'next-auth/providers/credentials'
import { prisma } from './prisma'
import { compare } from 'bcrypt'

export const authOptions: NextAuthOptions = {
  providers: [
    CredentialsProvider({
      credentials: {
        email: { label: "Email", type: "email" },
        password: { label: "Password", type: "password" }
      },
      async authorize(credentials) {
        if (!credentials?.email || !credentials?.password) {
          return null
        }

        const user = await prisma.user.findUnique({
          where: { email: credentials.email }
        })

        if (!user) return null

        const isValid = await compare(credentials.password, user.password)
        
        if (!isValid) return null

        return {
          id: user.id,
          email: user.email,
          name: user.name
        }
      }
    })
  ],
  session: {
    strategy: 'jwt'
  },
  pages: {
    signIn: '/login'
  }
}
```
**Why:** Credentials provider allows email/password auth with our database

### Step 4: Create API Route Handler
```typescript
// app/api/auth/[...nextauth]/route.ts
import NextAuth from 'next-auth'
import { authOptions } from '@/lib/auth'

const handler = NextAuth(authOptions)

export { handler as GET, handler as POST }
```
**Why:** Next.js 14 App Router requires explicit GET/POST exports

### Step 5: Create Signup Endpoint
```typescript
// app/api/auth/signup/route.ts
import { NextResponse } from 'next/server'
import { hash } from 'bcrypt'
import { prisma } from '@/lib/prisma'
import { z } from 'zod'

const signupSchema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
  name: z.string().min(2)
})

export async function POST(req: Request) {
  try {
    const body = await req.json()
    const { email, password, name } = signupSchema.parse(body)

    const exists = await prisma.user.findUnique({
      where: { email }
    })

    if (exists) {
      return NextResponse.json(
        { error: 'User already exists' },
        { status: 400 }
      )
    }

    const hashedPassword = await hash(password, 12)

    const user = await prisma.user.create({
      data: {
        email,
        password: hashedPassword,
        name
      }
    })

    return NextResponse.json(
      { user: { id: user.id, email: user.email, name: user.name } },
      { status: 201 }
    )
  } catch (error) {
    if (error instanceof z.ZodError) {
      return NextResponse.json(
        { error: error.errors },
        { status: 400 }
      )
    }
    return NextResponse.json(
      { error: 'Internal server error' },
      { status: 500 }
    )
  }
}
```
**Why:** Separate signup from login, validate inputs with zod, hash passwords

### Step 6: Add Environment Variables
```bash
# .env.local
NEXTAUTH_SECRET=your-secret-here-generate-with-openssl-rand-base64-32
NEXTAUTH_URL=http://localhost:3000
```
**Why:** next-auth requires these for session encryption

## Verification

### Unit Tests
- [ ] signupSchema validates correct inputs
- [ ] signupSchema rejects invalid inputs
- [ ] Password hashing works correctly
- [ ] Duplicate email detection works

### Integration Tests
- [ ] Signup creates user in database
- [ ] Login with correct credentials succeeds
- [ ] Login with wrong password fails
- [ ] Session persists across requests

### Manual Verification
1. [ ] Start dev server: `npm run dev`
2. [ ] Open Prisma Studio: `npx prisma studio`
3. [ ] Use signup form to create account
4. [ ] Verify user appears in database
5. [ ] Login with created account
6. [ ] Verify session cookie is set
7. [ ] Refresh page, verify still logged in
8. [ ] Logout, verify session cleared

### Success Criteria
All of these must be true:
- [ ] Users can create accounts
- [ ] Users can log in with correct credentials
- [ ] Users cannot log in with wrong credentials
- [ ] Sessions persist across page refreshes
- [ ] Passwords are hashed in database (never plaintext)
- [ ] All TypeScript types compile without errors
- [ ] No ESLint warnings

## Rollback Plan
If this waypoint fails:
1. `git reset --hard HEAD~1` (remove commits)
2. `npx prisma migrate reset` (reset database)
3. Review MEMORY.md for issues encountered
4. Update this WORK file with fixes
5. Try again

## Status
- [x] Not Started
- [x] In Progress
- [ ] Blocked (reason: _______)
- [ ] Done
- [ ] Verified

## Notes
- Decided to use bcrypt over argon2 for broader Next.js compatibility
- Session strategy is JWT (not database) for better Vercel deployment
- May need to add email verification in future waypoint
```

---

## How to Use FLOW

### Starting a New Project

1. **Create the structure:**
```bash
mkdir -p FLOW/WORK
cd FLOW
```

2. **Write INTENT.md:**
Talk to your AI: *"Help me create INTENT.md for [your idea]"*

3. **Create CONTEXT.md:**
Talk to your AI: *"Based on INTENT.md, help me create CONTEXT.md with my stack preferences"*

4. **Generate LAYERS.md:**
Prompt: *"Read INTENT.md and CONTEXT.md, then create LAYERS.md that breaks this system into logical components"*

5. **Define WAYPOINTS.md:**
Prompt: *"Based on LAYERS.md, create WAYPOINTS.md with 8-12 key milestones in dependency order"*

6. **Create GRAPH.md:**
Prompt: *"Read WAYPOINTS.md and create GRAPH.md showing all dependencies and parallel work opportunities"*

7. **Build First Waypoint:**
Prompt: *"Create WORK/WP-01-[name].md for the first waypoint with full implementation details"*

8. **Execute:**
Prompt: *"Read WORK/WP-01-[name].md and implement it. Update MEMORY.md with any decisions or learnings."*

9. **Repeat** for each waypoint.

---

### Working on Existing Projects

1. **Create FLOW directory** in your project
2. **Document current state:**
   - Create INTENT.md (describe what exists)
   - Create CONTEXT.md (document current stack)
   - Create LAYERS.md (map existing architecture)
3. **Define new work:**
   - Create WAYPOINTS.md (what you're adding/changing)
   - Create GRAPH.md (dependencies for new work)
4. **Execute as normal**

---

### Prompts for Your AI

**Creating structure:**
```
Read INTENT.md and create LAYERS.md that identifies the main system components, 
their responsibilities, and dependencies. Follow the template in FLOW documentation.
```

**Planning waypoints:**
```
Based on LAYERS.md, create WAYPOINTS.md with 8-12 atomic milestones. Each waypoint 
should be independently verifiable and include clear deliverables. Mark dependencies 
between waypoints.
```

**Creating dependency graph:**
```
Read WAYPOINTS.md and create GRAPH.md showing which waypoints depend on which others. 
Identify opportunities for parallel work. Show the critical path.
```

**Implementing waypoint:**
```
Read WORK/WP-05-auth-api.md and implement it exactly as specified. Follow all 
constraints in CONTEXT.md. Update MEMORY.md with any important decisions or patterns 
discovered during implementation.
```

**Checking dependencies:**
```
Before implementing WP-07, verify that all dependencies listed in GRAPH.md are 
complete. Check WAYPOINTS.md status for each dependency.
```

**Parallel work:**
```
According to GRAPH.md, which waypoints can I work on in parallel right now? 
List waypoints that have no incomplete dependencies.
```

---

## Advanced Usage

### Large Projects (50+ Waypoints)

**Use milestones:**
```markdown
# WAYPOINTS.md

## Milestone 1: Foundation (WP-01 to WP-10)
[Waypoints...]

## Milestone 2: Core Features (WP-11 to WP-25)
[Waypoints...]

## Milestone 3: Polish (WP-26 to WP-35)
[Waypoints...]
```

**Separate GRAPH.md per milestone:**
```
FLOW/
├── GRAPH-M1.md
├── GRAPH-M2.md
└── GRAPH-M3.md
```

---

### Team Collaboration

**FLOW supports parallel work:**

1. **Check GRAPH.md** for independent waypoints
2. **Assign waypoints** to team members/AI sessions
