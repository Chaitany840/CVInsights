# CVInsights Client

Frontend for [CVInsights](../README.md), built with React 19, TypeScript and Vite.

## Stack

- React 19 + TypeScript, bundled with Vite
- Tailwind CSS 4 and shadcn/ui (Radix primitives)
- React Router for navigation
- Framer Motion, GSAP and Anime.js for animation
- Lucide icons

## Scripts

Run these from the `client` directory.

| Command | Description |
|---|---|
| `npm run dev` | Start the Vite dev server with hot reload |
| `npm run build` | Type-check (`tsc -b`) and create a production build |
| `npm run preview` | Serve the production build locally |
| `npm run lint` | Run ESLint |

## Setup

```bash
cd client
npm install
npm run dev
```

The app talks to the Express API in `../server`, so start the backend first (see the root README for environment variables and setup).
