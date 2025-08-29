# 🤖 CLAUDE.md — Engineering Playbook for the Claude Code AI Agent

> Operational rules for building and maintaining the **secure, real‑time, backend‑agnostic React poker client**.
> Audience: automated agents and engineers working in this repo.

Last updated: 2025‑08‑30

---

## 0) Ground Rules (read first)

- **Authoritative server.** Client renders state; it never decides truth.
- **Least‑information.** The client must never receive other players’ private cards or deck order.
- **Protocol‑driven.** Only consume/emit fields defined in `/packages/protocol/`. No ad‑hoc additions.
- **Pure logic.** Game state updates are produced by pure reducers from server events.
- **Deterministic UI.** No random UI behavior; animations are deterministic and cancelable.
- **Type safety.** TypeScript `strict` must remain enabled and green.

---

## 1) Repository Layout

/apps/
simulator/ # Reference backend + e2e harness (WS JSON)
showcase/ # Demo app for dev + Storybook host
/packages/
client/ # React components, hooks, state machine
protocol/ # Versioned message schemas + validators
transport/ # WS (default), pluggable interfaces
theme/ # Tokens, sounds, assets, i18n bundles
/docs/
protocol.md # Human-readable spec (mirrors /protocol)
/tools/
codegen/ # Schema -> TS types, guards, fixtures
scripts/ # Repo-maintenance scripts


**Package manager:** `npm`.  
**Build:** Vite / tsup (for packages).  
**Node target:** LTS (>= 20).  
**ESM first:** All packages are ESM.

---

## 2) TypeScript & Linting

- `tsconfig.json`:
  - `"strict": true`, `"noUncheckedIndexedAccess": true`, `"exactOptionalPropertyTypes": true`
  - `"moduleResolution": "bundler"`, `"types": ["vite/client"]`
- ESLint:
  - `@typescript-eslint` + `eslint-config-next` (if needed) or `eslint-config-airbnb-typescript` sans formatting rules.
- Prettier:
  - Use default config; no line‑length exceptions for generated types.
- CI blocks merges on lint/type errors.

---

## 3) Protocol Discipline

- Source of truth: `/packages/protocol/schema.ts` (or `.proto` if using Protobuf; keep a TS mirror).
- Provide:
  - **Types**: `ServerEvent`, `ClientCommand`, `Capabilities`, `TableSnapshot`
  - **Runtime validators**: `isServerEvent(x): x is ServerEvent` (zod or custom guards)
  - **Versioning**: `version: "1.x.y"`, plus `capabilities: string[]`
  - **Ordering**: every server message includes `seq: number` (monotonic per table)
  - **Scoping**: private messages include `scope: { tableId, seat, sessionId }`
- Breaking changes require:
  - Incremented major version
  - Migration notes in `/docs/protocol.md`
  - Conformance tests in `/apps/simulator`

**Never** log or persist full events if they may contain private data; redact fields first.

---

## 4) Transports (real‑time)

- Default: **WebSocket over TLS**.
- API (`/packages/transport`):

```ts
export interface Transport {
  connect(url: string, token: string): Promise<void>;
  subscribe(topic: string, onMessage: (data: Uint8Array | object) => void): () => void; // returns unsubscribe
  publish(topic: string, msg: object): Promise<void>;
  close(code?: number, reason?: string): Promise<void>;
  onStatus?(cb: (s: "connecting"|"open"|"closed"|"error") => void): void;
}
