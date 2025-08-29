# 📘 Claude Code – Project EPICs

> A secure, real-time, open-source React Poker Client designed to work with multiple backend architectures.

---

## EPIC-01: Protocol & Core Data Model

**Goal:**  
Design a clean, versioned, backend-agnostic message protocol for communication between server and client. Define message schemas and types for poker actions, state updates, and player interactions.

**Key Deliverables:**
- `v1` Protocol Spec (JSON or Protobuf)
- Message Types: `JoinTable`, `CardsDealt`, `ActionRequested`, `BetPlaced`, `HandEnded`, etc.
- Handshake & authentication metadata
- Snapshot vs. streaming updates
- Versioning & capability flags

---

## EPIC-02: State Engine & Event Reducer

**Goal:**  
Build the internal event-driven state machine that applies server messages and updates the client UI accordingly.

**Key Deliverables:**
- Immutable state representation of table, players, hand
- Reducers for server events
- Local prediction (e.g., action sent, optimistic highlight)
- Undo/replay/debug support

---

## EPIC-03: Transport Layer Abstraction

**Goal:**  
Support real-time communication via pluggable transports like WebSocket, Socket.IO, or MQTT, abstracted behind a common interface.

**Key Deliverables:**
- `Transport` interface
- WebSocket implementation with reconnect + resume
- Per-seat pub/sub channel support
- JWT token handling
- Offline/retry logic

---

## EPIC-04: React Poker UI Kit

**Goal:**  
Create a set of highly modular React components representing the poker table, cards, chips, seats, actions, and other elements.

**Key Deliverables:**
- `<PokerTable />`, `<Seat />`, `<Card />`, `<ChipStack />`, `<ActionBar />`
- Responsive layout (6-max, 9-max, HU)
- Accessible & keyboard-friendly components
- Animations (deal, bet, pot pull, showdown)
- Component Storybook

---

## EPIC-05: Seat & Auth Scoping

**Goal:**  
Ensure each client only receives data scoped to their seat. Handle authentication and authorization per session.

**Key Deliverables:**
- JWT seat binding and token validation
- Private vs. public message streams
- Secure reconnect with resume token
- Middleware for permission enforcement

---

## EPIC-06: Security & Anti-Cheat

**Goal:**  
Ensure the client is secure, cheat-resistant, and leak-proof. No other players’ cards or server secrets should be accessible.

**Key Deliverables:**
- Server authority enforcement (never trust client)
- Strict info scoping in protocol
- Optionally: commit-reveal cryptographic dealing (provably fair)
- Action rate limiting, replay protection

---

## EPIC-07: Simulated Game Server (Reference)

**Goal:**  
Build a mock poker server to simulate games for local development, testing, and demo purposes.

**Key Deliverables:**
- Shuffle, deal, validate bets, showdowns
- Simulate different player behaviors (AI bots)
- Push events using the defined protocol
- Serve snapshot on reconnect

---

## EPIC-08: Developer Integration Kit

**Goal:**  
Make it easy for poker operators to plug Claude Code into their backend logic by using lightweight adapter patterns.

**Key Deliverables:**
- Adapter interface: backend-to-protocol
- Reference adapter (Node.js or Go)
- Backend conformance test suite
- Operator onboarding docs

---

## EPIC-09: Table Variants & UX Rules

**Goal:**  
Support flexible table types and UX variants.

**Key Deliverables:**
- 6-max, 9-max, HU layout logic
- Run-it-twice, straddle, ante, etc.
- Turn timers, auto-post blinds, chat/emotes
- UX flow for rebuy/reseat

---

## EPIC-10: Theming & White-label Support

**Goal:**  
Make the client skinnable and brandable by different operators.

**Key Deliverables:**
- Theme tokens (colors, sounds, fonts, logos)
- Configurable UI via JSON/CSS-in-JS
- Language packs / i18n support
- Custom animations (deal, chip movement)

---

## EPIC-11: Testing, QA & Storybook

**Goal:**  
Ensure the code is battle-tested, stable, and easy to work with for contributors.

**Key Deliverables:**
- Jest unit tests for state machine
- E2E tests for client-server gameplay
- Visual tests for components
- Storybook with test scenarios

---

## EPIC-12: Docs & Open Source Portal

**Goal:**  
Build a polished open-source portal with developer onboarding, API docs, architecture overview, and contribution guidelines.

**Key Deliverables:**
- `README.md`, `CONTRIBUTING.md`, architecture diagrams
- Protocol documentation (with live explorer)
- Local dev setup guide
- Example deployments & integrations

---
