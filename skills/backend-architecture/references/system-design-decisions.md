# System-design decisions

Use this reference when a task needs a system-level choice beyond the organization of one feature. It supplements the existing backend structure and implementation guidance; it does not prescribe a standard topology.

## Start with requirements and constraints

Separate what the system must do from how well or under what conditions it must do it:

- **Functional requirements** describe behavior: users, workflows, data, integrations, and outputs.
- **Quality attributes** describe qualities such as latency, availability, consistency, security, privacy, recoverability, throughput, and ease of change.
- **Constraints** include team size and skills, delivery deadlines, existing systems, regulatory obligations, budget, hosting, and operational capacity.

Ask which requirements are measurable and which are firm. State unknowns and assumptions rather than inventing traffic numbers, service-level objectives, or future growth. Rank the few qualities that drive the decision; trying to maximize every quality at once often adds needless cost and complexity.

## Choose boundaries and architecture shape

Start with the smallest deployable shape that supports clear ownership and change:

- A **modular monolith** keeps capabilities in one deployable application while making their code and data ownership explicit. It is often the best starting point for a small team, an evolving product, or tightly related workflows.
- Separate a process or service when a concrete need exists, such as independent scaling, resource isolation, security boundary, deployment cadence, team ownership, or a failure mode that cannot be contained in-process. Compare the operational cost of networking, deployment, monitoring, data consistency, and on-call work with the benefit.
- Prefer a **layered** arrangement when separating transport, application policy, and infrastructure makes responsibilities easier to understand. Use **hexagonal** or ports-and-adapters boundaries when isolating policy from external systems has a practical benefit, such as multiple adapters or meaningful independent tests. These are tools, not mandatory layers or folder names.
- Define service/module ownership around business capability and data invariants. A boundary should have a clear owner, a small public contract, and limited reasons to change. Avoid multiple services writing the same authoritative records without an explicit consistency and ownership plan.

Do not split a monolith because it is growing in lines of code, because a team expects future scale, or because a technology makes splitting easy. First consider improving module boundaries and measuring the real bottleneck. A modular monolith can later be separated along an established boundary if requirements justify it.

## Compare trade-offs and record decisions

For a material decision, compare a small number of credible options against the requirements that matter. State:

1. The problem and constraints.
2. Options considered, including keeping the current design.
3. The important benefits, costs, failure modes, and operational responsibilities of each.
4. The chosen option, assumptions, and evidence behind it.
5. Consequences, migration or rollback considerations, and what signal would justify revisiting it.

Use an architecture decision record (ADR) when the choice has lasting consequences, crosses team or service boundaries, or would otherwise be repeatedly reconsidered. Keep it short and store it where the project keeps design decisions. Do not create an ADR for every routine implementation choice or add a new documentation system just for this skill.

Prefer incremental evolution: preserve working contracts, introduce compatible boundaries, observe behavior, and remove old paths only after the new path is verified and safe to retire. Avoid speculative platform work and “future-proofing” without a credible requirement.

## Scale and performance from evidence

First identify the user-visible target, representative workload, current latency/throughput/error rate, and the constrained resource. Measure before and after a change using comparable conditions; do not promise scale from a diagram alone.

- **Vertical scaling** gives a process or database more CPU, memory, or I/O capacity. It is often the simplest first response, within platform and cost limits.
- **Horizontal scaling** adds instances. It can improve capacity or availability, but requires routing/load balancing and careful handling of shared state, sessions, rate limits, database pools, background work, and deployment drains.
- Stateless request handling makes replica scaling easier, but do not force all state into a remote service without considering latency, availability, and operating cost.
- For a database bottleneck, inspect workload and query plans first. Consider indexes, query shape, pool limits, read replicas, partitioning, or sharding only when the measured bottleneck and consistency needs support them. Detailed model/query choices belong to `$database-engineering`.
- Add a cache only for demonstrated repeated work or a clear latency/load need, with an explicit freshness and invalidation contract. See `$caching-engineering` for cache design.
- Use a queue/worker when buffering, retrying, decoupling, or independent scaling provides a concrete benefit. Bound queue growth and worker concurrency by downstream capacity; queues move load in time but do not remove it.
- Plan rough capacity from request/job rates, work per operation, concurrency, payload size, connection limits, and peak behavior. Include headroom and a way to observe saturation. Avoid precise capacity claims without workload data.

