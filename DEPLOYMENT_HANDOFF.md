# Surat OMS production handoff

This repository contains application source and database migrations only. It does **not** contain the local PostgreSQL database, dealers, products, orders, dispatches, users, sessions, passwords, or `.env.local` credentials.

## Create a clean production installation

1. Create a brand-new PostgreSQL database dedicated only to **Matariya Surat OMS**.
2. Configure these values privately in the hosting provider; never commit them to GitHub:
   - `DATABASE_URL`
   - `DIRECT_URL` (recommended for migrations)
   - `NEXT_PUBLIC_DATA_PROVIDER=postgres`
   - `BETTER_AUTH_SECRET` (a new random value of at least 32 characters)
   - `BETTER_AUTH_URL` (the final HTTPS site URL)
3. Run `npm install` and `npm run build`. The build applies all committed migrations.
4. Do **not** run `npm run db:seed` or `npm run db:seed-review-orders` for production. Those commands create demonstration data.
5. Create the first administrator by temporarily setting `FIRST_ADMIN_NAME`, `FIRST_ADMIN_EMAIL`, `FIRST_ADMIN_USERNAME`, and `FIRST_ADMIN_PASSWORD`, then run `npm run auth:bootstrap-admin` (or allow the first production build to run it).
6. Remove all four `FIRST_ADMIN_*` values from the hosting environment immediately after the administrator is created. Further users can be managed inside **Settings → Users**.

## Important separation rule

Do not connect this application to the Outstation OMS database or the local review database. Production must have its own PostgreSQL database, authentication secret, users, numbering, orders, dispatches, and history.

## Verification before launch

Run:

```text
npm run db:validate
npm run typecheck
npm run lint
npm run test:database-architecture
npm run test:security
```

After deployment, confirm that the new database has no dealers, products, orders, dispatches, or users other than the deliberately created first administrator.
