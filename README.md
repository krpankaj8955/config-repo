# config-repo

Centralized configuration served by the **config-server** (Spring Cloud Config).
Each Spring Boot backend pulls its properties from here at startup.

**One folder per source repo** (the config-server searches every subfolder):

```
config-repo/
├── lottery-system/        # lottery-system repo
│   ├── lottery-backend.properties        (shared)
│   ├── lottery-backend-dev.properties
│   └── lottery-backend-prod.properties
└── MLEngineering/         # MLEngineering repo
    ├── ml-from-scratch.properties         (shared)
    ├── ml-from-scratch-dev.properties
    └── ml-from-scratch-prod.properties
```

Files are named `{spring.application.name}-{profile}.properties` (that's how the
server matches an app to its config); the **folder** is named after the repo.
To add a repo, drop in a new folder with its `{app}-{profile}` files.

## Rules
- **No plaintext secrets.** Secrets stay as `${ENV_VAR}` placeholders; the *app*
  resolves them from its own environment (server-side `.env`). For secrets that
  must live here, use Config Server encryption: store as `{cipher}...`.
- `application.properties`/`.yml` = defaults shared by every app.
- `*-dev` = local development; `*-prod` = production (env-driven, fail-fast).

Frontends (Next.js / Vite) do **not** read from here — their build-time vars
(`VITE_*`, API base URL) come from GitHub Actions Variables/Secrets.
