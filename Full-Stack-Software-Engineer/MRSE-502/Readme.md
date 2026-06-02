# MRSE-502 API Design & Development

In an era where software systems are increasingly interconnected, Application Programming Interfaces (APIs) serve as the critical backbone of modern digital architecture and distributed computing. **MRSE-502: API Design & Development** provides a comprehensive exploration of the principles, practices, and technologies required to engineer robust, scalable, and secure APIs. Moving beyond foundational programming, this course immerses students in the entire API lifecycle, encompassing architectural planning, interface design, implementation, and deployment. Through a blend of theoretical frameworks and hands-on (practice), learners will master industry-standard paradigms such as RESTful services, GraphQL, and microservices communication, while navigating crucial operational aspects like versioning, authentication, rate limiting, and automated testing. By the end of the course, students will be equipped with the advanced engineering skills necessary to design high-performance, developer-friendly APIs that drive innovation and enable seamless integration across complex software ecosystems.

## 1. Course Description

MRSE-502 provides a rigorous, hands-on foundation in the end-to-end lifecycle of modern API design and development. Students learn to architect, implement, secure, document, and test RESTful and emerging API paradigms entirely within a local full-stack environment powered by **Next.js Route Handlers** and **MySQL 8 via WAMP**.

The course follows an industry-grade engineering workflow — from OpenAPI specification-first design through CI-ready automated testing — and bridges theory with production-quality engineering practices including versioning, rate limiting, observability, and GraphQL basics.

## 2. Learning Outcomes

Upon successful completion, students will be able to:

| # | Outcome |
|---|---------|
| LO-1 | Design resource-oriented REST APIs using OpenAPI 3.1 specification-first methodology |
| LO-2 | Build Next.js App Router Route Handlers connected to a local MySQL 8 database via WAMP |
| LO-3 | Implement authentication and authorization using JWT, API keys, and OAuth 2.0 patterns |
| LO-4 | Apply data validation, error handling, and HTTP semantics correctly and consistently |
| LO-5 | Write and run automated API tests using Jest, Supertest, and Postman/Newman |
| LO-6 | Document APIs with Swagger UI embedded in the Next.js project |
| LO-7 | Version, rate-limit, and paginate APIs following industry standards |
| LO-8 | Apply basic GraphQL schema design and resolver patterns |
| LO-9 | Analyse API performance, security vulnerabilities, and observability patterns |
| LO-10 | Deliver a complete, tested, and documented API project from requirements to deployment-ready state |



## 3. Technology Stack

### Core Stack

| Layer | Technology | Version | Role |
|-------|-----------|---------|------|
| Framework | Next.js | 15.x (App Router) | API route handlers, middleware |
| Language | TypeScript | 5.x | Type-safe API development |
| Runtime | Node.js | 22.x LTS | JavaScript runtime |
| Database | MySQL | 8.x | Relational data store (via WAMP) |
| Local Server | WAMP Server | 3.x | Apache + MySQL + PHP (MySQL GUI) |
| ORM | Prisma | 5.x | Type-safe DB access, migrations |
| Auth | JWT (jose) | 5.x | Stateless authentication |
| Validation | Zod | 3.x | Schema-based request validation |
| Documentation | Swagger UI / OpenAPI | 3.1 | API specification & interactive docs |
| Testing | Jest + Supertest | Latest | Unit & integration testing |
| API Client | Postman + Newman | Latest | Manual & automated API testing |
| Env Management | dotenv / .env.local | — | Environment variable management |

### Supporting Tools

| Tool | Purpose |
|------|---------|
| VS Code | IDE with REST Client extension |
| Git + GitHub | Version control & collaboration |
| TablePlus / phpMyAdmin | MySQL GUI (via WAMP) |
| Thunder Client | VS Code in-editor API testing |
| Insomnia | Alternative REST/GraphQL client |
| pnpm | Fast package manager |
| ESLint + Prettier | Code quality and formatting |



## 4. Environment Setup

### 4.1 WAMP Installation & MySQL Configuration

```bash
# WAMP Stack installed at: C:\wamp64\
# MySQL default port: 3306
# phpMyAdmin: http://localhost/phpmyadmin
```

**MySQL Setup for MRSE-502:**
```sql
-- Create course database
CREATE DATABASE mrse502_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Create application user
CREATE USER 'mrse502_user'@'localhost' IDENTIFIED BY 'your_secure_password';
GRANT ALL PRIVILEGES ON mrse502_db.* TO 'mrse502_user'@'localhost';
FLUSH PRIVILEGES;
```

### 4.2 Next.js Project Initialization

```bash
# Create new Next.js project
pnpm create next-app@latest mrse502-api --typescript --eslint --app --no-tailwind --src-dir

cd mrse502-api

# Install core dependencies
pnpm add prisma @prisma/client zod jose next-swagger-doc swagger-ui-react
pnpm add -D jest @types/jest ts-jest supertest @types/supertest
```

### 4.3 Prisma + MySQL Configuration

```bash
# Initialize Prisma
pnpm prisma init --datasource-provider mysql
```

