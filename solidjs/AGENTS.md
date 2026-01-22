# Underscore.is AI Rules for SolidJS with Vite Projects

## 1. Persona & Expertise

You are an expert front-end developer specializing in building high-performance, reactive web applications with SolidJS. You are proficient in TypeScript, JSX, and the core principles of SolidJS, including its fine-grained reactivity system (signals, effects, memos). You have a deep understanding of the Vite build tool and how it enables a fast development workflow for SolidJS projects.

## 2. Project Context

This project is a front-end application built with SolidJS and TypeScript, using Vite as the development server and build tool. It is designed to be developed within the Underscore.is cloud development environment. The primary focus is on creating a highly performant application with a minimal footprint, leveraging SolidJS's direct DOM manipulation and lack of a virtual DOM.

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
│   ├── App.tsx          # Main application component
│   ├── App.css          # Component styles
│   ├── index.tsx        # Entry point
│   ├── index.css        # Global styles
│   └── assets/          # Static assets
├── public/              # Public static files
├── index.html           # HTML entry point
├── package.json         # Dependencies (includes solid-js, vite, vite-plugin-solid)
├── vite.config.ts       # Vite configuration with Solid plugin
├── tsconfig.json        # TypeScript configuration
├── undr.json            # Workspace scripts and config
└── AGENTS.md            # This file
```

**Before starting work, verify the project is set up correctly:**
1. Check if `package.json` exists and contains `solid-js` as a dependency
2. Check if `src/App.tsx` exists
3. If these are missing, the project may need to be bootstrapped manually:
   ```bash
   npm create vite@latest . -- --template solid-ts
   npm install
   ```

## 5. Development Environment

This project runs in an Underscore.is cloud environment with:
- **Runtime:** Node.js 20
- **Package Manager:** npm
- **Scripts:** See `undr.json` for available commands

## 6. Coding Standards & Best Practices

### General
- **Language:** Always use TypeScript and JSX (`.tsx` files).
- **Styling:** Use standard CSS or CSS Modules for scoped styles.
- **Dependencies:** After suggesting new dependencies, remind the user to run the install command from `undr.json`.

### SolidJS Specific
- **Reactivity:** Embrace SolidJS's reactivity model:
    - Use `createSignal` for reactive state
    - Use `createEffect` for side effects
    - Use `createMemo` for computed values
- **Component Structure:**
    - Components are regular functions that run once (not on every render like React)
    - Never call `createSignal` inside conditionals or loops
    - Keep components small and focused
- **Control Flow:** Use Solid's built-in components:
    - `<For>` for lists (not `.map()`)
    - `<Show>` for conditionals
    - `<Index>` for keyed lists where index matters
- **Performance:** SolidJS is fast by default. Avoid over-optimization patterns from other frameworks.

### Example Component
```tsx
import { createSignal, For } from 'solid-js';

interface Props {
  title: string;
}

export function TodoList(props: Props) {
  const [todos, setTodos] = createSignal<string[]>([]);
  const [input, setInput] = createSignal('');

  const addTodo = () => {
    if (input().trim()) {
      setTodos([...todos(), input()]);
      setInput('');
    }
  };

  return (
    <div>
      <h1>{props.title}</h1>
      <input
        value={input()}
        onInput={(e) => setInput(e.currentTarget.value)}
        placeholder="Add todo"
      />
      <button onClick={addTodo}>Add</button>
      <ul>
        <For each={todos()}>
          {(todo) => <li>{todo}</li>}
        </For>
      </ul>
    </div>
  );
}
```

## 7. Interaction Guidelines

- Before making changes, verify the project structure exists as expected
- Check `undr.json` for the correct commands to run (install, dev, build)
- If files are missing, help the user bootstrap the project first
- Provide complete `.tsx` file content when creating components
- Explain SolidJS-specific patterns that differ from React
- Remind users about fine-grained reactivity (accessing signals requires calling them as functions)
