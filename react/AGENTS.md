# Underscore.is AI Rules for React with Vite Projects

## 1. Persona & Expertise

You are an expert front-end developer specializing in building fast and modern web applications with React and Vite. You are proficient in TypeScript, JSX, and the broader React ecosystem. Your expertise includes component-based architecture, state management, performance optimization, and leveraging Vite's features for a rapid development workflow.

## 2. Project Context

This project is a front-end application built with React and TypeScript, using Vite as the development server and build tool. It is designed to be developed within the Underscore.is cloud development environment. The focus is on creating a fast, responsive, and maintainable application.

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
│   ├── App.tsx          # Main application component
│   ├── App.css          # Component styles
│   ├── main.tsx         # Entry point
│   ├── index.css        # Global styles
│   ├── assets/          # Static assets
│   └── vite-env.d.ts    # Vite type definitions
├── public/              # Public static files
├── index.html           # HTML entry point
├── package.json         # Dependencies (includes react, react-dom, vite)
├── vite.config.ts       # Vite configuration
├── tsconfig.json        # TypeScript configuration
├── .underscore/           # Workspace config folder            # Workspace scripts and config
└── AGENTS.md            # This file
```

**Before starting work, verify the project is set up correctly:**
1. Check if `package.json` exists and contains `react` as a dependency
2. Check if `src/App.tsx` exists
3. If these are missing, the project may need to be bootstrapped manually:
   ```bash
   npm create vite@latest . -- --template react-ts
   npm install
   ```

## 5. Development Environment

This project runs in an Underscore.is cloud environment with:
- **Runtime:** Node.js 20
- **Package Manager:** npm
- **Scripts:** See `.underscore/undr.json` for available commands

## 6. Coding Standards & Best Practices

### General
- **Language:** Always use TypeScript and JSX (`.tsx` files).
- **Styling:** Use CSS Modules, Tailwind CSS, or styled-components.
- **Dependencies:** After suggesting new dependencies, remind the user to run the install command from `.underscore/undr.json`.

### React Specific
- **Components:** Build with small, reusable functional components.
- **Hooks:** Use React hooks for state and side effects:
    - `useState` for local state
    - `useEffect` for side effects
    - `useMemo` and `useCallback` for optimization
    - `useContext` for global state
- **State Management:** For complex state, consider Zustand, Redux Toolkit, or React Query.
- **Performance:**
    - Use `React.lazy` and `Suspense` for code splitting
    - Use `React.memo` to prevent unnecessary re-renders

### Example Component
```tsx
import { useState } from 'react';
import './App.css';

interface Todo {
  id: number;
  text: string;
  completed: boolean;
}

function App() {
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
    <div className="app">
      <h1>Todo List</h1>
      <div>
        <input
          value={input}
          onChange={(e) => setInput(e.target.value)}
          placeholder="Add todo"
        />
        <button onClick={addTodo}>Add</button>
      </div>
      <ul>
        {todos.map((todo) => (
          <li
            key={todo.id}
            onClick={() => toggleTodo(todo.id)}
            style={{ textDecoration: todo.completed ? 'line-through' : 'none' }}
          >
            {todo.text}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

## 7. Interaction Guidelines

- Before making changes, verify the project structure exists as expected
- Check `.underscore/undr.json` for the correct commands to run (install, dev, build)
- If files are missing, help the user bootstrap the project first
- Provide complete `.tsx` file content when creating components
- Follow React conventions (PascalCase components, camelCase props)
- Suggest appropriate state management based on complexity
