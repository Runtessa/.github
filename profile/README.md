<div align="center">

# Runtessa

### Build the application. Keep the platform running.

**An open-source application platform for live backend code, executable data models and React interfaces.**

[Website & docs](https://runtessa.github.io/platform/) · [Explore the platform](https://github.com/Runtessa/platform) · [Build your first application](https://github.com/Runtessa/platform/blob/main/docs/tutorials/first-application.md) · [Architecture](https://github.com/Runtessa/platform/blob/main/docs/architecture/dependency-graph.md) · [Contribute](https://github.com/Runtessa/platform/blob/main/CONTRIBUTING.md)

</div>

## Applications evolve while the platform keeps running

Runtessa brings application development and execution into one self-hosted
TypeScript platform. Developers author tenant backend routes, TypeORM models,
React UI packages and access rules in a browser Studio or a VS Code workspace.
Backend and UI changes can be previewed as drafts, reviewed and published as
application revisions.

When a request arrives, the runtime resolves the tenant and access policy,
selects the published backend code and executes it. The browser Kernel loads
published UI packages. **Publishing application code does not require rebuilding
or restarting the platform services.** Platform upgrades and database schema
changes retain their own build and migration workflows.

The building blocks fit internal business applications, administration tools,
organization portals and integration-backed dashboards. They provide an
application platform to build on; those products are not prebuilt applications.

## The live application lifecycle

1. **Author:** edit backend modules, models, React packages and permissions in the
   tenant workspace. Put reusable application logic into shared projects.
2. **Preview:** save a draft and exercise it with tenant-authorized developer
   access. Draft and published module/UI artifacts remain separate.
3. **Publish:** review selected changes and publish their application revision.
   The runtime selects the updated published artifacts for subsequent requests.
4. **Execute:** Core checks access and runs backend code in a restricted process;
   Kernel resolves and loads application UI in the browser.
5. **Iterate:** inspect differences and revision history, restore source to a
   draft when needed, then review and publish the next revision.

The two paths have explicit responsibilities:

| Path | Flow | What happens |
| --- | --- | --- |
| Application publication | Developer → Studio → Core database | Studio persists drafts and revisions, then selects the reviewed published module snapshot. |
| Backend request | Application client → Core → restricted process → Core → client | Core resolves the tenant, ACL and selected code, grants execution capabilities, and returns the handler result. |

Core and Studio host their execution processes inside their existing services.
Kernel runs in the application browser. The
[dependency graph](https://github.com/Runtessa/platform/blob/main/docs/architecture/dependency-graph.md)
separately documents service calls, logical databases, Redis and optional tools.

### A published application handler

A tenant GET module configured at `/api/hello` can export this handler:

```js
module.exports = async function handler(req, adapters) {
  return {
    statusCode: 200,
    body: { message: 'Hello from Runtessa', revision: 'v1' },
  };
};
```

Configure the tenant and endpoint ACL, save the draft and publish it. Change
`revision` to `v2`, preview the draft and publish again: subsequent published
requests use the new application code while the platform services keep running.
The [first application walkthrough](https://github.com/Runtessa/platform/blob/main/docs/tutorials/first-application.md)
connects an HTTP module to a React interface and covers the required setup.

## What the platform includes

| Area | Capabilities |
| --- | --- |
| **Live backend** | Tenant routing, HTTP handlers, runtime compilation, named adapter grants and restricted execution |
| **Executable models** | TypeORM classes, decorators, relations and methods; reviewed tenant schema operations |
| **Application UI** | React packages, component dependencies, route manifests, styles and a session-aware Kernel API client |
| **Development Studio** | Monaco editing, split and diff views, search, draft preview, publication, revision history and administration |
| **Identity and access** | Accounts, organizations, roles, tenant OAuth/SSO, workspace authorization and runtime endpoint ACL |
| **Application reuse** | Versioned shared backend projects and UI packages used across tenant applications |
| **Developer tooling** | VS Code workspace pull/push, file authoring, preview and assistant workflows |
| **Operations** | Monitoring snapshots and alerts; optional assistant gateway and container-management tools |

Model changes have a distinct lifecycle: a Studio model save can initiate tenant
schema synchronization. Restoring source does not restore database contents, and
module publication does not apply platform migrations. See
[authoring and publication](https://github.com/Runtessa/platform/blob/main/docs/platform/authoring.md)
and the [model cookbook](https://github.com/Runtessa/platform/blob/main/docs/platform-cookbook/02-backend-modules.md).

## A repository for each responsibility

[`platform`](https://github.com/Runtessa/platform) is the integration workspace:
it pins tested component commits, owns shared build and deployment tooling, and
retains the original project's complete history. Its directory layout preserves
the relationships between npm workspaces, shared packages and image builds.

| Repository | Responsibility | Integration path |
| --- | --- | --- |
| [runtime](https://github.com/Runtessa/runtime) | Core: tenant routing, ACL, registry, adapters and live backend execution | `src/core` |
| [studio](https://github.com/Runtessa/studio) | Browser editor and IDE server: authoring, previews, publication and administration | `src/ideNew` |
| [kernel](https://github.com/Runtessa/kernel) | Application browser runtime: UI package loading, routing and API access | `src/kernel` |
| [packages](https://github.com/Runtessa/packages) | Shared entities, contracts, restricted-process support and platform migrations | `src/packages` |
| [auth](https://github.com/Runtessa/auth) | Identity service, organizations, sessions and tenant OAuth/SSO | `src/auth` |
| [vscode](https://github.com/Runtessa/vscode) | Local tenant application development in VS Code | `src/ide-vscode` |
| [monitoring](https://github.com/Runtessa/monitoring) | Monitoring ingest, metric storage, alerts and rules | `src/monitoring` |
| [assistant-gateway](https://github.com/Runtessa/assistant-gateway) | Optional HTTP/SSE bridge to Codex CLI workspaces | `src/codex-gateway` |
| [container-agent](https://github.com/Runtessa/container-agent) | Optional Docker/Podman lifecycle and orchestration operations | `src/container-agent` |

Components include their relevant subtree histories, with original authors and
commit dates. The original cross-component commit IDs remain in `platform`.
[Repository ownership and migration evidence](https://github.com/Runtessa/platform/blob/main/docs/project/repositories.md)
explain the split and the pinned workspace contract.

## Get started

Clone the complete integration workspace, including its pinned components:

```sh
git clone --recurse-submodules https://github.com/Runtessa/platform.git
cd platform
npm ci
```

Use **Node 22.13+ in the 22.x line**. Live execution requires a compatible Linux
host with bubblewrap and user namespaces. PostgreSQL 17 and Redis provide the
platform stores; Docker/Podman supports the packaged installation and disposable
integration tests.

The commands above install the root workspace. Continue through the
[installation guide](https://github.com/Runtessa/platform/blob/main/docs/getting-started.md)
for service dependencies, private configuration, platform migrations and startup.
Then follow the
[HTTP + React tutorial](https://github.com/Runtessa/platform/blob/main/docs/tutorials/first-application.md).

| You want to… | Start here |
| --- | --- |
| Understand the application platform | [Platform tour](https://github.com/Runtessa/platform/blob/main/docs/platform/overview.md) |
| Write routes, models, UI and access rules | [Application cookbook](https://github.com/Runtessa/platform/blob/main/docs/platform-cookbook/README.md) |
| Work on platform internals | [Developer guide](https://github.com/Runtessa/platform/blob/main/docs/developer-guide.md) |
| Operate or upgrade an installation | [Docker guide](https://github.com/Runtessa/platform/blob/main/docker/README.md) and [platform migrations](https://github.com/Runtessa/platform/blob/main/scripts/database/README.md) |
| Inspect release evidence and remaining work | [Validation](https://github.com/Runtessa/platform/blob/main/docs/release/validation.md) and [roadmap](https://github.com/Runtessa/platform/blob/main/ROADMAP.md) |

## Build with us

Contributions can improve runtime behavior, the authoring experience, integrations,
accessibility, examples, tests and documentation. Start in the component that
owns the behavior. Develop and test from the assembled `platform` workspace;
commit a component change in its repository, then review the updated platform
commit pin and any integration lock changes.

The integration pipeline exercises Core and Studio authorization, live execution
and process isolation, database migrations, fresh installation, actual image
startup, OAuth/PKCE, Redis monitoring, TLS and the VS Code extension host.
Recursive source and history scans cover all initialized components.

Read the [contribution guide](https://github.com/Runtessa/platform/blob/main/CONTRIBUTING.md)
for checks and review expectations. Use
[platform issues](https://github.com/Runtessa/platform/issues) for cross-component
work and the owning repository's issues for component changes. Report sensitive
findings through the [security policy](https://github.com/Runtessa/platform/blob/main/SECURITY.md).

## Project status and license

**Open-source developer preview.** The platform and its nine component repositories
are public. Validation records document exercised paths and remaining dependency,
operational and conformance work. Production load guarantees and unrestricted
public execution of untrusted code are outside the current release claims.

The code and documentation use
[Apache-2.0](https://github.com/Runtessa/platform/blob/main/LICENSE), with retained
[notices](https://github.com/Runtessa/platform/blob/main/NOTICE). Third-party assets
and dependencies retain their own licenses.
