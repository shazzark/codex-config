# Docker and containerization

## Build a suitable image

Inspect the project's Node.js version, package manager, lockfile, module format, build output, and runtime entrypoint. Keep the build reproducible and match dependency installation to the lockfile. Use multi-stage builds when separating compilation/development dependencies from runtime files materially reduces image size or exposure. Keep the runtime stage limited to files and packages needed to serve the application. Use a maintained base image compatible with required native modules and platform targets; make update ownership clear.

Add a `.dockerignore` appropriate to the build context. Common exclusions include local `node_modules`, VCS metadata, test output, local secrets, and unrelated files. Do not exclude files that the build actually needs. Build context is sent to the builder, so inspect exclusions for both performance and accidental secret inclusion. Docker documents [build best practices](https://docs.docker.com/build/building/best-practices/), [multi-stage builds](https://docs.docker.com/get-started/docker-concepts/building-images/multi-stage-builds/), and [build context](https://docs.docker.com/build/concepts/context/).

## Runtime safety and resource use

- Run as a non-root user when the application and base image support it; grant only required filesystem access.
- Do not bake runtime secrets into Dockerfile `ARG`/`ENV`, image layers, labels, build logs, or copied files. Provide secrets at runtime through the platform's protected mechanism.
- Use environment variables for non-secret configuration when appropriate, validate required values on startup, and avoid putting development defaults into production.
- Set resource limits/reservations according to hosting behavior and measured needs. Understand how the runtime behaves under memory/CPU pressure; a container limit can change failure behavior.
- Define a health check that tests the relevant service readiness, but keep it bounded and inexpensive. A health check should not expose sensitive diagnostics.
- Persist only the data that must survive container replacement, using explicit named volumes or managed storage. Containers are replaceable; do not assume their writable layer is durable.
- Handle signals and graceful termination so Node.js closes listeners, database pools, workers, and telemetry exporters within the platform's termination window.

## Local Docker Compose

Use Compose to make local dependencies reproducible when it helps contributors. Keep local development configuration and production deployment configuration distinct. Use service DNS names on the Compose network rather than `localhost` between containers. Expose only ports needed by the host or other services. Use named volumes for local database state when persistence is wanted; document reset behavior before removing a volume.

`depends_on` orders container creation but does not automatically mean an application is ready. Add service health checks and use the appropriate healthy/completed dependency condition when startup sequencing matters; the application must still handle later disconnects and reconnect/fail safely. See Docker's [Compose startup order guidance](https://docs.docker.com/compose/how-tos/startup-order/).

## Node.js/TypeScript container considerations

Install from the committed lockfile using the chosen package manager's reproducible/CI mode. Ensure the production stage includes compiled TypeScript output and runtime dependencies but not development-only tooling unless required at runtime. Avoid copying local build output or secrets accidentally. Test the built image itself, not only the host-side application.

## Troubleshooting

Inspect service status, health state, logs, exit code, image/build output, effective environment names (never print secret values), port bindings, DNS/network membership, mounted paths and permissions, and resource events. Verify the app's listening interface and port inside the container. Narrow failures by separating build-time failure, process startup failure, dependency readiness, connectivity, and application health. Do not use `docker system prune`, `docker compose down -v`, or volume deletion as routine diagnostics; such actions can remove valuable data.
