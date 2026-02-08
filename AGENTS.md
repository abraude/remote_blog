# Agent Development Guidelines

This is a SvelteKit project with TypeScript, Drizzle ORM, better-auth, and Tailwind CSS.

## Development Commands

```bash
# Development
bun run dev              # Start dev server
bun run dev -- --open    # Start dev server and open in browser

# Building
bun run build            # Production build
bun run preview          # Preview production build

# Quality Checks
bun run check            # Type checking (svelte-check)
bun run check:watch      # Type checking in watch mode
bun run lint             # Linting (prettier + eslint)
bun run format           # Format code with prettier

# Database
bun run db:push          # Push schema changes to database
bun run db:generate      # Generate migrations
bun run db:migrate       # Run migrations
bun run db:studio        # Open Drizzle Studio

# Authentication
bun run auth:schema      # Generate auth schema from better-auth config
```

## Code Style Guidelines

### Imports

- Use ES module imports
- Group imports by: external packages, SvelteKit internals, project modules
- SvelteKit-specific imports use `$` prefix (e.g., `$app/server`, `$lib/server/auth`, `$env/dynamic/private`)
- Absolute imports: use `$lib/` alias for project files, avoid `../` or `./` when possible

### Formatting

- Use tabs for indentation
- Single quotes for strings
- No trailing commas
- Max line width: 100 characters
- Svelte files use 'svelte' parser

### TypeScript

- Strict mode enabled
- No explicit type imports from packages; rely on auto-imports
- Use `$types` import for SvelteKit types: `import type { PageServerLoad, Actions } from './$types'`
- Define types explicitly for props: `let { data }: { data: PageServerData } = $props()`

### Naming Conventions

- Functions: camelCase (e.g., `handleBetterAuth`, `signInEmail`, `signOut`)
- Variables: camelCase
- Database tables: lowercase (e.g., `task`)
- Exported DB tables: lowercase, matching table name
- Types/interfaces: PascalCase
- Constants: SCREAMING_SNAKE_CASE

### Svelte Components

- Use Svelte 5 syntax with `$props()` for props
- Always specify `<script lang="ts">` for TypeScript
- Use `use:enhance` for form actions
- Server-side files use `.server.ts` extension

### Error Handling

- Throw early for missing env vars: `if (!env.DATABASE_URL) throw new Error('DATABASE_URL is not set')`
- Use `try-catch` blocks in actions, checking for `APIError` from better-auth
- Return `fail(400, { message: error.message })` for validation errors
- Return `fail(500, { message: 'Unexpected error' })` for generic errors
- Use `redirect(302, '/path')` for redirects

### Database Schema

- Use `drizzle-orm` with SQLite
- Define schemas using fluent API: `text('id').primaryKey()`
- Generate auth schema via `bun run auth:schema`
- Use `.notNull()` for required fields

### Global Types

- Define types in `src/app.d.ts` under `global namespace App`
- Extend `App.Locals` for session/user data

## Tech Stack

- SvelteKit 2.x with Svelte 5, TypeScript (strict mode), Drizzle ORM with SQLite, better-auth, Tailwind CSS 4.x, Vite 7.x

## Linting/Testing Notes

- ESLint configured with TypeScript and Svelte plugins; Prettier configured with Svelte and Tailwind plugins
- No test runner configured (tests, if any, must be run manually)
- Always run `bun run lint` and `bun run check` before committing changes

## Environment Variables

- Use `$env/dynamic/private` for server-side env vars
- Required: `DATABASE_URL`, `BETTER_AUTH_SECRET`, `ORIGIN`
- Store env templates in `.env.example`, never commit `.env` files
- Check for required env vars at module initialization and throw if missing

## Project Structure & Naming

```
src/
├── routes/          # SvelteKit routes (file-based routing)
├── lib/
│   ├── server/      # Server-only code (auth, db, utilities)
│   └── assets/      # Static assets (favicon, images)
├── app.d.ts         # Global type definitions
└── app.html         # Root HTML template
```

- Svelte pages: `+page.svelte` (component) and `+page.server.ts` (load/actions)
- Layouts: `+layout.svelte` and `+layout.server.ts`
- Server actions in `+page.server.ts`; use `PageServerLoad` and `Actions` types from `./$types`

## Common Patterns

```typescript
// Server Load
export const load: PageServerLoad = async (event) => {
	if (!event.locals.user) return redirect(302, '/login');
	return { user: event.locals.user };
};

// Form Action
export const actions: Actions = {
	actionName: async (event) => {
		const formData = await event.request.formData();
		const field = formData.get('field')?.toString() ?? '';
		return redirect(302, '/success');
	}
};

// Component Props
let { data }: { data: PageServerData } = $props();
```

```svelte
<!-- Form Enhancement -->
<form method="post" action="?/actionName" use:enhance>
```

## Database & Workflow

- Import db from `$lib/server/db` and schema from `$lib/server/db/schema`
- Use `.insert()`, `.select()`, `.update()`, `.delete()` from drizzle-orm
- Define default values with `.default()` or `.$defaultFn()`
- Workflow: modify schema → `bun run db:push` → implement server logic → create UI → lint/check
