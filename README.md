# Repro steps

These are reproduction steps for this issue: https://github.com/prisma/prisma/issues/5340

1. Make sure your deps are installed: `pnpm install`
2. Set `DATABASE_URL` to MySQL DB URL
3. Run initial migration: `pnpx prisma migrate dev --name init`

Migration should succeed but fails with following error:

```
jkim@dev-jkim prisma-issue-5340 $ pnpx prisma migrate dev --name init
Environment variables loaded from .env
Prisma schema loaded from prisma/schema.prisma
Datasource "db": MySQL database "dev" at "127.0.0.1:3306"

The following migration(s) have been created and applied from new schema changes:

migrations/
  └─ 20210630174101_init/
      └─ migration.sql

      Your database is now in sync with your schema.

      Running generate... (Use --skip-generate to skip the generators)

      > prisma@2.26.0 preinstall /home/jkim/code/prisma-issue-5340/node_modules/prisma
      > node scripts/preinstall-entry.js


      > prisma@2.26.0 install /home/jkim/code/prisma-issue-5340/node_modules/prisma
      > node scripts/install-entry.js


      > @prisma/engines@2.26.0-23.9b816b3aa13cc270074f172f30d6eda8a8ce867d postinstall /home/jkim/code/prisma-issue-5340/node_modules/@prisma/engines
      > node download/index.js

      npm notice save prisma is being moved from dependencies to devDependencies
      npm notice created a lockfile as package-lock.json. You should commit this file.
      npm WARN prisma-issue-5340@1.0.0 No description
      npm WARN prisma-issue-5340@1.0.0 No repository field.

      + prisma@2.26.0
      added 1 package from 1 contributor and updated 1 package in 1.743s

      > @prisma/client@2.26.0 postinstall /home/jkim/code/prisma-issue-5340/node_modules/@prisma/client
      > node scripts/postinstall.js

      npm WARN prisma-issue-5340@1.0.0 No description
      npm WARN prisma-issue-5340@1.0.0 No repository field.

      + @prisma/client@2.26.0
      added 2 packages from 1 contributor in 2.94s
      Error: Could not resolve @prisma/client despite the installation that we just tried.
      Please try to install it by hand with npm install @prisma/client and rerun prisma generate 🙏.
```

Prisma still appears to be running `npm install` instead of `pnpm install`. Observe that it is creating a new npm lockfile instead of editing the pnpm lockfile.

# Environment

This is the environment I used to repro this issue.

Node: 12.21.0

PNPM: 5.4.12

OS: Ubuntu

Prisma version:
```
jkim@dev-jkim prisma-issue-5340 $ pnpx prisma --version
Environment variables loaded from .env
prisma               : 2.26.0
@prisma/client       : 2.26.0
Current platform     : debian-openssl-1.1.x
Query Engine         : query-engine 9b816b3aa13cc270074f172f30d6eda8a8ce867d (at node_modules/@prisma/engines/query-engine-debian-openssl-1.1.x)
Migration Engine     : migration-engine-cli 9b816b3aa13cc270074f172f30d6eda8a8ce867d (at node_modules/@prisma/engines/migration-engine-debian-openssl-1.1.x)
Introspection Engine : introspection-core 9b816b3aa13cc270074f172f30d6eda8a8ce867d (at node_modules/@prisma/engines/introspection-engine-debian-openssl-1.1.x)
Format Binary        : prisma-fmt 9b816b3aa13cc270074f172f30d6eda8a8ce867d (at node_modules/@prisma/engines/prisma-fmt-debian-openssl-1.1.x)
Default Engines Hash : 9b816b3aa13cc270074f172f30d6eda8a8ce867d
Studio               : 0.408.0
```


