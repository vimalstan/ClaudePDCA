# API Boundary & Type Contract Lens

- Is `as` used to cast API response data or external payloads? (Each `as` is an untested assumption — use Zod validation or type guards)
- Do frontend TypeScript interfaces match the actual backend output shape?
- Are conditionally-rendered components tested with data that triggers the condition?
- For multi-stage data pipelines, is there at least one test exercising the full chain with real data?
- Is `Record<string, unknown>` destructured with `as` casts? (Zero type safety despite appearing typed)
- Are request/response types derived from shared Zod schemas?
