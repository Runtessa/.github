<div align="center">

![Runtessa — Signal](https://github.com/Runtessa/.github-private/blob/main/profile/signal.png?raw=true)

# Runtessa

### Build the application. Keep the platform running.

Live tenant backend code, executable data models and React applications.

</div>

## Start here

[Platform workspace](https://github.com/Runtessa/platform) brings the tested
component versions together. All repositories are private during preparation.

```sh
git clone --recurse-submodules https://github.com/Runtessa/platform.git
cd platform
```

## Component ownership

| Repository | Responsibility |
| --- | --- |
| [runtime](https://github.com/Runtessa/runtime) | Live tenant backend runtime |
| [studio](https://github.com/Runtessa/studio) | Browser development studio |
| [kernel](https://github.com/Runtessa/kernel) | React application browser kernel |
| [packages](https://github.com/Runtessa/packages) | Shared entities, migrations and runtime contracts |
| [auth](https://github.com/Runtessa/auth) | Identity, OAuth and SSO |
| [assistant-gateway](https://github.com/Runtessa/assistant-gateway) | Optional Codex CLI gateway |
| [container-agent](https://github.com/Runtessa/container-agent) | Optional container operations agent |
| [monitoring](https://github.com/Runtessa/monitoring) | Optional monitoring and alerts |
| [vscode](https://github.com/Runtessa/vscode) | VS Code tenant workspace extension |

The original project history stays in `platform`; each component includes its
subtree history. Repository separation adds no execution service and keeps the
IDE's established themes and editor colors.

[Contribution guide](https://github.com/Runtessa/platform/blob/main/CONTRIBUTING.md) ·
[Architecture and history](https://github.com/Runtessa/platform/blob/main/docs/project/repositories.md)
