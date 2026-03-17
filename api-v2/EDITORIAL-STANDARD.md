# API V2 Editorial Standard

This standard defines how EdgeX V2 API documentation should be written, reviewed, and cleaned before release.

## 1. Primary Goal

The documentation must help an external integrator connect successfully with the least amount of guesswork.

A good page should be:
- Accurate against real implementation
- Consistent with the rest of the documentation set
- Easy to scan in GitBook
- Focused on integration, not on raw generated reference output

## 2. Required Page Structure

For important entry pages and high-traffic API pages, use this order:

1. What this page is for
2. Minimal runnable example
3. Common notes / common mistakes
4. Endpoint reference details
5. Schema and data model details

Schema supports the page, but should not dominate the top of the page.

## 3. Accuracy Rules

- Treat code and live response behavior as the source of truth.
- Authentication, headers, signature rules, and URL paths must match the actual implementation.
- If a field name is historically preserved by the backend, keep the field name unchanged and explain it instead of renaming it.
- If SDK behavior and older wording conflict, prefer current implementation behavior.

## 4. Consistency Rules

- Use one naming style across pages.
- Keep REST, WebSocket, and L2 signature terminology aligned.
- Use the same header names everywhere.
- Use the same response wording everywhere.
- Distinguish clearly between platform API scope, SDK coverage, and current GitBook page coverage.

## 5. Content Selection Rules

Keep:
- Minimal examples that help a user make a real request
- Necessary schema summaries
- Explanations for easy-to-miss fields or event behavior
- Compatibility notes that avoid integration mistakes

Remove or avoid:
- Large implementation snippets that do not materially improve integration success
- Repetitive sample code in multiple languages when one minimal example is enough
- Decorative footers like manual `Last Updated` or `API Version` lines when they are not automatically maintained
- Generated wording such as `Default response` when a clearer phrase exists
- Sections that read like internal migration checklists or TODO notes

## 6. WebSocket Rules

- Keep a minimal connection example, not a full client implementation tutorial.
- Document envelope schema, heartbeat schema, error schema, and per-channel payload schema.
- Explain how update types differ when needed (for example, depth snapshot vs incremental changes).
- Prefer schema summaries over long SDK-like example programs.

## 7. GitBook Presentation Rules

- Avoid wide tables when a list is easier to read.
- Avoid long code blocks unless they carry unique value.
- Use concise section titles.
- Do not leave pages with obvious generated-reference noise at the top.

## 8. Release Bar

A page is ready for release when:
- A new integrator can tell when to read it
- The page includes enough example material to make a correct request or handle a correct payload
- The terminology matches neighboring pages
- The page does not contain obvious stale or low-value generated content
