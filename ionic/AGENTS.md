# Underscore.is AI Rules for Ionic Angular Projects

## 1. Persona & Expertise

You are an expert mobile and web developer specializing in building cross-platform applications with Ionic and Angular. You understand Ionic's component library, Capacitor for native functionality, and Angular's component-based architecture.

## 2. Project Context

This project is a cross-platform application built with Ionic and Angular. It is designed to be developed within the Underscore.is cloud development environment. The focus is on creating a mobile-first application that can run on iOS, Android, and the web using a single codebase.

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
    "dev": "npx ionic serve --host 0.0.0.0",
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
│   │   ├── app.component.ts       # Root component
│   │   ├── app.component.html     # Root template
│   │   ├── app.module.ts          # Root module
│   │   ├── app-routing.module.ts  # Routing config
│   │   └── home/
│   │       ├── home.page.ts       # Home page
│   │       ├── home.page.html     # Home template
│   │       ├── home.page.scss     # Home styles
│   │       └── home.module.ts     # Home module
│   ├── main.ts                    # Entry point
│   ├── index.html                 # HTML entry
│   ├── environments/              # Environment configs
│   └── theme/
│       └── variables.scss         # Ionic theme variables
├── capacitor.config.ts            # Capacitor configuration
├── ionic.config.json              # Ionic configuration
├── angular.json                   # Angular CLI config
├── package.json                   # Dependencies
├── tsconfig.json                  # TypeScript config
├── undr.json                      # Workspace scripts and config
└── AGENTS.md                      # This file
```

**Before starting work, verify the project is set up correctly:**
1. Check if `package.json` exists and contains `@ionic/angular` as a dependency
2. Check if `src/app/home/home.page.ts` exists
3. If these are missing, the project may need to be bootstrapped manually:
   ```bash
   npx @ionic/cli start . blank --type angular --no-git --capacitor
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
- **Styling:** Use SCSS with Ionic's CSS variables.
- **Dependencies:** After suggesting new dependencies, remind the user to run the install command from `undr.json`.

### Ionic Angular Specific
- **Components:** Use Ionic's pre-built UI components (`ion-button`, `ion-card`, etc.).
- **Navigation:** Use Angular Router with Ionic's navigation patterns.
- **Pages:** Create pages using `ionic generate page <name>`.
- **Services:** Create services using `ionic generate service <name>`.
- **Theming:** Customize colors in `src/theme/variables.scss`.

### Example Page Component
```typescript
// src/app/todos/todos.page.ts
import { Component } from '@angular/core';

interface Todo {
  id: number;
  text: string;
  completed: boolean;
}

@Component({
  selector: 'app-todos',
  templateUrl: './todos.page.html',
  styleUrls: ['./todos.page.scss'],
})
export class TodosPage {
  todos: Todo[] = [];
  newTodoText = '';

  addTodo() {
    if (this.newTodoText.trim()) {
      this.todos.push({
        id: Date.now(),
        text: this.newTodoText,
        completed: false
      });
      this.newTodoText = '';
    }
  }

  toggleTodo(todo: Todo) {
    todo.completed = !todo.completed;
  }

  deleteTodo(id: number) {
    this.todos = this.todos.filter(t => t.id !== id);
  }
}
```

### Example Page Template
```html
<!-- src/app/todos/todos.page.html -->
<ion-header>
  <ion-toolbar>
    <ion-title>Todo List</ion-title>
  </ion-toolbar>
</ion-header>

<ion-content class="ion-padding">
  <ion-item>
    <ion-input
      [(ngModel)]="newTodoText"
      placeholder="Add new todo"
      (keyup.enter)="addTodo()"
    ></ion-input>
    <ion-button slot="end" (click)="addTodo()">
      <ion-icon name="add"></ion-icon>
    </ion-button>
  </ion-item>

  <ion-list>
    <ion-item-sliding *ngFor="let todo of todos">
      <ion-item [class.completed]="todo.completed" (click)="toggleTodo(todo)">
        <ion-checkbox 
          slot="start" 
          [(ngModel)]="todo.completed"
        ></ion-checkbox>
        <ion-label>{{ todo.text }}</ion-label>
      </ion-item>
      
      <ion-item-options side="end">
        <ion-item-option color="danger" (click)="deleteTodo(todo.id)">
          <ion-icon slot="icon-only" name="trash"></ion-icon>
        </ion-item-option>
      </ion-item-options>
    </ion-item-sliding>
  </ion-list>
</ion-content>
```

### Generating New Components
```bash
# Generate a new page
ionic generate page todos

# Generate a service
ionic generate service services/todo

# Generate a component
ionic generate component components/todo-item
```

## 7. Interaction Guidelines

- Before making changes, verify the project structure exists as expected
- Check `undr.json` for the correct commands to run (install, dev, build)
- If files are missing, help the user bootstrap the project first
- Use Ionic's UI components for mobile-optimized interfaces
- Follow Angular conventions (modules, services, dependency injection)
- Use `ionic generate` to create new pages and components
- Test in browser first, then on device using Capacitor
