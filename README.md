# roadmap-site

A [Next.js](https://nextjs.org) 15 (App Router) project bootstrapped with [`create-next-app`](https://nextjs.org/docs/app/api-reference/cli/create-next-app), using React 19, TypeScript, and Tailwind CSS 4.

## Current status

This repository is still close to its initial scaffold: `app/page.tsx` and `app/layout.tsx` currently render the default `create-next-app` starter page, not custom roadmap content. The repo also contains `content/MileStone3.md`, a written project roadmap document, and lists `gray-matter` and `next-mdx-remote` as dependencies for parsing and rendering Markdown — but neither of those packages is imported anywhere in the code yet, so Markdown content from `content/` is not currently rendered by the site. There are no API routes and no backend in this project.

See [GUIDE.md](./GUIDE.md) for a full breakdown of the project structure, configuration, and what's implemented versus what's just scaffolded so far.

## Getting Started

First, run the development server:

```bash
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `app/page.tsx`. The page auto-updates as you edit the file.

This project uses [`next/font`](https://nextjs.org/docs/app/building-your-application/optimizing/fonts) to automatically optimize and load [Geist](https://vercel.com/font), a font family for Vercel.

Other available scripts (from `package.json`):

```bash
npm run build   # next build --turbopack
npm run start   # next start
npm run lint    # eslint
```

## Learn More

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js) - your feedback and contributions are welcome!

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.

Check out our [Next.js deployment documentation](https://nextjs.org/docs/app/building-your-application/deploying) for more details.
