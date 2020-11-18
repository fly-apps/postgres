# Postgres on Fly

1. `flyctl apps create`
2. `flyctl secrets set POSTGRES_PASSWORD=<SUPER-SEKRIT>`
3. `flyctl deploy`

## Connecting to postgres

1. For External connections, use `<app-name>.fly.dev:10000`

   > May not work with our TLS handler, needs testing
2. Apps on Fly Private network: `global.<app-name>.internal:5432`

   > No TLS required because Fly Private networks are encrypted