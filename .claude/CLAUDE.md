
You are an expert in TypeScript, Angular, and scalable web application development. You write functional, maintainable, performant, and accessible code following Angular and TypeScript best practices.

## TypeScript Best Practices

- Use strict type checking
- Prefer type inference when the type is obvious
- Avoid the `any` type; use `unknown` when type is uncertain

## Angular Best Practices

- Always use standalone components over NgModules
- Must NOT set `standalone: true` inside Angular decorators. It's the default in Angular v20+.
- Use signals for state management
- Implement lazy loading for feature routes
- Do NOT use the `@HostBinding` and `@HostListener` decorators. Put host bindings inside the `host` object of the `@Component` or `@Directive` decorator instead
- Use `NgOptimizedImage` for all static images.
  - `NgOptimizedImage` does not work for inline base64 images.

## Accessibility Requirements

- It MUST pass all AXE checks.
- It MUST follow all WCAG AA minimums, including focus management, color contrast, and ARIA attributes.

### Components

- Keep components small and focused on a single responsibility
- Use `input()` and `output()` functions instead of decorators
- Use `computed()` for derived state
- Set `changeDetection: ChangeDetectionStrategy.OnPush` in `@Component` decorator
- Always use separate `.html` and `.scss` files for templates and styles — no inline templates or styles
- Prefer Reactive forms instead of Template-driven ones
- Do NOT use `ngClass`, use `class` bindings instead
- Do NOT use `ngStyle`, use `style` bindings instead
- When using external templates/styles, use paths relative to the component TS file.

## State Management

- Use signals for local component state
- Use `computed()` for derived state
- Keep state transformations pure and predictable
- Do NOT use `mutate` on signals, use `update` or `set` instead

## Templates

- Keep templates simple and avoid complex logic
- Use native control flow (`@if`, `@for`, `@switch`) instead of `*ngIf`, `*ngFor`, `*ngSwitch`
- Use the async pipe to handle observables
- Do not assume globals like (`new Date()`) are available.
- Do not write arrow functions in templates (they are not supported).

## Services

- Design services around a single responsibility
- Use the `providedIn: 'root'` option for singleton services
- Use the `inject()` function instead of constructor injection

---

# Geek-CRM — Customer Relationship Management for Local Service Businesses

## Project Overview

CRM built specifically for Geek At Your Spot and similar local service businesses (IT consultants, contractors, tradespeople) in South Florida. Manages contacts, deals/quotes, invoices, follow-ups, and service history. Built as Angular Elements on WordPress + Express backend.

## Workspace Structure

```
Geek-CRM-Workspace/
  projects/
    geek-crm-library/           # Angular library — components, services, models
      src/
        public-api.ts           # Library entry point
        lib/
          models/               # TypeScript interfaces
          services/             # API services
          components/           # UI components
    geek-crm-elements/          # Angular Elements app — registers custom elements
      src/main.ts               # Registers custom elements
    geek-crm-backend/           # Express + TypeScript backend
      prisma/schema.prisma      # Database schema
      src/
        server.ts               # Express entry point
        config/                 # Environment, logger, database
        routes/                 # API route handlers
        middleware/             # Error handler
        utils/                  # Error helpers, response helpers
```

## Component Inventory

(To be populated as components are created)

| Custom Element Tag | Component | Directory |
|---|---|---|
| | | |

## Service Inventory

(To be populated as services are created)

| Service | Location | Purpose |
|---|---|---|

## API Endpoints

(To be populated as routes are created)

| Method | Endpoint | Description |
|---|---|---|

## Build Commands

```bash
# Library
ng build geek-crm-library

# Elements app (produces main.js + styles.css)
ng build geek-crm-elements

# Backend
cd projects/geek-crm-backend
npm run dev          # Development with hot reload
npm run build        # TypeScript compilation
npm run lint         # ESLint check
npx prisma generate  # Regenerate Prisma client
npx prisma migrate dev  # Run migrations
```

## Session Notes

**February 17, 2026 (Session 1):**
- Workspace created with `ng new --no-create-application`
- Claude Code initialized with settings, skills, and MCP agents copied from geek-flow-workspace
- Plan.md created with competitive analysis and phased implementation roadmap
- Next: Create Angular library + Elements app projects, set up Express backend, Prisma schema
