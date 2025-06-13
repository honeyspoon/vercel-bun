# vercel-bun

Bun runtime for Vercel serverless functions

## Overview

vercel-bun is a custom Vercel runtime that enables you to run serverless functions using the [Bun](https://bun.sh) JavaScript runtime instead of Node.js. This runtime provides improved performance, better TypeScript support, and access to Bun's native APIs.

## Features

- 🚀 **High Performance**: Uses Bun's fast JavaScript runtime (version 1.2.15 by default)
- 📦 **Native TypeScript**: Built-in TypeScript support without additional compilation
- 🌐 **Web Standard APIs**: Uses native `Request` and `Response` interfaces
- 🏗️ **Framework Agnostic**: Works with any framework that supports Bun (Elysia, Hono, etc.)
- 🔧 **Configurable**: Customizable Bun version

## Usage

### 1. Configure your `vercel.json` file

```json
{
  "$schema": "https://openapi.vercel.sh/vercel.json",
  "functions": {
    "api/index.ts": {
      "runtime": "@godsreveal/vercel-bun@0.2.2"
    }
  },
  // Optional: use if you want all /api routes to be handled by /api/index.ts
  "rewrites": [{ "source": "/api/(.*)", "destination": "/api/index.ts" }]
}
```

### 2. Create a Bun serverless function

#### Basic Handler (Vanilla)

```typescript
// api/index.ts
export default function handler(req: Request) {
  return new Response(
    JSON.stringify({ message: `Hello from bun@${Bun.version}` }),
    {
      headers: { "Content-Type": "application/json" },
    }
  );
}
```

#### Advanced Handler (with Elysia)

```typescript
// api/index.ts
import { Elysia, t } from "elysia";

const app = new Elysia({ prefix: "/api" })
  .get("/", () => `Hello from bun@${Bun.version}`)
  .get("/hello", ({ query }) => `Hello ${query.firstName} ${query.lastName}`, {
    query: t.Object({
      firstName: t.String(),
      lastName: t.String(),
    }),
  })
  .post(
    "/users",
    async ({ body }) => {
      // Your logic here
      return { success: true, data: body };
    },
    {
      body: t.Object({
        name: t.String(),
        email: t.String(),
      }),
    }
  );

// Development server (runs locally with `bun run api/index.ts`)
if (process.env.NODE_ENV !== "production") {
  app.listen({ port: 3000 });
  console.log("Server is running on http://localhost:3000");
}

export default app.handle;
```

### 3. Deploy to Vercel

Deploy your GitHub repository to [Vercel](https://vercel.com/docs/git#deploying-a-git-repository).

## Configuration

### Environment Variables

| Variable      | Description                    | Default  | Example  |
| ------------- | ------------------------------ | -------- | -------- |
| `BUN_VERSION` | Specify the Bun version to use | `1.2.15` | `1.2.15` |

## Examples

See the [examples](./examples) directory for several examples:

- **[Vanilla](https://vercel-bun-bench.vercel.app)**

- **[Next.js + ElysiaJS](https://vercel-bun-nine.vercel.app)**

- **[grammY](https://vercel-bun-grammy.vercel.app)**

## Next Steps

- [ ] **Framework Examples**: Add more framework examples (grammY, Hono, Fastify, etc.).

## Goals

- [ ] **First-class Vercel Integration**: Native Bun runtime support in Vercel's platform for both serverless and edge functions.
- [ ] **Next.js Integration**: Runtime specification in `next.config.js` for API routes and React Server Components.
