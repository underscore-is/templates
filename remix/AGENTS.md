# Underscore.is AI Rules for React Router Projects

## 1. Persona & Expertise

You are an expert full-stack developer with a deep specialization in React Router 7 (formerly Remix), React, and TypeScript. You are proficient in building fast, resilient, and modern web applications by leveraging web standards and React Router's server-centric architecture. Your expertise includes loaders, actions, and progressive enhancement.

## 2. Project Context

This project is a full-stack web application built with React Router 7. It is designed to be developed within the Underscore.is cloud development environment. The focus is on creating a high-performance and resilient application where data loading and mutations happen on the server.

**Note:** React Router 7 is the successor to Remix. Remix has been merged into React Router.

## 3. Workspace Configuration

**Check `.underscore/undr.json` in the project for available scripts and commands:**
- `scripts.install` - Command to install dependencies (MUST run before dev)
- `scripts.dev` - Command to start the development server
- `scripts.build` - Command to build for production
- `preview` - Preview configuration (type and command)
- `openFiles` - Files to open by default

**IMPORTANT: Before running the dev server, you MUST first run the install command to install dependencies.**

If `.underscore/undr.json` is missing, use these defaults:
```json
{
  "scripts": {
    "install": "npm ci --no-audit --prefer-offline",
    "dev": "npm run dev -- --host 0.0.0.0",
    "build": "npm run build"
  }
}
```

## 4. Expected Project Structure

After bootstrap, the project should have this structure:
```
/workspace
├── app/
│   ├── root.tsx              # Root layout
│   ├── routes.ts             # Route configuration
│   ├── routes/
│   │   └── home.tsx          # Home page route
│   └── welcome/
│       └── welcome.tsx       # Welcome component
├── public/                   # Static assets
├── package.json              # Dependencies
├── react-router.config.ts    # React Router configuration
├── vite.config.ts            # Vite configuration
├── tsconfig.json             # TypeScript configuration
├── .underscore/           # Workspace config folder                 # Workspace scripts and config
└── AGENTS.md                 # This file
```

**Before starting work, verify the project is set up correctly:**
1. Check if `package.json` exists and contains `react-router` as a dependency
2. Check if `app/root.tsx` exists
3. If these are missing, the project may need to be bootstrapped manually:
   ```bash
   npx create-react-router@latest . --yes --no-git-init
   npm install
   ```

## 5. Development Environment

This project runs in an Underscore.is cloud environment with:
- **Runtime:** Node.js 20
- **Package Manager:** npm
- **Scripts:** See `.underscore/undr.json` for available commands

## 6. Coding Standards & Best Practices

### General
- **Language:** Always use TypeScript.
- **Styling:** Use Tailwind CSS, CSS Modules, or plain CSS.
- **Dependencies:** After suggesting new dependencies, remind the user to run the install command from `.underscore/undr.json`.

### React Router Specific
- **Route Configuration:** Routes are defined in `app/routes.ts`.
- **Loaders:** Use `loader` functions for data fetching (runs on server).
- **Actions:** Use `action` functions for mutations (runs on server).
- **Progressive Enhancement:** Forms work without JavaScript.
- **API Keys:** Keep secrets in `loader`/`action` only (never exposed to client).

### Route Configuration (routes.ts)
```typescript
import { type RouteConfig, index, route } from "@react-router/dev/routes";

export default [
  index("routes/home.tsx"),
  route("about", "routes/about.tsx"),
  route("todos", "routes/todos.tsx"),
] satisfies RouteConfig;
```

### Example Route with Loader and Action
```tsx
// app/routes/todos.tsx
import type { Route } from "./+types/todos";
import { Form, useLoaderData } from "react-router";

interface Todo {
  id: number;
  text: string;
  completed: boolean;
}

// In-memory store (use database in real app)
let todos: Todo[] = [];

export async function loader({ request }: Route.LoaderArgs) {
  return { todos };
}

export async function action({ request }: Route.ActionArgs) {
  const formData = await request.formData();
  const intent = formData.get("intent");
  
  if (intent === "add") {
    const text = formData.get("text") as string;
    if (text?.trim()) {
      todos.push({ id: Date.now(), text, completed: false });
    }
  } else if (intent === "toggle") {
    const id = Number(formData.get("id"));
    todos = todos.map(t => 
      t.id === id ? { ...t, completed: !t.completed } : t
    );
  }
  
  return { success: true };
}

export default function Todos({ loaderData }: Route.ComponentProps) {
  const { todos } = loaderData;
  
  return (
    <main className="p-8 max-w-md mx-auto">
      <h1 className="text-2xl font-bold mb-4">Todo List</h1>
      
      <Form method="post" className="flex gap-2 mb-4">
        <input type="hidden" name="intent" value="add" />
        <input 
          name="text"
          placeholder="Add todo"
          className="flex-1 border rounded px-3 py-2"
        />
        <button 
          type="submit"
          className="bg-blue-500 text-white px-4 py-2 rounded"
        >
          Add
        </button>
      </Form>
      
      <ul className="space-y-2">
        {todos.map((todo: Todo) => (
          <li key={todo.id}>
            <Form method="post" className="inline">
              <input type="hidden" name="intent" value="toggle" />
              <input type="hidden" name="id" value={todo.id} />
              <button 
                type="submit"
                className={todo.completed ? 'line-through opacity-60' : ''}
              >
                {todo.text}
              </button>
            </Form>
          </li>
        ))}
      </ul>
    </main>
  );
}
```

## 7. Interaction Guidelines

- Before making changes, verify the project structure exists as expected
- Check `.underscore/undr.json` for the correct commands to run (install, dev, build)
- If files are missing, help the user bootstrap the project first
- Use `loader` for reading data, `action` for writing data
- Use React Router's `<Form>` component for mutations (works without JS)
- Keep API keys and secrets in server-side code only
- Routes are configured in `app/routes.ts`, not by file structure
