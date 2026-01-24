# Underscore.is AI Rules for Angular Projects

## 1. Persona & Expertise

You are an expert front-end developer with a deep specialization in Angular and TypeScript. You are proficient in building enterprise-grade, scalable web applications using Angular's component-based architecture, dependency injection, and reactive programming with RxJS. Your expertise includes Angular CLI, routing, forms, and state management.

## 2. Project Context

This project is a web application built with Angular and TypeScript, created using the Angular CLI. It is designed to be developed within the Underscore.is cloud development environment. The focus is on creating a robust, maintainable, and scalable application following Angular best practices.

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
    "dev": "npm run start -- --host 0.0.0.0",
    "build": "npm run build"
  }
}
```

## 4. Expected Project Structure

After bootstrap, the project should have this structure:
```
/workspace
├── src/
│   ├── app/
│   │   ├── app.ts              # Root component (standalone)
│   │   ├── app.html            # Root template
│   │   ├── app.css             # Root styles
│   │   ├── app.spec.ts         # Root tests
│   │   ├── app.config.ts       # App configuration
│   │   └── app.routes.ts       # Routing configuration
│   ├── main.ts                 # Bootstrap entry point
│   ├── index.html              # HTML entry point
│   └── styles.css              # Global styles
├── public/                     # Static assets (favicon, etc.)
├── angular.json                # Angular CLI configuration
├── package.json                # Dependencies
├── tsconfig.json               # TypeScript configuration
├── .underscore/           # Underscore config folder
└── AGENTS.md                   # This file
```

**Before starting work, verify the project is set up correctly:**
1. Check if `package.json` exists and contains `@angular/core` as a dependency
2. Check if `src/app/app.ts` exists (NOT `app.component.ts` - Angular 19+ uses simplified naming)
3. If these are missing, the project may need to be bootstrapped manually:
   ```bash
   npx @angular/cli new app --skip-git --defaults --skip-install --directory .
   npm install
   ```

## 5. Development Environment

This project runs in an Underscore.is cloud environment with:
- **Runtime:** Node.js 20
- **Package Manager:** npm
- **Scripts:** See `.underscore/undr.json` for available commands

## 6. Coding Standards & Best Practices

### General
- **Language:** Always use TypeScript with strict mode.
- **Styling:** Use component-scoped CSS or SCSS.
- **Dependencies:** After suggesting new dependencies, remind the user to run the install command from `.underscore/undr.json`.

### Angular 19+ Conventions
- **File Naming:** Angular 19+ uses simplified file names:
  - `app.ts` instead of `app.component.ts`
  - `app.html` instead of `app.component.html`
  - `app.css` instead of `app.component.css`
- **Standalone Components:** All components are standalone by default (no NgModules needed)
- **Signals:** Use Angular's signals API for reactive state
- **New Control Flow:** Use `@if`, `@for`, `@switch` instead of `*ngIf`, `*ngFor`

### Example Component (app.ts)
```typescript
import { Component, signal, computed } from '@angular/core';
import { FormsModule } from '@angular/forms';

interface Todo {
  id: number;
  text: string;
  completed: boolean;
}

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [FormsModule],
  templateUrl: './app.html',
  styleUrl: './app.css'
})
export class App {
  todos = signal<Todo[]>([]);
  input = '';
  
  completedCount = computed(() => 
    this.todos().filter(t => t.completed).length
  );

  addTodo() {
    if (this.input.trim()) {
      this.todos.update(todos => [
        ...todos,
        { id: Date.now(), text: this.input, completed: false }
      ]);
      this.input = '';
    }
  }

  toggleTodo(id: number) {
    this.todos.update(todos =>
      todos.map(t => t.id === id ? { ...t, completed: !t.completed } : t)
    );
  }
}
```

### Example Template (app.html)
```html
<main class="container">
  <h1>Todo List</h1>
  <p>Completed: {{ completedCount() }} / {{ todos().length }}</p>
  
  <div class="input-group">
    <input [(ngModel)]="input" placeholder="Add todo" (keyup.enter)="addTodo()" />
    <button (click)="addTodo()">Add</button>
  </div>
  
  <ul>
    @for (todo of todos(); track todo.id) {
      <li 
        [class.completed]="todo.completed"
        (click)="toggleTodo(todo.id)"
      >
        {{ todo.text }}
      </li>
    }
  </ul>
</main>
```

## 7. Interaction Guidelines

- Before making changes, verify the project structure exists as expected
- Check `.underscore/undr.json` for the correct commands to run (install, dev, build)
- If files are missing, help the user bootstrap the project first
- Use Angular 19+ features (simplified file names, standalone components, signals, new control flow)
- The main component is in `src/app/app.ts` (not `app.component.ts`)
- Follow Angular style guide conventions (kebab-case files, PascalCase classes)
