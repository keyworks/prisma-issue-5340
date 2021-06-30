# Repro steps

These are reproduction steps for this issue: https://github.com/prisma/prisma/issues/5340

1. Make sure your deps are installed: `pnpm install`
2. Set `DATABASE_URL` to MySQL DB URL
3. Run initial migration: `pnpx prisma migrate dev --name init`

Migration succeeds but Prisma still appears to be running `npm install` and creating an npm lockfile instead of editing the existing pnpm lockfile. This will also likely fail when pnpm specific dependency protocols are used like `workspace:` (not demonstrated in these repro steps but can provide further repro steps if needed).

```
jkim@dev-jkim prisma-issue-5340 (master) $ DATABASE_URL=mysql://root:pintastic@127.0.0.1:3306/dev pnpx prisma migrate dev --name init
Prisma schema loaded from prisma/schema.prisma
Datasource "db": MySQL database "dev" at "127.0.0.1:3306"

Already in sync, no schema change or pending migration was found.

Running generate... (Use --skip-generate to skip the generators)

> prisma@2.27.0-integration-fix-sdk-pnpm.1 preinstall /home/jkim/code/prisma-issue-5340/node_modules/prisma
> node scripts/preinstall-entry.js


> prisma@2.27.0-integration-fix-sdk-pnpm.1 install /home/jkim/code/prisma-issue-5340/node_modules/prisma
> node scripts/install-entry.js


> @prisma/engines@2.27.0-2.6f7f670adbb07fd4b8862c6d2bcb8c4703065918 postinstall /home/jkim/code/prisma-issue-5340/node_modules/@prisma/engines
> node download/index.js

npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN prisma-issue-5340@1.0.0 No repository field.

+ prisma@2.27.0-integration-fix-sdk-pnpm.1
added 1 package from 1 contributor and updated 1 package in 1.269s

> @prisma/client@2.27.0-integration-fix-sdk-pnpm.1 postinstall /home/jkim/code/prisma-issue-5340/node_modules/@prisma/client
> node scripts/postinstall.js

npm WARN prisma-issue-5340@1.0.0 No repository field.

+ @prisma/client@2.27.0-integration-fix-sdk-pnpm.1
added 2 packages from 1 contributor in 1.505s

✔ Generated Prisma Client (2.27.0-integration-fix-sdk-pnpm.1) to ./node_modules/@prisma/client in 53ms
```

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


