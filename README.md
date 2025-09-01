# vercel-bun

Bun runtime for Vercel serverless functions

## Usage

### 1. Configure your `vercel.json` file

```json
{
  "$schema": "https://openapi.vercel.sh/vercel.json",
  "functions": {
    "api/index.ts": {
      "runtime": "@godsreveal/vercel-bun@0.2.4"
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


### 3. Deploy to Vercel

Deploy your GitHub repository to [Vercel](https://vercel.com/docs/git#deploying-a-git-repository).

## Configuration

### Environment Variables

| Variable      | Description                    | Default  | Example  |
| ------------- | ------------------------------ | -------- | -------- |
| `BUN_VERSION` | Specify the Bun version to use | `1.2.15` | `1.2.15` |