`.env.local`:
```env
DATABASE_URL="mysql://mrse502_user:your_secure_password@localhost:3306/mrse502_db"
JWT_SECRET="super-secret-key-min-32-chars-long"
API_KEY_SALT="random-salt-for-api-keys"
NODE_ENV="development"
NEXT_PUBLIC_API_BASE_URL="http://localhost:3000/api"
```

### 4.4 Project Structure (Reference)

```
mrse502-api/
├── src/
│   ├── app/
│   │   └── api/
│   │       ├── v1/
│   │       │   ├── users/
│   │       │   │   ├── route.ts          # GET /api/v1/users, POST /api/v1/users
│   │       │   │   └── [id]/
│   │       │   │       └── route.ts      # GET, PUT, DELETE /api/v1/users/:id
│   │       │   ├── products/
│   │       │   └── auth/
│   │       └── docs/
│   │           └── route.ts              # Swagger UI endpoint
│   ├── lib/
│   │   ├── db.ts                         # Prisma client singleton
│   │   ├── auth.ts                       # JWT utilities
│   │   ├── validate.ts                   # Zod schema helpers
│   │   └── errors.ts                     # Standardized error responses
│   ├── middleware.ts                      # Global Next.js middleware
│   └── types/
│       └── api.ts                        # Shared API types
├── prisma/
│   ├── schema.prisma
│   └── migrations/
├── tests/
│   ├── unit/
│   └── integration/
├── docs/
│   └── openapi.yaml                      # OpenAPI 3.1 specification
└── postman/
    └── mrse502.collection.json
```


## 5. Course Modules

### MODULE 1 — Foundations of API Engineering

#### Week 1 — APIs in Modern Software Systems (3 hrs)

