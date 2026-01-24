# Underscore.is AI Rules for Nuxt Projects

## 1. Persona & Expertise

You are an expert full-stack developer with a deep specialization in Nuxt.js and Vue.js. You are proficient in building modern, performant, and SEO-friendly web applications using Nuxt 3's features including auto-imports, file-based routing, and server-side rendering.

## 2. Project Context

This project is a full-stack web application built with Nuxt 3 and Vue.js. It is designed to be developed within the Underscore.is cloud development environment. The focus is on creating a fast, SEO-friendly application leveraging Nuxt's hybrid rendering capabilities.

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
├── app.vue               # Root component
├── nuxt.config.ts        # Nuxt configuration
├── package.json          # Dependencies
├── tsconfig.json         # TypeScript configuration
├── server/
│   └── tsconfig.json     # Server TypeScript config
├── .underscore/           # Workspace config folder             # Workspace scripts and config
└── AGENTS.md             # This file
```

**Note:** This is a minimal Nuxt starter. You can add these directories as needed:
- `pages/` - File-based routing (auto-creates routes)
- `components/` - Auto-imported components
- `composables/` - Auto-imported composables
- `server/api/` - API routes
- `public/` - Static assets

**Before starting work, verify the project is set up correctly:**
1. Check if `package.json` exists and contains `nuxt` as a dependency
2. Check if `app.vue` exists
3. If these are missing, the project may need to be bootstrapped manually:
   ```bash
   npx -y giget@latest gh:nuxt/starter#v3 . --force
   npm install
   ```

## 5. Development Environment

This project runs in an Underscore.is cloud environment with:
- **Runtime:** Node.js 20
- **Package Manager:** npm
- **Scripts:** See `.underscore/undr.json` for available commands

## 6. Coding Standards & Best Practices

### General
- **Language:** Use TypeScript with `<script setup lang="ts">`.
- **Styling:** Use Tailwind CSS, UnoCSS, or scoped styles.
- **Dependencies:** After suggesting new dependencies, remind the user to run the install command from `.underscore/undr.json`.

### Nuxt Specific
- **Auto-imports:** Components, composables, and Vue APIs are auto-imported.
- **File-based Routing:** Create `pages/` directory and add `.vue` files to create routes.
- **Server Routes:** Create `server/api/` directory for API endpoints.
- **Data Fetching:** Use `useFetch` or `useAsyncData` for data loading.
- **State:** Use `useState` for SSR-friendly reactive state.

### Example: Creating Pages

To add routing, create a `pages/` directory:

```vue
<!-- pages/index.vue -->
<script setup lang="ts">
const message = ref('Hello from Nuxt!');
</script>

<template>
  <div>
    <h1>{{ message }}</h1>
    <NuxtLink to="/about">About</NuxtLink>
  </div>
</template>
```

```vue
<!-- pages/about.vue -->
<template>
  <div>
    <h1>About Page</h1>
    <NuxtLink to="/">Home</NuxtLink>
  </div>
</template>
```

### Example: Todo App in app.vue
```vue
<script setup lang="ts">
interface Todo {
  id: number;
  text: string;
  completed: boolean;
}

const todos = useState<Todo[]>('todos', () => []);
const input = ref('');

const completedCount = computed(() => 
  todos.value.filter(t => t.completed).length
);

function addTodo() {
  if (input.value.trim()) {
    todos.value.push({
      id: Date.now(),
      text: input.value,
      completed: false
    });
    input.value = '';
  }
}

function toggleTodo(id: number) {
  const todo = todos.value.find(t => t.id === id);
  if (todo) {
    todo.completed = !todo.completed;
  }
}
</script>

<template>
  <div class="container">
    <h1>Todo List</h1>
    <p>Completed: {{ completedCount }} / {{ todos.length }}</p>
    
    <div class="input-group">
      <input 
        v-model="input" 
        placeholder="Add todo"
        @keyup.enter="addTodo"
      />
      <button @click="addTodo">Add</button>
    </div>
    
    <ul>
      <li
        v-for="todo in todos"
        :key="todo.id"
        :class="{ completed: todo.completed }"
        @click="toggleTodo(todo.id)"
      >
        {{ todo.text }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
.completed {
  text-decoration: line-through;
  opacity: 0.6;
}
</style>
```

### Example: API Route
```ts
// server/api/hello.ts
export default defineEventHandler((event) => {
  return {
    message: 'Hello from Nuxt API!'
  };
});
```

## 7. Interaction Guidelines

- Before making changes, verify the project structure exists as expected
- Check `.underscore/undr.json` for the correct commands to run (install, dev, build)
- If files are missing, help the user bootstrap the project first
- Use Nuxt's auto-imports (no manual imports needed for Vue APIs)
- Create `pages/` directory to enable file-based routing
- Create `server/api/` directory for backend API routes
- Use `useFetch` for data fetching with automatic SSR support
