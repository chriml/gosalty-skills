---
name: gosalty-external-agent
description:
  Use when building, wiring, or operating external AI agents that interact with
  GoSalty data and workflows, especially partner dashboard actions such as
  creating, updating, publishing, cancelling, or deleting events, partner
  locations, onboarding, and partner knowledge.
---

# GoSalty External Agent

Use this skill to connect AI agents to GoSalty through the existing Convex
surface. Prefer existing agent tools and public Convex functions over direct
database writes.

## Repository Override

Convex coding guidance from `convex/_generated/ai/guidelines.md` is
authoritative. Read it before editing Convex code and prefer it over older local
examples.

Do not run `npx convex dev`, `npx convex dev --once`, or deployment-affecting
Convex commands unless the user explicitly clears that action.

## Start Here

1. Read `convex/_generated/ai/guidelines.md` before editing Convex code.
2. Identify the actor:
   - Partner dashboard agent: use the existing tools in
     `convex/db/partners/tools/`.
   - App user/community event: use `api.db.events.mutations.submitUserEvent`.
   - Feed or discovery read: use paginated queries in `convex/db/events/queries.ts`.
3. For destructive or publishing actions, require an explicit user confirmation
   before mutating.
4. Keep selectors user-friendly: accept partner, location, event, and spot by
   name or id when using tools.

## Preferred Integration Path

For AI agent actions inside GoSalty, register or reuse tools on
`convex/agents/chatAgent/chatAgent.ts`. Current partner tools already cover:

- `createPartnerEventTool`
- `updatePartnerEventTool`
- `deletePartnerEventTool`
- `createPartnerLocationTool`
- `updatePartnerLocationTool`
- `deletePartnerLocationTool`
- `updatePartnerLocationOnboardingTool`
- `addPartnerKnowledgeTool`
- `deletePartnerKnowledgeTool`
- `setPartnerSpotForecastDefaultTool`

Read [references/partner-tools.md](references/partner-tools.md) when changing or
calling these tools.

## Event Workflows

Read [references/events.md](references/events.md) before implementing event
creation, publishing, importing, or external-agent event management.

Core rules:

- Partner events require a partner/location-scoped authenticated user.
- Community events require the authenticated app user and are always published.
- Store event times as epoch milliseconds.
- Validate `start < end`, non-negative `cost`, and non-negative
  `maxParticipants`.
- Resolve and store coordinates for any event location; event discovery relies
  on the geospatial index.
- Do not move a published/cancelled partner event back to `draft`.

## External API Shape

If exposing a new external integration:

- Add the smallest Convex action/mutation needed; keep sensitive work internal
  by default.
- Authenticate first with `getAuthUserId(ctx)` or the partner auth wrappers.
- Use inline `v.*` validators for Convex args.
- Reuse normalization helpers from the existing module before adding new logic.
- Return stable ids and concise status payloads, not full documents unless the
  caller needs them.

## Verification

- For changes under `convex/`, run type checking inside `convex` only.
- For partner event tools, add or update focused Convex tests when behavior
  changes.
- Do not start `convex dev` from this repo.
