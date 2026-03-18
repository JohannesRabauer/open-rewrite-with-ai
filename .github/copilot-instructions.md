# Project Guidelines

## Architecture
- This workspace is a modernization monorepo with four distinct roles:
- `legacy-banking/`: the legacy target application being analyzed and transformed.
- `rewrite-recipe-legacy-banking/`: the main recipe authoring repo for this workspace; prefer making modernization changes here unless the task explicitly targets the app or the framework.
- `rewrite/`: upstream OpenRewrite framework and reference implementation; use it as the source of proven recipe patterns, visitor usage, and tests.
- `open-rewrite-with-ai/`: planning and workflow notes that explain the AI-assisted modernization loop.
- Treat `legacy-banking/` as the specimen and acceptance target, not the default place to implement modernization logic.
- When changing recipe behavior, compare against existing patterns in `rewrite/` before inventing new abstractions.

## Build and Test
- `legacy-banking/`
- Backend build: `cd legacy-banking/banking-service && mvn clean package -DskipTests`
- Client build: `cd legacy-banking/banking-client && mvn clean package -DskipTests`
- Run stack: `cd legacy-banking && docker compose up -d --build`
- Check service logs: `cd legacy-banking && docker compose logs banking-service --tail=200`
- `rewrite-recipe-legacy-banking/`
- Preferred build: `cd rewrite-recipe-legacy-banking && ./gradlew build`
- Tests only: `cd rewrite-recipe-legacy-banking && ./gradlew test`
- Publish locally for integration testing: `cd rewrite-recipe-legacy-banking && ./gradlew publishToMavenLocal`
- Maven alternative: `cd rewrite-recipe-legacy-banking && ./mvnw install`
- `rewrite/`
- Full build: `cd rewrite && ./gradlew build`
- Compile without tests: `cd rewrite && ./gradlew assemble`
- Module test: `cd rewrite && ./gradlew :rewrite-java:test`
- Filtered test: `cd rewrite && ./gradlew :rewrite-java:test --tests "org.openrewrite.java.ChangeTypeTest"`
- For RPC-backed modules in `rewrite-python` or `rewrite-javascript`, always run tests with explicit timeouts and prefer narrow test selection.

## Conventions
- Favor minimal, composable recipes over a single oversized recipe unless the task explicitly requires orchestration in one entry point.
- In `rewrite-recipe-legacy-banking/`, keep new recipes aligned with OpenRewrite conventions:
- Reuse existing recipes where possible and add custom recipes only for gaps specific to `legacy-banking/`.
- Follow existing recipe naming, `@Option` metadata, and `RewriteTest` patterns from `rewrite/` and the recipe starter examples.
- Use declarative YAML recipes for straightforward search/replace or recipe composition; use imperative Java recipes when AST-aware logic or conditional transforms are required.
- In OpenRewrite visitors, never cast LST nodes without checking `instanceof` first, and always return modified copies rather than mutating in place.
- Keep changes scoped to the repo you are working in. Do not copy framework code from `rewrite/` into `rewrite-recipe-legacy-banking/`.
- For `legacy-banking/`, preserve the legacy architecture unless a task explicitly says to modernize the runtime:
- SOAP/JAX-WS stays SOAP unless requested otherwise.
- Spring XML wiring and direct JDBC are intentional characteristics of the sample app.
- Keep `db/init.sql` seed data stable unless the task specifically targets demo data.
- Use forward slashes in commands and paths when possible; this avoids Windows path issues in Gradle and tool output.

## Workflow
- Start recipe work in `rewrite-recipe-legacy-banking/`, then validate the result against `legacy-banking/`.
- Use `legacy-banking/plans/` as the source of truth for the legacy system structure before editing the app or writing a migration recipe.
- When you need proven implementation patterns, inspect matching code in `rewrite/` first.
- Consult repo-local guidance when working deeply in a subproject:
- `legacy-banking/.github/copilot-instructions.md`
- `rewrite/CLAUDE.md`
- `rewrite-recipe-legacy-banking/CLAUDE.md`
- `.claude/skills/` directories in `legacy-banking/`, `rewrite/`, and `open-rewrite-with-ai/`

## Pitfalls
- `legacy-banking/` uses old infrastructure on purpose: PostgreSQL 9.3, Tomcat 7, Spring 3.2.x, SOAP, Swing. Do not “fix” these by default.
- `rewrite/` is a large multi-module build. Avoid full-suite test runs when a module-level or class-level run is enough.
- Tests in RPC-based language modules can hang indefinitely if communication breaks; use explicit timeouts.
- `rewrite-recipe-legacy-banking/` mixes Gradle and Maven wrappers. Prefer Gradle unless the task is specifically about Maven plugin behavior.
- Plugin versions such as `latest.release` can introduce drift. If a build issue appears version-related, inspect the build files before changing recipe code.