Backpressure means slowing or rejecting incoming work when a downstream component cannot safely keep up. Prefer bounded queues, concurrency limits, admission control, and clear overload responses over unbounded buffering or retries that amplify load.

## Distributed communication and consistency

Use synchronous request/response when the caller needs an immediate result and the dependency can meet its latency and availability needs. Use asynchronous events or queues when buffering, independent processing, or temporal decoupling matters and delayed completion is acceptable. Keep the interaction synchronous when introducing eventual consistency would confuse users or weaken an invariant.

For asynchronous flows, define ownership, message schema/version, delivery expectations, ordering needs, duplicate handling, retry limits, poison-message handling, and how operators find and recover stuck work. Assume at-least-once delivery unless the specific system proves otherwise; make effects idempotent where duplicates matter. Do not claim exactly-once business effects merely because a broker advertises exactly-once delivery within a narrower boundary.

Transactions generally protect invariants inside one database boundary. Across services or an external provider, prefer explicit state transitions, idempotency keys, outbox/inbox or reconciliation patterns where warranted, and compensating actions for reversible business steps. A saga is a sequence of local transactions and compensations; it adds coordination and does not provide a global rollback. Choose eventual consistency only when the product can explain and tolerate the delay or temporary disagreement.

Treat timeouts, bounded retries with jitter, circuit breakers, bulkheads, and graceful degradation as responses to a specific failure mode, not a collection to add everywhere. A circuit breaker temporarily stops calls to a failing dependency; it needs a recovery policy and must not conceal permanent errors. Avoid retry storms and ensure that timeout does not imply a remote write failed to commit.

For WebSockets, external APIs, DNS, proxies, and transport-level investigation, use `$networking-engineering`. For observed faults, use `$debugging-workflow` to establish the cause before redesigning the system.

## Reliability and operational fit

Translate availability or recovery goals into observable behavior: what can fail, what users see, what data can be lost, how quickly service must recover, and who responds. Do not assign an availability target or recovery-time objective without product/operations input.

Choose the smallest reliability mechanisms that address the required failure modes. Health/readiness signals, timeouts, resource limits, graceful shutdown, backups, restore tests, deployment verification, and a rollback or forward-repair plan may matter more than adding redundant services. Redundancy is useful only when its failure domains, dependencies, and recovery behavior are understood.

Every independently deployed component adds build/release configuration, access controls, secrets, network paths, alerts, dashboards, capacity decisions, and on-call ownership. Include these operating costs in architecture comparisons. Managed services can reduce maintenance work but still require configuration, cost controls, access policy, recovery planning, and provider failure assumptions. Self-host only when the control or cost benefit justifies ownership of upgrades, availability, backups, and incident response. `$devops-infrastructure` owns detailed deployment and operational procedures.

## Data, APIs, security, and cost boundaries

- Choose data stores from access patterns, integrity, consistency, durability, recovery, team capability, and operating needs. Keep each fact's authoritative owner clear. Do not add a database technology simply to match a pattern.
- Treat API and event schemas as contracts. Prefer compatible additions, define deprecation and consumer migration, and consider mixed old/new versions during rollout. Keep schema-evolution implementation with `$database-engineering` and API/module contracts with the existing backend guidance.
- Identify trust boundaries and least-privilege access between users, services, workers, databases, and providers. Include data exposure, secret distribution, and failure-open versus failure-closed behavior in choices. `$security-engineering` owns detailed threat modeling and controls.
- Compare direct costs (compute, storage, network, managed-service fees) and operational costs (engineering time, on-call, upgrades, recovery, incident impact). Optimize only with credible usage/cost data; favor the option the team can safely operate.

## Review questions

- Which requirement or measured problem makes this architectural choice necessary now?
- Could clearer modules, a query improvement, or a vertical resource increase solve it more simply?
- Who owns the data and invariant, and what happens during a partial failure or mixed-version rollout?
- What new operational work, cost, security boundary, and recovery path does the option add?
- What evidence will show that it helps, and what conditions would make us revisit it?
