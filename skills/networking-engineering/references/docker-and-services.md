# Docker and database/cache connectivity

## Docker and Compose networking

Containers have separate network namespaces. From a container, `localhost` points to that same container. In Compose, services on the same project network can usually reach one another by service name and the container's listening port (for example, `postgres:5432`). Port publishing (`HOST:CONTAINER`) exposes a container port through the host; it is not needed for container-to-container traffic on a shared network. Binding a published port to all host interfaces may expose it beyond the local machine; restrict host binding when only local development needs access.

User-defined bridge networks provide isolation and service-name resolution. Compose creates a default service network unless configuration changes it. Containers receive dynamic addresses, so use service DNS names rather than hard-coded container IPs. `host.docker.internal` or `host-gateway` may reach the host, with platform-specific behavior; inspect the current Docker Desktop/Engine setup. See Docker's [bridge network](https://docs.docker.com/engine/network/drivers/bridge/), [Compose networking](https://docs.docker.com/compose/how-tos/networking/), and [port publishing](https://docs.docker.com/engine/network/port-publishing/) docs.

`depends_on` can order service startup, but application readiness and later recovery still need handling. Add health conditions only when startup sequencing benefits; applications should reconnect safely after a dependency restarts. Don't change host networking, publish database ports, or add broad networks as a shortcut before identifying the caller and needed path.

## PostgreSQL, MongoDB, and Redis

- **PostgreSQL:** from a sibling container, use the Compose service DNS name and internal port (commonly `5432`), not `localhost` or the host-published port. From the host, use the published host address/port. Confirm database/user, TLS mode, network allowlists, and pool limits without printing credentials. See the [Docker PostgreSQL connectivity guide](https://docs.docker.com/guides/postgresql/immediate-setup-and-data-persistence/).
- **MongoDB:** verify the connection URI's host list, port, authentication database/credentials presence, TLS requirements, and whether replica-set discovery returns addresses routable from the client network. A seed host reachable at first connection does not guarantee all discovered members are reachable.
- **Redis:** verify the service name/port (commonly `6379`), authentication/TLS settings, database/client config, and whether the client connects to the intended cache/queue instance. Do not expose Redis publicly to test a private service path.

Managed services may require private peering, allowlists, egress routing, DNS resolvers, TLS, or region alignment. Validate from the application's actual runtime network; a developer laptop's successful connection proves a different path. Leave schema, credentials, persistence, and cache correctness to `$database-engineering`, `$security-engineering`, and `$caching-engineering`.

## Safe connectivity checks

Start with read-only inspection: `docker compose ps`, `docker compose logs --tail 100 <service>`, `docker network inspect <network>`, and `docker compose exec <app> ...` only for an authorized local/isolated project. Avoid `down -v`, pruning, volume deletion, restarting shared services, printing environment values, or broad port scans. `docker compose exec` may require a tool inside the container; use an existing shell/client and do not install packages as a diagnostic without approval.
