# Multi-tenancy

> Original text and pictures taken from here: https://github.com/supertokens/docs/blob/master/v2/multitenancy/architecture.mdx

![Tenancy](/images/single-tenant-vs-multi-tenant.png)

## Types of Multi-tenancy

![Schema](/images/multitenancy-pillar-schema.png)

## User Flow

![User Flow](/images/multi-tennant-user-flow.png)

## Features

![Features](/images/multi-tenancy-features.png)

## Tenant and App ID
In a single tenant system, you have one user pool and one common set of login methods for each of the users. In a multi tenant system, you have one user pool per tenant and can have different login methods for each tenant. These different user pools can be in the same database or different databases.

There are two levels of abstractions for multi tenancy:
- Tenant level: This is the level at which you can have different user pools, login methods and / or databases per tenant. All of these tenants and their users are part of the same application. Users can also be shared across tenants. 
- App level: This is the top most level, and is the level at which you can define multiple apps to run using the same app core instance. Each app can have its own set of tenants and users, which can't be shared with other apps. Therefore, this is most suitable for:
    - Running different applications within your company, all using the same app core instance.
    - Running different dev envs (dev1, dev2, staging, cicd, prod, etc) for the same application.

When you start the core for the first time, backend server creates an app (appId is `"public"`) and one tenant in it (tenantId is `"public"`). From there on, both tenants and apps can be created dynamically at runtime by querying the core. When you create a new app, you also get a new tenant (tenantId is `"public"`) as part of that app created for you.

## Relation to end users and their data
In a multi app, multi tenant setup:
- A user can be uniquely recognized by their appId -> userId. This allows the same user to be shared across tenants (if you want that to happen).
- The identity of the user (their email for example) can be uniquely identified by appId -> tenantId -> email. This allows the same email to be used across tenants in a way that they are still treated as different users (they will have different user IDs). The same holds true for phone numbers and third party login profile
- Roles and permissions are created on an app level, but their mapping to the users are defined on a tenant level. This means that the same user shared across tenants can have different roles / permissions, depending on the tenant they are currently logged into. It also means that you can share the same role / permission set across tenants.
- Sessions are per tenant (appId -> tenantId -> session handle) and cannot be shared across tenants.
- User metadata is per app level since users are also on a per app level.

You can also inspect our database schema to get a better understanding of the data model (find the schema in one of the auth recipes -> pre build UI -> core -> self hosted -> postgresql / mysql).

![Workspaces](/images/parent-workspaces.png)

## Relation to database
From a database point of view, you can create multiple apps and tenants in the same database or in different databases. The only restriction is that for an app, you cannot share a user across `tenantA` and `tenantB` if the databases for `tenantA` and `tenantB` are different. Putting it another way, a user can only be shared across the tenants if those tenants are using the same database.

## Relation to backend and frontend SDK setup
Unlike tenants, each app will need to have its own app backend and app frontend SDK setup. This means that for each app configured in the app core, you will need to run a separate backend and frontend, just for that app. This in turn fits well with the model described above wherein an app is defined as separate, independent applications or separate dev environments.

For multiple tenants, you can run the same backend and frontend across all tenants of an app. Each request from the frontend will contain a tenantId identifying that tenant to the backend, and once logged in, each session will also contain that user's tenantId.

# Links

 - Good intro: https://www.gooddata.com/blog/what-multitenancy/
 - Arch intro: https://www.gooddata.com/blog/multi-tenant-architecture/
 - Nice schemas/pics: https://supertokens.com/features/multi-tenancy
 - Good arch description: https://supertokens.com/docs/multitenancy/architecture
 - Loopback 4 example for multi-tenant app: https://github.com/loopbackio/loopback-next/tree/master/examples/multi-tenancy