# AGENTS.md — Bingo Mixer

Social "human bingo" icebreaker app: 5×5 board, players find people matching prompts, win with 5 in a row. Also a **Copilot agent lab** — see [workshop/GUIDE.md](workshop/GUIDE.md) before restructuring tooling or docs.

## Mandatory development checklist

Run all three before committing or marking any task complete:

- [ ] `npm run lint` passes
- [ ] `npm run build` passes (`tsc -b && vite build`)
- [ ] `npm test` passes (vitest run)

## Commands

Node 22+ · `npm install` · `npm run dev` → http://localhost:5173

## Architecture

State flows one way: pure logic → hook → presentational components.

| Layer | File | Rule |
|-------|------|------|
| Domain types | [src/types/index.ts](src/types/index.ts) | Shared interfaces only; no logic |
| Pure logic | [src/utils/bingoLogic.ts](src/utils/bingoLogic.ts) | No React/side effects — unit-tested in [bingoLogic.test.ts](src/utils/bingoLogic.test.ts) |
| State | [src/hooks/useBingoGame.ts](src/hooks/useBingoGame.ts) | ALL state + localStorage persistence; components stay stateless |
| Screens | [src/App.tsx](src/App.tsx) | Switches on `GameState` (`'start' \| 'playing' \| 'bingo'`) |
| Components | [src/components/](src/components/) | Presentational only: props in, callbacks out |
| Content | [src/data/questions.ts](src/data/questions.ts) | Prompt pool — use the **Quiz Master** agent |

## Conventions

- **Tailwind 4, CSS-first** — tokens in `@theme` in [src/index.css](src/index.css). **Always read** [.github/instructions/tailwind-4.instructions.md](.github/instructions/tailwind-4.instructions.md) before writing CSS.
- **Mobile-first touch UI** — `active:` not `hover:`, `select-none`, generous tap targets.
- **TypeScript strict** — `verbatimModuleSyntax` → `import type` for types; `erasableSyntaxOnly` bans enums/namespaces/parameter properties.
- **Imports** — relative paths only.
- **Tests** — Vitest `globals: true` (no imports needed), jsdom, setup in [src/test/setup.ts](src/test/setup.ts). Test pure logic, not styling.

## Pitfalls

- Board is always 25 squares; center (index 12) is pre-marked free; pool needs **≥ 24 unique prompts** or `generateBoard` breaks.
- localStorage state is versioned (`STORAGE_VERSION`) — **bump it** when the stored shape changes or users get silently reset.
- Vite base path auto-detects `VITE_REPO_NAME` for GitHub Pages — don't hardcode asset URLs.
- `vite.config.ts` imports `defineConfig` from `vitest/config`, not `vite` — keep it so the `test` block typechecks.

## Agents & skills

- **Agents**: `Quiz Master` (prompts), `Pixel Jam` (UI design), `UI Review`, `tdd` / `tdd-red` / `tdd-green` / `tdd-refactor`
- **Skills**: `setup` (first-time setup), `frontend-design`

## Docs

- [README.md](README.md) — run/build, deploy (translations: [es](README.es.md), [pt-BR](README.pt_BR.md))
- [workshop/](workshop/) — lab guide (00–05); **update all three locales** (`es/`, `pt_BR/`) when editing lab steps
- [docs/](docs/) — published pages mirroring the workshop