**Topics:**
- What is an API? Web API taxonomy: REST, GraphQL, gRPC, WebSockets, tRPC
- HTTP deep dive: methods, status codes, headers, request/response lifecycle
- REST architectural constraints (Fielding's dissertation principles)
- Resource modeling: nouns, not verbs; collections vs. items
- URL design patterns and anti-patterns
- API lifecycle: design → build → test → document → version → deprecate

**Lab 1-A — HTTP Fundamentals (30 min):**
Use Postman to send raw HTTP requests (GET, POST, PUT, DELETE, PATCH) to public APIs (JSONPlaceholder, httpbin.org). Inspect headers, status codes, and response bodies. Document observations.

**Key Concepts:**
- Statelessness, uniform interface, client-server separation
- Safe vs. idempotent methods
- Content negotiation (`Accept`, `Content-Type`)



#### Week 2 — Environment Setup & First Route Handler (3 hrs)

**Topics:**
- WAMP installation, MySQL 8 configuration, phpMyAdmin walkthrough
- Next.js 15 App Router architecture: pages vs. route handlers
- `route.ts` anatomy: `GET`, `POST`, `PUT`, `DELETE`, `PATCH` exports
- Next.js `Request` / `Response` (Web API standard)
- Environment variables with `.env.local`
- Prisma schema design, `prisma migrate dev`, `prisma studio`
- TypeScript essentials for API development: interfaces, generics, utility types

**Lab 1-B — First API Route (60 min):**

```typescript
// src/app/api/v1/health/route.ts
import { NextResponse } from 'next/server';

export async function GET() {
  return NextResponse.json({
    status: 'ok',
    timestamp: new Date().toISOString(),
    version: '1.0.0',
    environment: process.env.NODE_ENV,
  }, { status: 200 });
}
```

Connect Prisma to WAMP MySQL. Run first migration. Verify with phpMyAdmin.

**Deliverable:** Working Next.js project with health endpoint, Prisma connected to local MySQL.


### MODULE 2 — RESTful API Design & Specification

#### Week 3 — Specification-First Design with OpenAPI 3.1 (3 hrs)

**Topics:**
- Why spec-first? Design-time validation, team alignment, auto-generated docs
- OpenAPI 3.1 structure: `info`, `paths`, `components`, `schemas`, `security`
- Describing endpoints: parameters (path, query, header, cookie), requestBody, responses
- Schema definition: data types, `$ref`, `allOf`, `oneOf`, `anyOf`, nullable fields
- Reusable components: schemas, parameters, responses, security schemes
- Tools: Swagger Editor (online), Stoplight Studio

**Lab 2-A — Design a Users API (60 min):**

Design a complete OpenAPI 3.1 spec for a Users resource before writing any code:

```yaml
# docs/openapi.yaml
openapi: "3.1.0"
info:
  title: MRSE-502 Course API
  version: "1.0.0"
  description: API built for MRSE-502 API Design & Development
servers:
  - url: http://localhost:3000/api/v1
    description: Local Development Server

paths:
  /users:
    get:
      summary: List all users
      operationId: listUsers
      tags: [Users]
      parameters:
        - $ref: '#/components/parameters/PageParam'
        - $ref: '#/components/parameters/LimitParam'
      responses:
        '200':
          description: Paginated list of users
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/PaginatedUsers'
        '401':
          $ref: '#/components/responses/Unauthorized'
```

**Key Concepts:**
- Contract-first development workflow
- Linting OpenAPI specs with Spectral
- Semantic versioning for APIs

#### Week 4 — Resource Modeling & URL Design (3 hrs)

**Topics:**
- Hierarchical resource relationships: `/users/{id}/posts/{postId}`
- Filtering, sorting, searching via query parameters: `?sort=createdAt&order=desc&search=john`
- Sparse fieldsets: `?fields=id,name,email`
- HTTP status codes — complete reference and correct usage patterns
- API response envelope design: `data`, `meta`, `links`, `errors`
- HATEOAS (Hypermedia as the Engine of Application State) — introduction

**Standard Response Envelope (course standard):**
```typescript
// src/types/api.ts
export interface ApiResponse<T> {
  data: T;
  meta?: {
    total?: number;
    page?: number;
    limit?: number;
    totalPages?: number;
  };
  links?: {
    self?: string;
    next?: string;
    prev?: string;
    first?: string;
    last?: string;
  };
}

export interface ApiError {
  error: {
    code: string;
    message: string;
    details?: Record<string, string[]>;
    timestamp: string;
    requestId?: string;
  };
}
```

**Lab 2-B — Build Users CRUD API (90 min):**

Implement all Users endpoints following the OpenAPI spec from Lab 2-A. Use Prisma for all database operations.

```typescript
// src/app/api/v1/users/route.ts
import { NextRequest, NextResponse } from 'next/server';
import { prisma } from '@/lib/db';
import { createUserSchema } from '@/lib/schemas/user';
import { paginate } from '@/lib/utils/paginate';

export async function GET(request: NextRequest) {
  const { searchParams } = request.nextUrl;
  const page = parseInt(searchParams.get('page') ?? '1');
  const limit = parseInt(searchParams.get('limit') ?? '10');

  const [users, total] = await prisma.$transaction([
    prisma.user.findMany({ skip: (page - 1) * limit, take: limit }),
    prisma.user.count(),
  ]);

  return NextResponse.json({
    data: users,
    meta: paginate(total, page, limit),
  });
}

export async function POST(request: NextRequest) {
  const body = await request.json();
  const parsed = createUserSchema.safeParse(body);
  if (!parsed.success) {
    return NextResponse.json(
      { error: { code: 'VALIDATION_ERROR', message: 'Invalid input', details: parsed.error.flatten().fieldErrors, timestamp: new Date().toISOString() } },
      { status: 422 }
    );
  }
  const user = await prisma.user.create({ data: parsed.data });
  return NextResponse.json({ data: user }, { status: 201 });
}
```

### MODULE 3 — Data Validation & Error Handling
**Weeks 5–6 | 6 Hours**

#### Week 5 — Input Validation with Zod (3 hrs)

**Topics:**
- Why validate inputs? Security, data integrity, developer experience
- Zod schema design: primitives, objects, arrays, unions, transforms, refinements
- Request validation: body, query parameters, path parameters, headers
- Zod + TypeScript inference: `z.infer<typeof schema>`
- Custom validation rules: email uniqueness, password strength, date ranges
- Reusable validation middleware pattern in Next.js

**Zod Schema Examples:**
```typescript
// src/lib/schemas/user.ts
import { z } from 'zod';

export const createUserSchema = z.object({
  name: z.string().min(2).max(100).trim(),
  email: z.string().email().toLowerCase(),
  password: z.string().min(8).regex(
    /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/,
    'Password must contain uppercase, lowercase, and number'
  ),
  role: z.enum(['USER', 'ADMIN', 'MODERATOR']).default('USER'),
  dateOfBirth: z.string().date().optional(),
});

export const updateUserSchema = createUserSchema.partial().omit({ password: true });
export const querySchema = z.object({
  page: z.coerce.number().int().positive().default(1),
  limit: z.coerce.number().int().min(1).max(100).default(10),
  sort: z.enum(['name', 'email', 'createdAt']).default('createdAt'),
  order: z.enum(['asc', 'desc']).default('desc'),
  search: z.string().optional(),
});

export type CreateUserInput = z.infer<typeof createUserSchema>;
```

**Lab 3-A — Validation Middleware (60 min):**

Build a reusable `withValidation` higher-order function that validates request bodies and query strings, returning standardized 422 errors.


#### Week 6 — Error Handling & HTTP Semantics (3 hrs)

**Topics:**
- Centralized error handling in Next.js route handlers
- Error taxonomy: validation errors (422), auth errors (401/403), not found (404), conflict (409), server errors (500)
- RFC 7807 Problem Details for HTTP APIs
- Operational vs. programmer errors: when to expose vs. hide details
- Database error handling: unique constraint violations, foreign key errors
- Global error boundary and structured logging
- Request ID generation for traceability

**Standardized Error Handler:**
```typescript
// src/lib/errors.ts
export class ApiError extends Error {
  constructor(
    public statusCode: number,
    public code: string,
    message: string,
    public details?: unknown
  ) {
    super(message);
  }
}

export const errorResponse = (error: unknown) => {
  if (error instanceof ApiError) {
    return NextResponse.json(
      { error: { code: error.code, message: error.message, details: error.details, timestamp: new Date().toISOString() } },
      { status: error.statusCode }
    );
  }
  // Log unexpected errors, return generic message
  console.error('[Unhandled API Error]', error);
  return NextResponse.json(
    { error: { code: 'INTERNAL_SERVER_ERROR', message: 'An unexpected error occurred', timestamp: new Date().toISOString() } },
    { status: 500 }
  );
};
```

**Lab 3-B — Error Handling Audit (60 min):**

Refactor all existing route handlers to use the centralized error system. Test each error scenario with Postman. Document all possible error codes per endpoint in OpenAPI spec.


### MODULE 4 — Authentication & Authorization
**Weeks 7–8 | 6 Hours**


#### Week 7 — JWT Authentication (3 hrs)

**Topics:**
- Authentication vs. authorization: definitions and distinctions
- JWT structure: header, payload, signature — base64 decode exercise
- Access tokens vs. refresh tokens: purpose, lifetime, rotation strategy
- Signing algorithms: HS256 vs. RS256 — when to use each
- Implementing JWT with `jose` library (Web Crypto API compatible)
- Storing tokens: `Authorization: Bearer` header (preferred) vs. cookies
- Token blacklisting strategies (in-memory, database, Redis)
- Password hashing with bcrypt

**Implementation:**
```typescript
// src/lib/auth.ts
import { SignJWT, jwtVerify } from 'jose';
import bcrypt from 'bcryptjs';

const secret = new TextEncoder().encode(process.env.JWT_SECRET!);

export async function signToken(payload: Record<string, unknown>, expiresIn = '15m') {
  return new SignJWT(payload)
    .setProtectedHeader({ alg: 'HS256' })
    .setIssuedAt()
    .setExpirationTime(expiresIn)
    .sign(secret);
}

export async function verifyToken(token: string) {
  const { payload } = await jwtVerify(token, secret);
  return payload;
}

export const hashPassword = (password: string) => bcrypt.hash(password, 12);
export const comparePassword = (password: string, hash: string) => bcrypt.compare(password, hash);
```

**Lab 4-A — Auth Endpoints (90 min):**

Build `/api/v1/auth/register`, `/api/v1/auth/login`, and `/api/v1/auth/refresh` endpoints. Implement Next.js middleware to protect routes.

```typescript
// src/middleware.ts
import { NextRequest, NextResponse } from 'next/server';
import { verifyToken } from '@/lib/auth';

export async function middleware(request: NextRequest) {
  const protectedPaths = ['/api/v1/users', '/api/v1/products'];
  const isProtected = protectedPaths.some(p => request.nextUrl.pathname.startsWith(p));

  if (!isProtected) return NextResponse.next();

  const authHeader = request.headers.get('authorization');
  if (!authHeader?.startsWith('Bearer ')) {
    return NextResponse.json({ error: { code: 'UNAUTHORIZED', message: 'Bearer token required', timestamp: new Date().toISOString() } }, { status: 401 });
  }

  try {
    const token = authHeader.split(' ')[1];
    const payload = await verifyToken(token);
    const requestHeaders = new Headers(request.headers);
    requestHeaders.set('x-user-id', payload.sub as string);
    requestHeaders.set('x-user-role', payload.role as string);
    return NextResponse.next({ request: { headers: requestHeaders } });
  } catch {
    return NextResponse.json({ error: { code: 'TOKEN_INVALID', message: 'Invalid or expired token', timestamp: new Date().toISOString() } }, { status: 401 });
  }
}
```

#### Week 8 — Authorization, API Keys & OAuth 2.0 Patterns (3 hrs)

**Topics:**
- Role-Based Access Control (RBAC): roles, permissions, resource ownership
- Attribute-Based Access Control (ABAC) — introduction
- API key authentication: generation (crypto.randomBytes), hashing, storage, scopes
- OAuth 2.0 authorization flows: Authorization Code, Client Credentials (for server-to-server)
- PKCE (Proof Key for Code Exchange) — why it matters for public clients
- Implementing API key middleware alongside JWT
- Security headers: `Strict-Transport-Security`, `X-Content-Type-Options`, `X-Frame-Options`

**API Key Implementation:**
```typescript
// src/app/api/v1/api-keys/route.ts
import { randomBytes, createHash } from 'crypto';

export function generateApiKey(): { raw: string; hash: string; prefix: string } {
  const raw = `mrse_${randomBytes(32).toString('hex')}`;
  const hash = createHash('sha256').update(raw).digest('hex');
  const prefix = raw.substring(0, 12); // store prefix for identification
  return { raw, hash, prefix };
}
```

**Lab 4-B — RBAC Implementation (60 min):**

Add role-based guards to existing endpoints. Admin-only: user management. User: own profile only. Test all permission combinations in Postman.


### MODULE 5 — API Documentation
**Weeks 9 | 3 Hours**

#### Week 9 — Swagger UI & API Documentation (3 hrs)

**Topics:**
- Why documentation matters: the API contract for consumers
- Embedding Swagger UI in Next.js using `next-swagger-doc` and `swagger-ui-react`
- Documenting with JSDoc + OpenAPI annotations (code-first approach)
- Writing effective descriptions: endpoint purpose, usage examples, error codes
- Documentation versioning: keeping docs in sync with code
- Beyond Swagger: Redoc, Stoplight, ReadMe.io (overview)
- Changelog and deprecation notice practices

**Swagger UI Setup in Next.js:**
```typescript
// src/app/api/docs/route.ts
import { createSwaggerSpec } from 'next-swagger-doc';
import { NextResponse } from 'next/server';

export async function GET() {
  const spec = createSwaggerSpec({
    apiFolder: 'src/app/api',
    definition: {
      openapi: '3.1.0',
      info: { title: 'MRSE-502 API', version: '1.0.0' },
      components: {
        securitySchemes: {
          BearerAuth: { type: 'http', scheme: 'bearer', bearerFormat: 'JWT' },
          ApiKeyAuth: { type: 'apiKey', in: 'header', name: 'X-API-Key' },
        },
      },
    },
  });
  return NextResponse.json(spec);
}
```

```typescript
// src/app/docs/page.tsx — Interactive Swagger UI
'use client';
import SwaggerUI from 'swagger-ui-react';
import 'swagger-ui-react/swagger-ui.css';

export default function DocsPage() {
  return <SwaggerUI url="/api/docs" />;
}
```

**Lab 5-A — Full API Documentation (90 min):**

Document 100% of existing endpoints in OpenAPI spec. Add examples, descriptions, and error responses. Verify interactive testing works via Swagger UI at `http://localhost:3000/docs`.

### MODULE 6 — API Testing
**Weeks 10–11 | 6 Hours**

#### Week 10 — Unit & Integration Testing (3 hrs)

**Topics:**
- Testing pyramid for APIs: unit → integration → end-to-end
- Jest configuration for Next.js with TypeScript (ts-jest)
- Testing route handlers with Supertest and `next/test`
- Test doubles: mocks, stubs, spies — when to use each
- Testing validation schemas with Zod
- Database testing strategies: test database, transactions with rollback, seeds
- Code coverage goals and measurement (`jest --coverage`)

**Jest Configuration:**
```javascript
// jest.config.ts
import type { Config } from 'jest';

const config: Config = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  setupFilesAfterEach: ['<rootDir>/tests/setup.ts'],
  moduleNameMapper: { '^@/(.*)$': '<rootDir>/src/$1' },
  collectCoverageFrom: ['src/**/*.ts', '!src/**/*.d.ts'],
  coverageThreshold: { global: { branches: 70, functions: 80, lines: 80 } },
};
export default config;
```

**Sample Integration Test:**
```typescript
// tests/integration/users.test.ts
import { GET, POST } from '@/app/api/v1/users/route';
import { NextRequest } from 'next/server';
import { prisma } from '@/lib/db';

beforeEach(async () => {
  await prisma.user.deleteMany(); // clean state
});

afterAll(async () => {
  await prisma.$disconnect();
});

describe('GET /api/v1/users', () => {
  it('returns 200 with empty data array when no users exist', async () => {
    const request = new NextRequest('http://localhost:3000/api/v1/users');
    const response = await GET(request);
    const body = await response.json();

    expect(response.status).toBe(200);
    expect(body.data).toEqual([]);
    expect(body.meta.total).toBe(0);
  });
});
```

**Lab 6-A — Test Suite Development (90 min):**

Write unit tests for all Zod schemas and utility functions. Write integration tests for all CRUD endpoints covering happy paths and edge cases.

#### Week 11 — Postman & Newman Automated Testing (3 hrs)

**Topics:**
- Postman Collections: organizing requests, environments, variables
- Pre-request scripts: dynamic token injection, timestamp generation
- Test scripts in Postman: `pm.test`, `pm.expect`, `pm.response`, chaining requests
- Environment management: local, staging (future), production (future)
- Newman CLI: running Postman collections from the command line
- Integrating Newman into npm scripts for CI-ready testing
- Contract testing concepts: consumer-driven contracts (Pact — overview)

**Newman CLI Setup:**
```bash
# Install Newman globally
npm install -g newman newman-reporter-htmlextra

# Run collection with local environment
newman run postman/mrse502.collection.json \
  --environment postman/local.environment.json \
  --reporters cli,htmlextra \
  --reporter-htmlextra-export reports/newman-report.html
```

**Postman Test Script Example:**
```javascript
// In Postman Test tab for POST /auth/login
pm.test("Status is 200", () => pm.response.to.have.status(200));
pm.test("Returns access token", () => {
  const body = pm.response.json();
  pm.expect(body.data.accessToken).to.be.a('string');
  pm.collectionVariables.set("accessToken", body.data.accessToken);
});
pm.test("Response time < 500ms", () => pm.expect(pm.response.responseTime).to.be.below(500));
```

**Lab 6-B — Postman Collection Build (90 min):**

Build a complete Postman collection covering all API endpoints with test scripts. Run with Newman and generate an HTML report.

### MODULE 7 — Advanced API Patterns
**Weeks 12–13 | 6 Hours**

#### Week 12 — Versioning, Pagination & Rate Limiting (3 hrs)

**Topics:**
- API versioning strategies: URL path (`/v1/`), header (`Accept-Version`), query param — tradeoffs
- Versioning in Next.js: folder structure `/api/v1/` and `/api/v2/`
- Deprecation headers: `Sunset`, `Deprecation`, `Link` headers
- Pagination patterns: offset/limit, cursor-based, keyset — performance comparison
- Implementing cursor pagination with MySQL and Prisma
- Sorting and filtering best practices
- Rate limiting concepts: token bucket, sliding window algorithms
- In-process rate limiting with `lru-cache` (local development)
- `Retry-After`, `X-RateLimit-Limit`, `X-RateLimit-Remaining` headers

**Rate Limiter (Local Dev):**
```typescript
// src/lib/rateLimit.ts
import { LRUCache } from 'lru-cache';

const rateLimitCache = new LRUCache<string, number[]>({ max: 500, ttl: 60 * 1000 });

export function rateLimit(identifier: string, maxRequests = 60): boolean {
  const now = Date.now();
  const windowMs = 60 * 1000;
  const requests = (rateLimitCache.get(identifier) ?? []).filter(t => now - t < windowMs);
  if (requests.length >= maxRequests) return false;
  rateLimitCache.set(identifier, [...requests, now]);
  return true;
}
```

**Cursor Pagination:**
```typescript
// Cursor-based pagination with Prisma
const users = await prisma.user.findMany({
  take: limit + 1,
  skip: cursor ? 1 : 0,
  cursor: cursor ? { id: cursor } : undefined,
  orderBy: { createdAt: 'desc' },
});
const hasNextPage = users.length > limit;
const nextCursor = hasNextPage ? users[limit - 1].id : null;
```

**Lab 7-A — Pagination & Rate Limiting (60 min):**

Implement cursor-based pagination on the Users endpoint. Add rate limiting middleware. Test limits with Postman runner (100 rapid requests).

#### Week 13 — GraphQL with Next.js (3 hrs)

**Topics:**
- GraphQL vs. REST: tradeoffs, when to choose each
- Core concepts: schema, types, queries, mutations, subscriptions
- Schema Definition Language (SDL): scalars, objects, enums, interfaces, unions
- Resolvers: structure, context, parent, args
- Setting up GraphQL Yoga in Next.js App Router
- Code-first vs. schema-first approaches
- N+1 problem and DataLoader pattern (introduction)
- GraphQL tools: GraphiQL, introspection, fragments

**GraphQL Setup in Next.js:**
```typescript
// src/app/api/graphql/route.ts
import { createYoga, createSchema } from 'graphql-yoga';
import { prisma } from '@/lib/db';

const schema = createSchema({
  typeDefs: `
    type User {
      id: ID!
      name: String!
      email: String!
      createdAt: String!
    }
    type Query {
      users(page: Int, limit: Int): [User!]!
      user(id: ID!): User
    }
    type Mutation {
      createUser(name: String!, email: String!, password: String!): User!
    }
  `,
  resolvers: {
    Query: {
      users: async (_, { page = 1, limit = 10 }) =>
        prisma.user.findMany({ skip: (page - 1) * limit, take: limit }),
      user: async (_, { id }) => prisma.user.findUnique({ where: { id } }),
    },
    Mutation: {
      createUser: async (_, args) => prisma.user.create({ data: args }),
    },
  },
});

const yoga = createYoga({ schema, graphqlEndpoint: '/api/graphql' });
export { yoga as GET, yoga as POST };
```

**Lab 7-B — GraphQL API (90 min):**

Build a parallel GraphQL endpoint for the Products resource. Test queries and mutations via GraphiQL at `http://localhost:3000/api/graphql`.

### MODULE 8 — Security, Performance & Observability
**Week 14 | 3 Hours**

#### Week 14 — API Security, Performance & Logging (3 hrs)

**Topics:**

**Security:**
- OWASP API Security Top 10 (2023): Broken Object Level Authorization, Broken Authentication, Excessive Data Exposure, Lack of Rate Limiting, BOLA, etc.
- SQL injection prevention with Prisma parameterized queries
- Input sanitization vs. validation
- CORS configuration in Next.js: allowed origins, methods, headers
- Sensitive data in responses: field-level filtering, masking PII
- Security headers with `next.config.js`

**Performance:**
- Response caching strategies: in-memory, HTTP cache headers (`Cache-Control`, `ETag`, `Last-Modified`)
- Database query optimization: select only needed fields, avoid N+1, use indexes
- Payload compression: gzip (Next.js default)
- Connection pooling with Prisma

**Observability:**
- Structured logging with `pino` (JSON logs)
- Request/response logging middleware
- Correlation IDs and request tracing
- Health check endpoints: liveness vs. readiness probes
- Basic metrics: request count, latency percentiles, error rates

```typescript
// src/lib/logger.ts
import pino from 'pino';
export const logger = pino({ level: process.env.LOG_LEVEL ?? 'info' });

// src/lib/requestLogger.ts — middleware wrapper
export const withLogging = (handler: Function) => async (req: NextRequest) => {
  const start = Date.now();
  const requestId = crypto.randomUUID();
  const response = await handler(req);
  logger.info({ requestId, method: req.method, path: req.nextUrl.pathname, status: response.status, duration: Date.now() - start });
  return response;
};
```

**CORS Configuration:**
```typescript
// next.config.ts
const nextConfig = {
  async headers() {
    return [
      {
        source: '/api/:path*',
        headers: [
          { key: 'Access-Control-Allow-Origin', value: 'http://localhost:3001' },
          { key: 'Access-Control-Allow-Methods', value: 'GET,POST,PUT,DELETE,PATCH,OPTIONS' },
          { key: 'Access-Control-Allow-Headers', value: 'Content-Type,Authorization,X-API-Key' },
          { key: 'X-Content-Type-Options', value: 'nosniff' },
          { key: 'X-Frame-Options', value: 'DENY' },
        ],
      },
    ];
  },
};
```

**Lab 8-A — Security & Observability Audit (90 min):**

Run OWASP checklist against the course API. Fix identified vulnerabilities. Add request logging middleware to all routes. Verify logs are structured JSON.

### MODULE 9 — Capstone Project & Review
**Week 15 | 3 Hours**

#### Week 15 — Project Presentations & Course Review (3 hrs)

**Topics:**
- Capstone project presentations (groups of 2)
- Code review and peer feedback session
- API design retrospective: what would you change?
- Emerging API trends: tRPC, REST vs. GraphQL vs. gRPC in 2025+
- WebSockets and Server-Sent Events for real-time APIs
- Edge functions and serverless API deployment (Vercel, Cloudflare Workers — overview)
- Career pathways: Backend Engineer, API Platform Engineer, Developer Experience Engineer

## 6. Lab & Project Structure

### Weekly Lab Summary

| Week | Lab | Focus Area | Hours |
|------|-----|-----------|-------|
| 1 | 1-A | HTTP Fundamentals with Postman | 0.5 |
| 2 | 1-B | First Route Handler + Prisma Setup | 1.0 |
| 3 | 2-A | OpenAPI 3.1 Spec Design | 1.0 |
| 4 | 2-B | Users CRUD API | 1.5 |
| 5 | 3-A | Validation Middleware with Zod | 1.0 |
| 6 | 3-B | Centralized Error Handling | 1.0 |
| 7 | 4-A | JWT Auth Endpoints | 1.5 |
| 8 | 4-B | RBAC Implementation | 1.0 |
| 9 | 5-A | Full Swagger UI Documentation | 1.5 |
| 10 | 6-A | Jest + Supertest Test Suite | 1.5 |
| 11 | 6-B | Postman Collection + Newman | 1.5 |
| 12 | 7-A | Pagination & Rate Limiting | 1.0 |
| 13 | 7-B | GraphQL API | 1.5 |
| 14 | 8-A | Security Audit & Logging | 1.5 |
| 15 | —  | Capstone Presentation | 1.0 |


### Capstone Project Requirements

**Project Title:** Full-Stack REST API — Domain of Your Choice

Students will design and build a production-quality API for a real-world domain (e-commerce, healthcare scheduling, library management, food delivery, etc.).

**Minimum Requirements:**

- [ ] **Domain model:** Minimum 4 related MySQL tables with proper relationships and indexes
- [ ] **Endpoints:** Minimum 20 unique endpoints across at least 4 resources
- [ ] **Auth:** JWT-based authentication with refresh tokens + role-based authorization (3 roles minimum)
- [ ] **Validation:** Zod schemas on all write operations (POST, PUT, PATCH)
- [ ] **Error handling:** Centralized error system with all standard error codes
- [ ] **Documentation:** Complete OpenAPI 3.1 spec + live Swagger UI
- [ ] **Testing:** Jest unit tests (≥70% coverage) + Postman collection (≥30 test cases)
- [ ] **Versioning:** `/v1/` URL versioning with deprecation headers on at least one changed endpoint
- [ ] **Pagination:** Cursor or offset pagination on all list endpoints
- [ ] **Rate limiting:** Applied to auth endpoints and public listing endpoints
- [ ] **Security:** CORS configured, security headers set, no sensitive data leakage
- [ ] **Logging:** Structured JSON logs with request IDs on all endpoints
- [ ] **GraphQL:** At least one resource exposed via a GraphQL endpoint
- [ ] **README.md:** Setup guide, architecture overview, endpoint summary, test instructions

**Deliverables:**
1. GitHub repository (private, instructor invited)
2. 15-minute live demo + code walkthrough (Week 15)
3. Postman collection with Newman HTML report
4. Peer review of one other team's API (written, 1 page)


## 7. Assessment & Grading

| Component | Weight | Details |
|-----------|--------|---------|
| Weekly Labs (14 labs) | 28% | 2% each — submitted as GitHub commits |
| Midterm API Design Assignment | 15% | OpenAPI spec + ER diagram for a given domain |
| Quiz 1 — HTTP & REST Fundamentals | 7% | Week 4, 30 min, 25 MCQs |
| Quiz 2 — Auth & Security | 7% | Week 9, 30 min, 25 MCQs |
| Capstone Project | 35% | Rubric below |
| Participation & Peer Review | 8% | Weekly discussions + 1 written peer review |

### Capstone Grading Rubric

| Criterion | Points |
|-----------|--------|
| Functionality — all endpoints work correctly | 20 |
| API Design — RESTful principles, URL structure, status codes | 15 |
| Authentication & Authorization — JWT + RBAC | 15 |
| Data Validation & Error Handling | 10 |
| Testing — coverage, quality of assertions | 15 |
| Documentation — OpenAPI completeness, Swagger UI | 10 |
| Security — OWASP checklist compliance | 5 |
| Code Quality — TypeScript, structure, naming | 5 |
| Presentation — clarity, demo quality, Q&A | 5 |
| **Total** | **100** |

### Grading Scale

| Grade | Range |
|-------|-------|
| A | 90–100% |
| B | 80–89% |
| C | 70–79% |
| D | 60–69% |
| F | Below 60% |


## 8. Tools & References

### Required Software (All Free)

- [Node.js 22 LTS](https://nodejs.org/) — JavaScript runtime
- [WAMP Server](https://www.wampserver.com/) — Apache + MySQL + PHP
- [VS Code](https://code.visualstudio.com/) — Code editor
- [Postman](https://www.postman.com/) — API testing client
- [Git](https://git-scm.com/) — Version control
- [pnpm](https://pnpm.io/) — Package manager (`npm install -g pnpm`)

### Recommended VS Code Extensions

```json
{
  "recommendations": [
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "prisma.prisma",
    "rangav.vscode-thunder-client",
    "humao.rest-client",
    "42crunch.vscode-openapi",
    "yoavbls.pretty-ts-errors"
  ]
}
```

### Core Documentation

| Resource | URL |
|----------|-----|
| Next.js Route Handlers | https://nextjs.org/docs/app/building-your-application/routing/route-handlers |
| Prisma Docs | https://www.prisma.io/docs |
| Zod Documentation | https://zod.dev |
| OpenAPI 3.1 Spec | https://spec.openapis.org/oas/v3.1.0 |
| jose (JWT) | https://github.com/panva/jose |
| GraphQL Yoga | https://the-guild.dev/graphql/yoga-server |
| OWASP API Security Top 10 | https://owasp.org/API-Security/ |

### Recommended Reading

- *Designing Web APIs* — Brenda Jin, Saurabh Sahni, Amir Shevat (O'Reilly)
- *REST API Design Rulebook* — Mark Masse (O'Reilly)
- *The Design of Web APIs* — Arnaud Lauret (Manning)
- RFC 9110: HTTP Semantics — https://www.rfc-editor.org/rfc/rfc9110
- RFC 7807: Problem Details for HTTP APIs — https://www.rfc-editor.org/rfc/rfc7807

### Prisma Schema Reference (Course Models)

```prisma
// prisma/schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}

model User {
  id           String    @id @default(cuid())
  name         String
  email        String    @unique
  passwordHash String    @map("password_hash")
  role         Role      @default(USER)
  isActive     Boolean   @default(true) @map("is_active")
  apiKeys      ApiKey[]
  sessions     Session[]
  createdAt    DateTime  @default(now()) @map("created_at")
  updatedAt    DateTime  @updatedAt @map("updated_at")

  @@index([email])
  @@map("users")
}

model ApiKey {
  id         String    @id @default(cuid())
  keyHash    String    @unique @map("key_hash")
  keyPrefix  String    @map("key_prefix")
  name       String
  scopes     Json
  userId     String    @map("user_id")
  user       User      @relation(fields: [userId], references: [id], onDelete: Cascade)
  lastUsedAt DateTime? @map("last_used_at")
  expiresAt  DateTime? @map("expires_at")
  createdAt  DateTime  @default(now()) @map("created_at")

  @@index([userId])
  @@map("api_keys")
}

model Session {
  id           String   @id @default(cuid())
  userId       String   @map("user_id")
  user         User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  refreshToken String   @unique @map("refresh_token")
  expiresAt    DateTime @map("expires_at")
  createdAt    DateTime @default(now()) @map("created_at")

  @@index([userId])
  @@map("sessions")
}

enum Role {
  USER
  MODERATOR
  ADMIN
}
```

## Appendix A — Course Schedule at a Glance

| Week | Module | Topic | Lab |
|------|--------|-------|-----|
| 1 | M1 | APIs & HTTP Fundamentals | 1-A: HTTP with Postman |
| 2 | M1 | Next.js + WAMP Setup, First Route | 1-B: Health Endpoint |
| 3 | M2 | OpenAPI 3.1 Spec-First Design | 2-A: Users Spec |
| 4 | M2 | Resource Modeling, URL Design | 2-B: Users CRUD |
| 5 | M3 | Input Validation with Zod | 3-A: Validation Middleware |
| 6 | M3 | Error Handling & HTTP Semantics | 3-B: Error System |
| 7 | M4 | JWT Authentication | 4-A: Auth Endpoints |
| 8 | M4 | Authorization & API Keys | 4-B: RBAC |
| 9 | M5 | Swagger UI Documentation | 5-A: Full Docs |
| 10 | M6 | Unit & Integration Testing | 6-A: Jest Suite |
| 11 | M6 | Postman & Newman | 6-B: Collection |
| 12 | M7 | Versioning, Pagination, Rate Limiting | 7-A: Pagination |
| 13 | M7 | GraphQL with Next.js | 7-B: GraphQL API |
| 14 | M8 | Security, Performance, Observability | 8-A: Security Audit |
| 15 | M9 | Capstone Presentations & Review | Demo Day |

## Appendix B — npm Scripts Reference

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "format": "prettier --write src/**/*.ts",
    "db:migrate": "prisma migrate dev",
    "db:push": "prisma db push",
    "db:studio": "prisma studio",
    "db:seed": "ts-node prisma/seed.ts",
    "db:reset": "prisma migrate reset",
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "test:newman": "newman run postman/mrse502.collection.json -e postman/local.environment.json --reporters cli,htmlextra --reporter-htmlextra-export reports/report.html",
    "generate": "prisma generate"
  }
}
```

