## Summary
- Consolidated Chapter 1: domain context, theoretical basis, competitor analysis, As-Is and To-Be.
- Added 8 Core Features and 12 measurable NFRs with acceptance criteria.
- Updated Docs-as-Code navigation and AI usage disclosure.

## Engineering constraints
- API target: p95 <= 500 ms at 50 RPS on the test stand.
- Ingestion target: >= 10 normalized documents/s, excluding external API wait time.
- Messaging: RabbitMQ + Transactional Outbox; target p95 processing <= 5 s.
- Data: PostgreSQL, Neo4j and Redis; HTTPS/TLS 1.2+ for protected API traffic.

## Validation
- NFR values are design targets and require load/integration testing.
- Do not merge before supervisor review.
