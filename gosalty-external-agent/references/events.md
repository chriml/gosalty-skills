# Events Reference

## Existing Entry Points

Partner dashboard events:

- Create: `api.db.events.mutations.createPartnerEvent`
- Update: `api.db.events.mutations.updatePartnerEvent`
- Delete: `api.db.events.mutations.deletePartnerEvent`
- List: `api.db.events.queries.getPartnerEvents`
- Harvest from nearby web sources: `api.db.events.actions.harvestSpotEvents`

Community/app-user events:

- Create: `api.db.events.mutations.submitUserEvent`
- Update: `api.db.events.mutations.updateUserEvent`
- Join/leave: `api.db.events.mutations.setEventJoinState`
- Bookmark/unbookmark: `api.db.events.mutations.setEventBookmarkState`
- Detail/read feed: `api.db.events.queries.getEventById`,
  `getSpotEventsPage`, `getSpotEventsFeed`

## Event Fields

Required for partner event creation:

```ts
{
  locationId: Id<"partnerLocations">,
  title: string,
  description: string,
  start: number,
  end: number,
  locationName: string,
  latitude: number,
  longitude: number,
  type: "special" | "deal" | "party" | "concert" | "festival" | "workshop" | "sports" | "community",
  status?: "draft" | "published" | "cancelled",
}
```

Common optional fields:

```ts
{
  heroImageUrls?: string[],
  link?: string,
  maxParticipants?: number,
  requirements?: string,
  agendaItems?: string[],
  includes?: string[],
  excludes?: string[],
  languageCodes?: string[],
  visibilityRadiusKm?: number,
  difficulty?: string,
  cost?: number,
}
```

Community event creation uses the same event details plus:

```ts
{
  locationSource: "current_location" | "spot" | "google_location",
  spotId?: Id<"spots">,
}
```

## Status And Type Rules

- Event types live in `convex/db/events/constants.ts` as `EVENT_TYPES`.
- Event statuses are `draft`, `published`, and `cancelled`.
- Missing or unknown status normalizes to `published`.
- Published or cancelled partner events cannot be changed back to `draft`.
- Community events created by users are stored as `published`.

## Location Rules

- Partner events normally fall back to the partner location.
- Tools can also resolve event locations from a spot or Google place.
- Direct mutation calls must supply numeric latitude and longitude.
- Event coordinates are stored through `insertEventWithCoordinates` or
  `replaceEventCoordinates`; do not write only the `events` table when the event
  should appear in map/feed discovery.

## Agent Behavior

- Ask for missing title, description, start, and end before creating.
- Parse natural-language dates to exact timestamps before mutation.
- Ask for confirmation before create, update, delete, publish, cancel, or
  location changes.
- Prefer returning a short message plus ids: `{ eventId, locationId }`.
- When selecting an existing event, resolve by id first, then title.
