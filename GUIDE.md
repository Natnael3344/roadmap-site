# Developer Guide

This guide covers how the `roadmap-site` project is put together, how to run and build it, and what to know before contributing.

## What this project actually is (current state)

`roadmap-site` is a [Next.js](https://nextjs.org) 15 application (App Router) generated with `create-next-app` and not yet substantially customized:

- `app/page.tsx` and `app/layout.tsx` are still the **unmodified `create-next-app` starter page and layout** — the homepage currently renders the default "Get started by editing `app/page.tsx`" welcome screen with links to Vercel/Next.js docs, not custom roadmap content.
- There is a `content/MileStone3.md` file containing a written project roadmap ("Milestone 3: Sign Language → Speech Model – Detailed Roadmap" for a "Think Inclusion Meeting App"). This is plain Markdown data sitting in the repo — **no route or component currently reads or renders it**.
- `package.json` includes `gray-matter` and `next-mdx-remote` as dependencies. These are libraries typically used to parse Markdown front matter and render Markdown/MDX files as React content, which strongly suggests the intended direction of the project (rendering files from `content/` as pages). As of this commit, **neither package is imported anywhere in the codebase** — they are installed but unused.
- There are **no API routes** (no `app/api/` directory, no `route.ts` files anywhere) and **no calls to `fetch`/`axios`** or any other HTTP client in the source. This is a pure front-end app with no backend and no external API integration today. That's why this repo does not include an `API_REFERENCE.md` — there is no real API to document yet.

In short: the repository is currently a Next.js scaffold plus one roadmap document, with the plumbing for Markdown-driven pages installed but not wired up.

## Tech stack

| Concern | Tool / Version |
|---|---|
| Framework | Next.js 15.5.0 (App Router, Turbopack) |
| UI library | React 19.1.0 / React DOM 19.1.0 |
| Language | TypeScript ^5 (strict mode) |
| Styling | Tailwind CSS ^4 via `@tailwindcss/postcss` |
| Linting | ESLint ^9 with `eslint-config-next` (`next/core-web-vitals`, `next/typescript`) |
| Fonts | `next/font/google` — Geist Sans and Geist Mono |
| Markdown tooling (installed, unused) | `gray-matter` ^4, `next-mdx-remote` ^5 |

Source: `package.json`.

## Project structure

```
roadmap-site/
├── app/
│   ├── favicon.ico
│   ├── globals.css      # Tailwind import + CSS variables for background/foreground, light/dark
│   ├── layout.tsx       # Root layout: loads Geist fonts, sets <html>/<body>, page <title>/description
│   └── page.tsx         # Home route ("/") — default create-next-app starter content
├── content/
│   └── MileStone3.md    # Standalone roadmap document, not yet rendered by any page
├── public/
│   ├── file.svg, globe.svg, next.svg, vercel.svg, window.svg
├── eslint.config.mjs    # Flat ESLint config extending next/core-web-vitals + next/typescript
├── next.config.ts       # Empty NextConfig object — no custom Next.js config yet
├── postcss.config.mjs   # Registers the @tailwindcss/postcss plugin
├── tsconfig.json        # Strict TS config, path alias "@/*" -> project root
├── package.json
└── package-lock.json
```

There is no `app/api/` directory and no server/database code of any kind.

## Configuration details

- **Path alias**: `tsconfig.json` maps `@/*` to the project root, so absolute imports like `import x from "@/content/..."` are available if you add code that needs them.
- **Tailwind**: Tailwind CSS v4 is wired in through `postcss.config.mjs` (single plugin: `@tailwindcss/postcss`) and imported in `app/globals.css` via `@import "tailwindcss";`. Theme tokens (`--color-background`, `--color-foreground`, font variables) are declared with the `@theme inline` block in the same file, and a `prefers-color-scheme: dark` media query swaps the background/foreground variables for dark mode.
- **Fonts**: `app/layout.tsx` loads `Geist` and `Geist_Mono` from `next/font/google` and exposes them as CSS variables (`--font-geist-sans`, `--font-geist-mono`) consumed by `globals.css`.
- **Next.js config**: `next.config.ts` exports an empty `NextConfig` — no redirects, rewrites, image domains, or experimental flags are configured.
- **ESLint**: `eslint.config.mjs` uses the flat-config `FlatCompat` shim to pull in `next/core-web-vitals` and `next/typescript`, and ignores `node_modules`, `.next`, `out`, `build`, and `next-env.d.ts`.
- **Environment variables**: none are referenced anywhere in the code, and `.gitignore` excludes `.env*` files defensively, but no `.env.example` exists because nothing currently reads `process.env`.

## Running the project locally

Requires Node.js (a version compatible with Next.js 15 / React 19 — Node 18.18+ or later is recommended) and npm (a `package-lock.json` is committed).

```bash
npm install
npm run dev
```

Then open [http://localhost:3000](http://localhost:3000). `npm run dev` runs `next dev --turbopack`, so the dev server uses Next.js's Turbopack bundler.

Other scripts defined in `package.json`:

```bash
npm run build   # next build --turbopack — production build, also using Turbopack
npm run start   # next start — serves the production build from `npm run build`
npm run lint    # eslint — runs the flat-config ESLint setup
```

## How the pieces fit together today

1. `app/layout.tsx` is the root layout for every route. It loads the two Geist fonts and wraps `children` in `<html>`/`<body>`, applying the font CSS variables and `antialiased`.
2. `app/page.tsx` is the only route (`/`) and renders the stock `create-next-app` welcome screen (logo, numbered instructions, "Deploy now" / "Read our docs" buttons, and a footer with links to Next.js resources). None of this content is specific to the roadmap-site project yet.
3. `app/globals.css` supplies the Tailwind import, CSS custom properties for colors, and the base `body` styling.
4. `content/MileStone3.md` sits alongside the app code as a plain-text roadmap artifact. It is not imported by any `.tsx` file, so it's effectively documentation-as-content right now rather than something the site displays.

## What a new contributor would likely need to build next

Nothing below is implemented — this section is guidance based on the dependencies already installed, not a description of existing behavior:

- `gray-matter` is designed to parse front matter out of Markdown files (e.g., a title/date block at the top of `content/MileStone3.md`, which currently has none).
- `next-mdx-remote` is designed to render Markdown/MDX strings as React components, typically inside a dynamic route such as `app/roadmap/[slug]/page.tsx` that reads a file from `content/`, parses it, and renders it.
- Wiring these two together against the `content/` directory is the most natural way to turn the current scaffold into an actual "roadmap site" that publishes documents like `MileStone3.md` as pages. Until that happens, the Markdown file must be opened directly (e.g., on GitHub) to be read.

## Deployment

No deployment configuration (e.g., `vercel.json`, Dockerfile, CI workflow) exists in the repo. As a standard Next.js app, it can be deployed with `npm run build` followed by `npm run start` on any Node host, or deployed to [Vercel](https://vercel.com/new) per Next.js's standard deployment flow. There is nothing project-specific to configure for deployment at this time (no environment variables, no external services).

## External APIs

None. The codebase makes no `fetch`/`axios`/HTTP client calls and defines no API routes, so there is nothing to document in an API reference. If Markdown rendering is added per the section above, that would still be local file reads (`fs`/`gray-matter`), not a network API.
