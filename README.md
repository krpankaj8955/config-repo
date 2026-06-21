# config-repo

Centralized configuration served by the **config-server** (Spring Cloud Config).
Each Spring Boot backend pulls its properties from here at startup, by
`{spring.application.name}-{profile}`:

| App (spring.application.name) | files |
|---|---|
| `ml-from-scratch` (MLEngineering backend) | `ml-from-scratch.properties`, `-dev`, `-prod` |
| `lottery-backend` (lottery-system backend) | `lottery-backend.properties`, `-dev`, `-prod` |

## Rules
- **No plaintext secrets.** Secrets stay as `${ENV_VAR}` placeholders; the *app*
  resolves them from its own environment (server-side `.env`). For secrets that
  must live here, use Config Server encryption: store as `{cipher}...`.
- `application.properties`/`.yml` = defaults shared by every app.
- `*-dev` = local development; `*-prod` = production (env-driven, fail-fast).

Frontends (Next.js / Vite) do **not** read from here — their build-time vars
(`VITE_*`, API base URL) come from GitHub Actions Variables/Secrets.
