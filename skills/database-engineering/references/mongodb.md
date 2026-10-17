# MongoDB

Model documents around access patterns, bounded aggregate boundaries, and consistency needs. Inspect the deployed MongoDB version, topology, schema validation, and Mongoose/native-driver conventions before changing behavior.

## Documents and relationships

- Collections hold documents, which can contain nested objects and arrays. Embed bounded data that is normally read and updated with its parent. Reference independently growing, shared, or separately permissioned data.
- Do not embed unbounded histories or arrays that grow without a practical limit. They can make documents large and updates expensive. Use a separate collection and query strategy for independently growing data.
- Duplicating a value can make reads simpler, but it creates an update obligation. Record which copy is authoritative and how other copies are refreshed or reconciled.
- Use stable identifiers and deliberate field naming. Treat field types and null/missing behavior as part of the data contract; absent and explicitly null fields may have different meanings.
- Use collection schema validation for important shape and type invariants where appropriate. Mongoose schemas help enforce application behavior but do not alone guarantee that all writers, scripts, or other services obey the schema.

## Indexes and queries

- Derive indexes from real filters, sort order, joins/lookups, and uniqueness requirements. Compound-index order matters. Indexes cost memory/storage and make writes more expensive.
- Use `explain` and query profiling with representative data. Inspect examined documents versus returned documents, sorts, index use, and aggregation stages. Do not add an index merely because a field exists.
- Bound array and result sizes, project only needed fields, and page large collections deterministically. Avoid deep skip/offset scans for large changing result sets when range-based pagination is suitable.
- Design aggregation pipelines so selective filters and projections occur early when valid, and understand memory, disk-use, and cardinality behavior for the deployed version.

## Atomicity and transactions

- A single-document write is atomic. Prefer a document boundary that lets one invariant be updated atomically when that fits the domain.
- Multi-document transactions are available in supported replica set and sharded deployments, but add coordination and operational cost. Confirm topology and version support before designing around them.
- Use transactions when an invariant truly spans documents and cannot be modeled more simply. Keep them bounded and make retry behavior safe; transient transaction retries must not duplicate external side effects.
- Use unique indexes and conditional updates to protect against concurrent duplicate creation or stale writes. Application-side “check then insert” by itself can race.

## Mongoose and native driver

- **Mongoose:** Keep schemas, validation, indexes, models, and migrations aligned with the actual collection. Understand middleware and casting behavior on the specific query/update APIs in use. Update validators and document-save behavior are not identical across all update paths; do not assume a Mongoose schema validates every database write.
- **Native driver:** Keep collection access and serialization explicit. Use BSON types consistently, parameterize values through driver operations, and centralize client lifecycle without hiding meaningful query behavior.
- In either case, create indexes through a controlled deployment/migration process rather than relying on every application process to perform expensive production index builds at startup.

## Data lifecycle and tests

Make backfills resumable, bounded, observable, and safe with old and new application versions during rollout. Test validation, unique indexes, atomic updates, and transaction behavior against an isolated deployment with matching topology when needed. Mocks cannot prove MongoDB query semantics or index behavior.
