# Database Rules

Apply this module when designing, reviewing, or changing database schemas, queries, indexes, permissions, or migrations. The [shared development rules](../SKILL.md) remain in effect.

## Skills and tools

Apply every matching row; skill requirements are cumulative.

| Task                                                                                     | Required skills                                         |
| ---------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| Any PostgreSQL work, including queries, data types, constraints, indexes, and extensions | `postgres`; load its task-specific references as needed |
| PostgreSQL table, schema, or data-model design                                           | `design-postgres-tables`                                |
| PostgreSQL schema changes or migrations                                                  | `postgres-database-migration`                           |
| Any Supabase work                                                                        | `supabase:supabase`                                     |
| Supabase database design or optimization                                                 | `supabase:supabase-postgres-best-practices`             |

- PostgreSQL hosted in Supabase matches both sets of rows above.
- Use the [Supabase plugin](plugin://supabase@openai-curated-remote) and its relevant MCP tools for supported Supabase operations.

For specialized PostgreSQL tasks, load the matching skills:

| Task                                        | Skills to read and apply                 |
| ------------------------------------------- | ---------------------------------------- |
| Spatial or geographic data                  | `design-postgis-tables`                  |
| Embeddings, RAG, or semantic search         | `pgvector-semantic-search`               |
| Combined keyword and semantic search        | `postgres-hybrid-text-search`            |
| TimescaleDB schema or hypertable design     | `setup-timescaledb-hypertables`          |
| Evaluating tables for hypertable conversion | `find-hypertable-candidates`             |
| Converting tables to hypertables            | `migrate-postgres-tables-to-hypertables` |

## Schema and access

- Enforce identities, uniqueness, valid states, and relationships with the database's appropriate constraints.
- Configure foreign-key deletion behavior to respect the retention boundary.
- Index actual query, queue, and relationship access paths; add indexes for demonstrated requirements.
- Separate migration privileges from runtime privileges and configure matching row-level policies where applicable.
- Provision passwords separately from migrations. Use certificate-verified encrypted connections to remote databases where supported.

## Supabase access from a backend

- In frameworks with a trusted backend, prefer connecting from that backend to Supabase PostgreSQL using a database connection string and the project's database driver or ORM. Route application data access through the backend rather than exposing application tables through the Data API by default.
- Choose a direct PostgreSQL endpoint or a Supabase pooler connection string according to the deployment's connection lifetime, network support, and driver requirements. Use bounded connection pooling; serverless deployments may require transaction pooling.
- Create a dedicated login role for the application with explicit schema usage and object grants. The runtime role must not own application objects or have administrative, schema-creation, or RLS-bypass privileges.
- Create a separate application schema, owned by the migration identity, and put application tables there instead of `public`. Keep this schema out of the Data API's exposed schemas and extra search path. Check inherited and default grants so `anon`, `authenticated`, and `PUBLIC` receive no unintended access to its objects.
- Use schema-qualified names in migrations and queries, or a deliberately configured application `search_path` for queries. Enforce user and tenant authorization in the backend and matching database policies where applicable; a shared database login does not identify the requesting user.

References: [Supabase connection methods](https://supabase.com/docs/guides/database/connecting-to-postgres), [Postgres roles](https://supabase.com/docs/guides/database/postgres/roles), [Securing your data](https://supabase.com/docs/guides/database/secure-data).

## Migrations and transactions

- Apply schema changes through migrations. Install pending migrations without resetting existing application data.
- Inspect existing rows and dependencies before destructive DDL; avoid cascading deletion beyond the intended scope.
- Save related data and processing state in one transaction so a crash cannot mark partial work complete.
- Keep network requests outside database transactions.
- Use transaction isolation or locking appropriate to concurrent operations, alongside the schema's uniqueness constraints.

## Verification

- Verify migration application, constraints, runtime-role permissions, rollback behavior, and cleanup boundaries relevant to the change. Include denied operations when testing access control.
- For the Supabase backend access model above, verify that the runtime role can perform required operations but cannot create or drop application objects, and that private tables remain inaccessible through the Data API.
- Run the project's available database security and performance checks after relevant schema changes.
