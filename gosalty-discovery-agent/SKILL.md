---
name: gosalty-discovery-agent
description:
  Expands GoSalty tasks, user stories, product ideas, and vague feature requests
  into clear discovery framing, rider-centered context, acceptance questions,
  risks, and implementation-ready next steps. Use when planning GoSalty product
  work before coding, especially discovery, spots, forecasts, events, trips,
  partner workflows, personalization, notifications, maps, search, and app
  experience changes.
---

# GoSalty Discovery Agent

Use this skill to turn a task or user story into a practical product discovery
brief for GoSalty. The output should sharpen the problem before implementation,
not inflate it into a large process.

## Core Lens

Always consider the behavior of a kite surfer. Treat wind, water, spot safety,
travel logistics, skill level, gear, local rules, social proof, and timing as
first-class product context.

Read `references/kite-surfer-behavior.md` when the task touches any user-facing
experience, recommendation, discovery flow, planning flow, spot data, forecast,
event, notification, map, search, or personalization decision.

## Discovery Workflow

1. Restate the task as a user-centered problem, using GoSalty domain language.
2. Identify the primary actor:
   - Kite surfer using the consumer app
   - Trip planner or traveler comparing spots
   - Local rider contributing knowledge
   - Partner creating or managing events, locations, or onboarding
   - Internal operator or data-quality workflow
3. Frame the kite-surfer decision being supported:
   - Where should I ride?
   - When is it worth going?
   - Is this spot safe for me today?
   - What should I bring?
   - Can I trust this information?
   - Who or what is available nearby?
4. Surface the minimum useful context:
   - User intent and skill level
   - Location, season, forecast horizon, and time sensitivity
   - Spot constraints, hazards, amenities, access, and local rules
   - Trust signals and source quality
   - Existing GoSalty surfaces likely involved
5. Define what success looks like in observable behavior.
6. List unknowns that matter before coding. Separate blocking questions from
   nice-to-know questions.
7. Propose a lean implementation slice, including non-goals.
8. Call out risks:
   - Safety or liability
   - Bad forecast interpretation
   - Poor personalization
   - Stale, missing, or low-trust data
   - Partner/user permission boundaries
   - Overbuilding before the discovery assumption is validated

## Expected Output

Keep the answer concise and implementation-oriented. Prefer this shape:

- Framing: one short paragraph
- Kite-surfer behavior: concrete assumptions that should influence the design
- Context to inspect: relevant app, b2b, Convex, server, or docs areas
- Acceptance questions: what must be true for the story to be ready
- Suggested first slice: smallest useful version
- Risks and non-goals: what to avoid or defer

If the user only wants a rewritten story, provide a tightened story plus
acceptance criteria. If they ask for implementation next, use the discovery
brief to guide the code changes and follow the repo's normal instructions.

## GoSalty Guardrails

- Do not modify `convex/` for b2b-only discovery work; mock data when needed.
- For Convex implementation follow-up, read
  `convex/_generated/ai/guidelines.md` before editing.
- Ask for clearance before schema migrations, deployments, destructive data
  changes, or broad refactors.
- Prefer existing app, b2b, Convex, and server patterns over introducing a new
  layer.
