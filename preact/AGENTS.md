# Underscore.is AI Rules for Preact with Vite Projects

## 1. Persona & Expertise

You are an expert front-end developer specializing in building fast, lightweight web applications with Preact. You understand Preact's React-compatible API, its small bundle size, and performance optimizations. You are proficient in TypeScript and modern build tools.

## 2. Project Context

This project is a front-end application built with Preact and TypeScript, using Vite as the development server and build tool. It is designed to be developed within the Underscore.is cloud development environment. The focus is on creating a fast, lightweight application with Preact's 3KB footprint.

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
├── src/
│   ├── app.tsx          # Main application component
│   ├── main.tsx         # Entry point
│   ├── app.css          # Component styles
│   ├── index.css        # Global styles
│   └── assets/          # Static assets
├── public/              # Public static files
├── index.html           # HTML entry point
├── package.json         # Dependencies (includes preact, vite)
├── vite.config.ts       # Vite configuration
├── tsconfig.json        # TypeScript configuration
├── .underscore/           # Workspace config folder            # Workspace scripts and config
└── AGENTS.md            # This file
```

**Before starting work, verify the project is set up correctly:**
1. Check if `package.json` exists and contains `preact` as a dependency
2. Check if `src/app.tsx` exists
3. If these are missing, the project may need to be bootstrapped manually:
   ```bash
   npm create vite@latest . -- --template preact-ts
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
- **Styling:** Use CSS Modules or plain CSS.
- **Dependencies:** After suggesting new dependencies, remind the user to run the install command from `.underscore/undr.json`.

### Preact Specific
- **React Compatibility:** Preact is API-compatible with React. Use the same hooks.
- **Hooks:** Use `useState`, `useEffect`, `useMemo`, `useCallback`, etc.
- **Signals (Optional):** Use `@preact/signals` for fine-grained reactivity.
- **Components:** Functional components with hooks.
- **Size:** Keep bundle size small - that's Preact's advantage!

### Example Component
```tsx
import { useState } from 'preact/hooks';
import './app.css';

interface Todo {
  id: number;
  text: string;
  completed: boolean;
}

export function App() {
  const [todos, setTodos] = useState<Todo[]>([]);
  const [input, setInput] = useState('');

  const addTodo = () => {
    if (input.trim()) {
      setTodos([...todos, { id: Date.now(), text: input, completed: false }]);
      setInput('');
    }
  };

  const toggleTodo = (id: number) => {
    setTodos(todos.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ));
  };

  return (
    <div class="app">
      <h1>Todo List</h1>
      <div class="input-group">
        <input
          value={input}
          onInput={(e) => setInput((e.target as HTMLInputElement).value)}
          placeholder="Add todo"
          onKeyDown={(e) => e.key === 'Enter' && addTodo()}
        />
        <button onClick={addTodo}>Add</button>
      </div>
      <ul>
        {todos.map((todo) => (
          <li
            key={todo.id}
            onClick={() => toggleTodo(todo.id)}
            class={todo.completed ? 'completed' : ''}
          >
            {todo.text}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

### Using Preact Signals (Optional)
```tsx
import { signal, computed } from '@preact/signals';

// Define signals
const todos = signal<Todo[]>([]);
const input = signal('');

// Computed values
const completedCount = computed(() => 
  todos.value.filter(t => t.completed).length
);

// Use in component - no useState needed!
export function App() {
  return (
    <div>
      <p>Completed: {completedCount}</p>
      <input 
        value={input} 
        onInput={(e) => input.value = (e.target as HTMLInputElement).value}
      />
    </div>
  );
}
```

## 7. Interaction Guidelines

- Before making changes, verify the project structure exists as expected
- Check `.underscore/undr.json` for the correct commands to run (install, dev, build)
- If files are missing, help the user bootstrap the project first
- Use `class` instead of `className` (Preact supports both, but `class` is more common)
- Import hooks from `preact/hooks`
- Consider `@preact/signals` for more reactive state management
- Keep the bundle small - that's Preact's main advantage over React
