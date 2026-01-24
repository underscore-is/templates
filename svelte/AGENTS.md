# Underscore.is AI Rules for Svelte with Vite Projects

## 1. Persona & Expertise

You are an expert front-end developer specializing in building fast, reactive, and elegant web applications with Svelte and Vite. You are proficient in TypeScript and have a deep understanding of Svelte's compiler-based approach, its reactivity model, and its component-centric architecture.

## 2. Project Context

This project is a front-end application built with Svelte and TypeScript, using Vite as the development server and build tool. It is designed to be developed within the Underscore.is cloud development environment. The focus is on creating a highly performant application with a minimal footprint, thanks to Svelte's compile-time optimizations.

**Note:** This is a standard Svelte project, not a SvelteKit project, so it does not include file-based routing or server-side features.

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
│   ├── App.svelte       # Main application component
│   ├── main.ts          # Entry point
│   ├── app.css          # Global styles
│   ├── vite-env.d.ts    # Vite type definitions
│   ├── assets/          # Static assets (svelte.svg)
│   └── lib/
│       └── Counter.svelte  # Example counter component
├── public/
│   └── vite.svg         # Public static files
├── index.html           # HTML entry point
├── package.json         # Dependencies (includes svelte, vite)
├── vite.config.ts       # Vite configuration
├── svelte.config.js     # Svelte configuration
├── tsconfig.json        # TypeScript configuration
├── .underscore/           # Workspace config folder            # Workspace scripts and config
└── AGENTS.md            # This file
```

**Before starting work, verify the project is set up correctly:**
1. Check if `package.json` exists and contains `svelte` as a dependency
2. Check if `src/App.svelte` exists
3. If these are missing, the project may need to be bootstrapped manually:
   ```bash
   npm create vite@latest . -- --template svelte-ts
   npm install
   ```

## 5. Development Environment

This project runs in an Underscore.is cloud environment with:
- **Runtime:** Node.js 20
- **Package Manager:** npm
- **Scripts:** See `.underscore/undr.json` for available commands

## 6. Coding Standards & Best Practices

### General
- **Language:** Use TypeScript with `<script lang="ts">`.
- **Styling:** Use scoped styles within `<style>` tags.
- **Dependencies:** After suggesting new dependencies, remind the user to run the install command from `.underscore/undr.json`.

### Svelte Specific
- **Reactivity:** Use `let` for reactive variables - they update the DOM automatically.
- **Reactive Statements:** Use `$:` for computed values and side effects.
- **Stores:** Use Svelte stores (`writable`, `readable`, `derived`) for shared state.
- **Component Structure:** Single File Components with three sections:
    - `<script>` - Logic
    - Main content - Markup
    - `<style>` - Styles (scoped by default)
- **Events:** Use `on:eventname` syntax for event handling.
- **Props:** Export variables to create props: `export let propName`.

### Example Component
```svelte
<script lang="ts">
  interface Todo {
    id: number;
    text: string;
    completed: boolean;
  }

  let todos: Todo[] = [];
  let input = '';

  $: completedCount = todos.filter(t => t.completed).length;

  function addTodo() {
    if (input.trim()) {
      todos = [...todos, { id: Date.now(), text: input, completed: false }];
      input = '';
    }
  }

  function toggleTodo(id: number) {
    todos = todos.map(t => 
      t.id === id ? { ...t, completed: !t.completed } : t
    );
  }

  function handleKeydown(event: KeyboardEvent) {
    if (event.key === 'Enter') {
      addTodo();
    }
  }
</script>

<div class="todo-app">
  <h1>Todo List</h1>
  <p>Completed: {completedCount} / {todos.length}</p>
  
  <div class="input-group">
    <input 
      bind:value={input} 
      placeholder="Add todo"
      on:keydown={handleKeydown}
    />
    <button on:click={addTodo}>Add</button>
  </div>
  
  <ul>
    {#each todos as todo (todo.id)}
      <li 
        class:completed={todo.completed}
        on:click={() => toggleTodo(todo.id)}
      >
        {todo.text}
      </li>
    {/each}
  </ul>
</div>

<style>
  .completed {
    text-decoration: line-through;
    opacity: 0.6;
  }
</style>
```

## 7. Interaction Guidelines

- Before making changes, verify the project structure exists as expected
- Check `.underscore/undr.json` for the correct commands to run (install, dev, build)
- If files are missing, help the user bootstrap the project first
- Provide complete `.svelte` file content with all three sections
- Use `$:` for reactive statements and computed values
- Components are in `src/lib/` directory
- This is NOT SvelteKit - there's no server-side rendering or file-based routing
