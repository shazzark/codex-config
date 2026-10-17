# Scenario Validation

These are structured design exercises against the skill and its references, not executable database tests or claims that an application or production system was changed. No database credentials or live database were used.

## 1. PostgreSQL schema for e-commerce orders

**Scenario assumptions:** A customer can place an order with multiple products. The system must preserve what was purchased and its price at checkout, prevent invalid quantities, and avoid duplicate order creation on client retry.

**Guidance outcome:** Use `customers`, `products`, `orders`, and `order_items` tables. Give each a primary key; link orders to customers and items to orders/products with foreign keys. Add positive-quantity and nonnegative-price constraints, and store the agreed currency/price precision. Store a snapshot of product name/SKU and unit price in order items so later catalog edits do not rewrite the purchase record. Add a unique idempotency key scoped to the customer/request for retry protection. Create the order and its items in one bounded transaction. Add indexes only for stated access patterns, such as a customer's recent orders and order-item lookup by order; validate with actual query plans and data.

**Evaluation:** Relationships and historical price semantics are explicit; integrity uses constraints; transaction scope follows the order invariant. The design avoids speculative indexes and unasked inventory/payment complexity.

## 2. MongoDB collections for a content management system

**Scenario assumptions:** Editors load an article with its current blocks and tags; authors and editorial history are queried independently. Revision history can grow without a fixed bound.

**Guidance outcome:** Store articles in an `articles` collection with bounded current content blocks and stable author identifiers. Keep authors in a separate collection. Embed a small bounded tag snapshot if it is useful for rendering, while defining the source of truth. Put unbounded revisions in a separate `article_revisions` collection keyed by article and revision time/number. Add a unique revision key and indexes based on actual slug lookup, publication listings, and revision retrieval. Use validation and atomic conditional updates for publish state/version. Confirm that the deployed topology supports any proposed multi-document transaction before relying on one.

**Evaluation:** Embedding follows bounded co-read content; independently growing history is referenced. Copies and query indexes have stated purposes, with no assumption MongoDB schema validation applies to every writer.

## 3. MySQL query with performance problems

**Scenario:** An orders page loads 25 orders but then performs a separate item and customer query for every order; its main query also filters by customer and sorts by creation time.

**Guidance outcome:** First capture the exact SQL, schema, row counts/distribution, indexes, parameters, and explain/profiling evidence. Replace the per-order lookups with a bounded join or a small number of batched queries, selecting only required columns. Check whether a composite index on customer and creation-time/order key matches the filter and deterministic ordering; confirm benefit and write cost with the plan and representative data. Verify the actual storage engine, version, collation, and SQL mode before assuming behavior.

**Evaluation:** Identifies N+1 as the observed likely issue in the scenario, but requires plan evidence before prescribing an index. Does not invent a measured speedup or claim the query is fixed.

## 4. Redis cache and invalidation

**Scenario assumptions:** Product detail reads are frequent, PostgreSQL is authoritative, and an edit should become visible promptly. A short stale window after a failed invalidation is unacceptable for a small set of safety-critical fields.

**Guidance outcome:** Use cache-aside for noncritical detail fields with versioned keys and a bounded TTL. After a committed product update, invalidate/update the cache; if reliable invalidation must survive process failure, record an outbox event with the database write and retry an idempotent invalidator. For safety-critical fields, read the authoritative database or use a version check rather than relying on a TTL cache. Define fallback and load protection during Redis failure; monitor hit rate and invalidation lag.

**Evaluation:** The source of truth and stale-data tolerance determine the pattern. It avoids claiming TTL guarantees freshness and does not add an outbox unless reliability requires it.

## 5. Safe migration for a populated database

**Scenario:** A populated `users` table needs a new required `display_name` derived from existing fields, while old and new application instances may run during a rolling deployment.

**Guidance outcome:** Inspect actual schema, migration history, query writers, row counts, lock behavior, and approved environment. Add the column in a backward-compatible form first. Deploy code that can read old and new rows and writes the new field. Backfill in bounded resumable batches, monitor load and progress, verify null counts and derived values, then enforce non-null in a later migration compatible with the engine/version. Remove old fields only after all consumers have moved. Rehearse on representative isolated data and define forward repair/restore options; do not run this in production without explicit authorization.

**Evaluation:** Preserves data and mixed-version compatibility, separates schema and backfill steps, and treats rollback and production execution as explicit operational decisions.

## Results and limitations

All five design exercises pass the checks for data ownership, relevant constraints, access-pattern-based indexes, correct consistency boundaries, and proportionate complexity. They are qualitative design reviews. No SQL was executed, no plans were measured, and no migration, cache, backup, or restore was tested against a database. Exact SQL, index choices, engine behavior, and rollout safety require the target schema, version, configuration, representative data, and isolated verification.
