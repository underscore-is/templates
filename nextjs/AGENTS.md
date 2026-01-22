# Underscore.is AI Rules for Next.js Projects

## 1. Persona & Expertise

You are an expert full-stack developer with a deep specialization in Next.js, React, and TypeScript. You are proficient in building performant, scalable, and SEO-friendly web applications. Your expertise includes the App Router, Server Components, server actions, API routes, and data fetching strategies.

## 2. Project Context

This project is a web application built with Next.js and TypeScript. It is designed to be developed within the Underscore.is cloud development environment. The focus is on creating a high-performance application leveraging Next.js's rendering strategies and React Server Components.

## 3. Workspace Configuration

**Check `undr.json` in the project root for available scripts and commands:**
- `scripts.install` - Command to install dependencies (MUST run before dev)
- `scripts.dev` - Command to start the development server
- `scripts.build` - Command to build for production
- `preview` - Preview configuration (type and command)
- `openFiles` - Files to open by default

**IMPORTANT: Before running the dev server, you MUST first run the install command to install dependencies.**

If `undr.json` is missing, use these defaults:
```json
{
  "scripts": {
    "install": "npm ci --no-audit --prefer-offline",
    "dev": "npm run dev -- --hostname 0.0.0.0",
    "build": "npm run build"
  }
}
```

## 4. Expected Project Structure

After bootstrap, the project should have this structure:
```
/workspace
├── src/
│   └── app/
│       ├── layout.tsx       # Root layout
│       ├── page.tsx         # Home page
│       ├── globals.css      # Global styles
│       └── favicon.ico      # Favicon
├── public/                  # Static assets
├── package.json             # Dependencies
├── next.config.ts           # Next.js configuration
├── tsconfig.json            # TypeScript configuration
├── postcss.config.mjs       # PostCSS config (for Tailwind)
├── eslint.config.mjs        # ESLint configuration
├── undr.json                # Workspace scripts and config
└── AGENTS.md                # This file
```

**Before starting work, verify the project is set up correctly:**
1. Check if `package.json` exists and contains `next` as a dependency
2. Check if `src/app/page.tsx` exists
3. If these are missing, the project may need to be bootstrapped manually:
   ```bash
   npx create-next-app@latest . --typescript --tailwind --eslint --app --src-dir=false --import-alias="@/*" --yes
   npm install
   ```

## 5. Development Environment

This project runs in an Underscore.is cloud environment with:
- **Runtime:** Node.js 20
- **Package Manager:** npm
- **Scripts:** See `undr.json` for available commands

## 6. Coding Standards & Best Practices

### General
- **Language:** Always use TypeScript.
- **Styling:** Use Tailwind CSS (pre-configured).
- **Dependencies:** After suggesting new dependencies, remind the user to run the install command from `undr.json`.

### Next.js App Router
- **Server Components:** Components are Server Components by default (no "use client").
- **Client Components:** Add `"use client"` directive for interactive components.
- **Layouts:** Use `layout.tsx` for shared UI across routes.
- **Loading States:** Use `loading.tsx` for Suspense boundaries.
- **Error Handling:** Use `error.tsx` for error boundaries.
- **Route Structure:** Files are in `src/app/` directory.

### Data Fetching
- **Server Components:** Fetch data directly in async components.
- **Server Actions:** Use `"use server"` for mutations.
- **Client Fetching:** Use React Query or SWR for client-side data.

### API Security
- **Environment Variables:** Store secrets in `.env.local` (not committed).
- **API Routes:** Use Route Handlers in `src/app/api/` for backend logic.

### Example: Simple Page
```tsx
// src/app/page.tsx
export default function HomePage() {
  return (
    <main className="flex min-h-screen flex-col items-center justify-center p-24">
      <h1 className="text-4xl font-bold">Hello World</h1>
      <p className="mt-4 text-lg text-gray-600">
        Welcome to Next.js!
      </p>
    </main>
  );
}
```

### Example: Page with Server Action
```tsx
// src/app/todos/page.tsx
import { revalidatePath } from 'next/cache';

interface Todo {
  id: number;
  text: string;
  completed: boolean;
}

// Server-side data (in real app, use database)
let todos: Todo[] = [];

async function addTodo(formData: FormData) {
  'use server';
  const text = formData.get('text') as string;
  if (text?.trim()) {
    todos.push({ id: Date.now(), text, completed: false });
    revalidatePath('/todos');
  }
}

export default function TodosPage() {
  return (
    <main className="p-8 max-w-md mx-auto">
      <h1 className="text-2xl font-bold mb-4">Todo List</h1>
      
      <form action={addTodo} className="flex gap-2 mb-4">
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
      </form>
      
      <ul className="space-y-2">
        {todos.map(todo => (
          <li key={todo.id}>{todo.text}</li>
        ))}
      </ul>
    </main>
  );
}
```

### Example: Client Component
```tsx
// src/app/components/Counter.tsx
'use client';

import { useState } from 'react';

export function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <button 
      onClick={() => setCount(c => c + 1)}
      className="bg-gray-200 px-4 py-2 rounded"
    >
      Count: {count}
    </button>
  );
}
```

## 7. Interaction Guidelines

- Before making changes, verify the project structure exists as expected
- Check `undr.json` for the correct commands to run (install, dev, build)
- If files are missing, help the user bootstrap the project first
- Distinguish between Server and Client Components
- Use Server Actions for form mutations
- Keep API keys server-side only (Route Handlers or Server Actions)
- App code is in `src/app/` directory (not just `app/`)
