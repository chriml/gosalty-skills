# Partner Tools Reference

## Where Tools Live

- Event and knowledge tools: `convex/db/partners/tools/partnerTools.ts`
- Location and onboarding tools: `convex/db/partners/tools/partnerLocationTools.ts`
- Shared selectors and confirmation helpers:
  `convex/db/partners/tools/shared.ts`
- Parsing, search, and Google/spot resolution helpers:
  `convex/db/partners/tools/utils.ts`
- Tool registration: `convex/agents/chatAgent/chatAgent.ts`

## Tool Contract

Tools return a `ToolResult` through helpers:

- `missingInformation(message, fields, options?)`
- `completed(message, data?)`
- `requireConfirmation(args, message, data?)`

Follow that shape for new tools so the chat UI can handle clarification,
confirmation, and success consistently.

## Confirmation Rule

Any tool that mutates partner dashboard state must accept:

```ts
confirmed?: boolean
```

If `confirmed` is not true, return `requireConfirmation(...)` with a concrete
summary of what will change. Only run the mutation after explicit confirmation.

## Selectors

Use these optional selector names when possible:

- `partner`: partner name or id
- `location`: partner location name or id
- `event`: event title or id
- `spot`: spot name or id
- `knowledgeEntry`: knowledge title or id
- `placeId`: Google place id selected by the app/B2B UI

Resolve selectors with the existing shared helpers instead of duplicating lookup
logic.

## Event Tool Behavior

`createPartnerEventTool` already:

- Resolves partner and location access.
- Requires title, description, start, and end.
- Parses `startAt` and `endAt` from ISO strings or epoch milliseconds.
- Resolves event location from fallback location, spot, or Google place.
- Defaults `status` to `published` and `type` to `special`.
- Calls `api.db.events.mutations.createPartnerEvent`.

`updatePartnerEventTool` keeps existing values for omitted fields and calls
`api.db.events.mutations.updatePartnerEvent`.

`deletePartnerEventTool` resolves the event and calls
`api.db.events.mutations.deletePartnerEvent`.

## Adding New Tools

- Keep input schemas small and explicit with zod.
- Use existing Convex mutations/actions; do not patch tables from tools unless a
  dedicated function does not exist.
- Avoid optional fields that silently erase existing data.
- Include ids in success payloads.
- Add the new tool to `chatAgent` only after it is implemented.
