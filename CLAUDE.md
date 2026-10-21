# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm install       # install dependencies
npm run dev       # Vite dev server on http://localhost:3000 (auto-opens browser)
npm run build     # production build to ./build (not ./dist)
```

There is no test runner, linter, or `tsconfig.json` configured. Vite (SWC) strips types without type-checking, so TypeScript errors will not fail the build.

## Architecture

A single-page React 18 + Vite treasure-hunt game, originally exported from Figma Make.

- **All game logic and UI live in `src/App.tsx`.** Three chests are created on mount; one randomly holds treasure. Opening the treasure chest gives +$100, a skeleton chest -$50. The game ends when the treasure is found or all chests are open; "Play Again" calls `initializeGame()` to reshuffle.
- Animations use `motion/react` (Framer Motion). Images in `src/assets/` and sounds in `src/audios/` are imported as ES modules so Vite bundles them.
- `src/components/ui/` is a stock shadcn/ui (Radix) component library; only `Button` is currently used. `src/components/figma/ImageWithFallback.tsx` is a Figma export helper.
- `vite.config.ts` aliases version-pinned import specifiers (e.g. `'sonner@2.0.3'`) to the real packages. These exist because the shadcn components import with version suffixes, so keep them when adding or editing those components. `@` is aliased to `./src`.

## Styling caveat: Tailwind is precompiled

`src/index.css` (imported by `main.tsx`) is a **static, pre-generated Tailwind v4 output** that includes only the utility classes that were in use at export time. Tailwind is not installed and there is no PostCSS/Tailwind build step. **A Tailwind class that isn't already in `src/index.css` will have no effect.** Before relying on a new utility, grep `src/index.css` for it. If it's missing, use inline styles, add a hand-written rule to `index.css`, or set up Tailwind properly. `src/styles/globals.css` holds the design-token source but is not imported anywhere.

## Other notes

- `src/guidelines/Guidelines.md` is an empty Figma Make template with no active rules.
- When adding third-party assets (sounds, icons), credit them in `Attributions.md`.
- `README.md` is a tutorial script for a Claude Code demo, not project documentation. It mentions planned features (sound effects on chest open, a win/tie/loss result display, a `key.png` hover cursor, SQLite sign-in/scores with a guest mode, Vercel/GitHub Pages deploy commands). The assets for these (`chest_open*.mp3`, `key.png`) already exist in `src/` and are partly wired up.
