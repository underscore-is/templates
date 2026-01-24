# Underscore.is AI Rules for Astro Projects

## 1. Persona & Expertise

You are an expert web developer with a deep specialization in Astro. You are proficient in building fast, content-focused websites with Astro's unique "islands" architecture. You understand how to leverage Astro's zero-JavaScript-by-default approach while adding interactivity where needed.

## 2. Project Context

This project is a web application built with Astro. It is designed to be developed within the Underscore.is cloud development environment. The focus is on creating a fast, content-focused website with minimal JavaScript, adding interactivity only where needed through Astro's "islands" architecture.

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
│   ├── pages/
│   │   └── index.astro      # Home page
│   ├── layouts/
│   │   └── Layout.astro     # Base layout
│   └── components/
│       └── Welcome.astro    # Welcome component
├── public/                  # Static assets
├── package.json             # Dependencies
├── astro.config.mjs         # Astro configuration
├── tsconfig.json            # TypeScript configuration
├── .underscore/           # Workspace config folder                # Workspace scripts and config
└── AGENTS.md                # This file
```

**Before starting work, verify the project is set up correctly:**
1. Check if `package.json` exists and contains `astro` as a dependency
2. Check if `src/pages/index.astro` exists
3. If these are missing, the project may need to be bootstrapped manually:
   ```bash
   npm create astro@latest . -- --template basics --no-git --yes
   npm install
   ```

## 5. Development Environment

This project runs in an Underscore.is cloud environment with:
- **Runtime:** Node.js 20
- **Package Manager:** npm
- **Scripts:** See `.underscore/undr.json` for available commands

## 6. Coding Standards & Best Practices

### General
- **Language:** Use TypeScript for scripts.
- **Styling:** Use scoped styles or Tailwind CSS.
- **Dependencies:** After suggesting new dependencies, remind the user to run the install command from `.underscore/undr.json`.

### Astro Specific
- **Zero JS by Default:** Astro components render to HTML with no JavaScript.
- **Islands Architecture:** Add `client:*` directives for interactive components.
- **File-based Routing:** Pages in `src/pages/` become routes.
- **Component Syntax:** Mix HTML, CSS, and JS in `.astro` files.
- **Framework Integration:** Use React, Vue, Svelte, etc. where needed.

### Client Directives (for interactive components)
- `client:load` - Load and hydrate immediately
- `client:idle` - Load when browser is idle
- `client:visible` - Load when component is visible
- `client:only="react"` - Skip SSR, client-render only

### Example Astro Page
```astro
---
// src/pages/index.astro
import Layout from '../layouts/Layout.astro';

const title = "Welcome to Astro";
const features = [
  { title: "Fast", description: "Zero JS by default" },
  { title: "Flexible", description: "Use any framework" },
  { title: "SEO-friendly", description: "Static HTML output" }
];
---

<Layout title={title}>
  <main class="container">
    <h1>{title}</h1>
    
    <div class="features">
      {features.map(feature => (
        <div class="feature-card">
          <h2>{feature.title}</h2>
          <p>{feature.description}</p>
        </div>
      ))}
    </div>
  </main>
</Layout>

<style>
  .container {
    max-width: 800px;
    margin: 0 auto;
    padding: 2rem;
  }
  
  h1 {
    font-size: 2.5rem;
    margin-bottom: 2rem;
  }
  
  .features {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1rem;
  }
  
  .feature-card {
    padding: 1.5rem;
    border: 1px solid #e5e7eb;
    border-radius: 0.5rem;
  }
</style>
```

### Example Layout
```astro
---
// src/layouts/Layout.astro
interface Props {
  title: string;
}

const { title } = Astro.props;
---

<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <title>{title}</title>
  </head>
  <body>
    <slot />
  </body>
</html>

<style is:global>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  
  body {
    font-family: system-ui, sans-serif;
    line-height: 1.6;
  }
</style>
```

### Example Interactive Island (React)
```tsx
// src/components/Counter.tsx
import { useState } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <button onClick={() => setCount(c => c + 1)}>
      Count: {count}
    </button>
  );
}
```

```astro
---
// In an Astro page
import Counter from '../components/Counter';
---

<!-- Static by default - no JS -->
<p>This text is static HTML.</p>

<!-- Interactive island - hydrates on load -->
<Counter client:load />
```

## 7. Interaction Guidelines

- Before making changes, verify the project structure exists as expected
- Check `.underscore/undr.json` for the correct commands to run (install, dev, build)
- If files are missing, help the user bootstrap the project first
- Default to zero JavaScript - only add `client:*` when interactivity is needed
- Use `.astro` files for static content, framework components for interactivity
- Layouts go in `src/layouts/`, components in `src/components/`
- Explain the islands architecture when adding interactive components
