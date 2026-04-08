# API Documentation

Generate API documentation from code.

## Process

1. Scan route files for endpoint definitions
2. Extract: method, path, parameters, request/response schemas
3. Cross-reference with Zod schemas for type information
4. Generate markdown documentation
5. Include examples for each endpoint

## Output

`docs/api.md` with sections per endpoint:
- Method + Path
- Description
- Request (params, query, body with types)
- Response (status codes, body with types)
- Example request/response
