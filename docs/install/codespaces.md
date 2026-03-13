---
summary: "Run OpenClaw in a GitHub Codespace for instant cloud dev environments"
read_when:
  - You want to develop or test OpenClaw without local setup
  - You want a pre-configured cloud dev environment via GitHub Codespaces
title: "GitHub Codespaces"
---

# GitHub Codespaces

**Goal:** open the OpenClaw repo in a GitHub Codespace and have a working development environment in minutes, with no local install required.

## What is a Codespace?

A [GitHub Codespace](https://github.com/features/codespaces) is a cloud-based development environment. OpenClaw includes a `.devcontainer` configuration so you get Node 22, `pnpm`, all dependencies, and port forwarding for the Gateway — ready to go.

## Quick start

<Steps>
  <Step title="Open the Codespace">
    Go to [github.com/openclaw/openclaw](https://github.com/openclaw/openclaw), click the green **Code** button, select the **Codespaces** tab, then click **Create codespace on main**.

    The container builds once (a few minutes on first launch) and reopens in seconds on subsequent starts.

  </Step>
  <Step title="Install dependencies">
    The `.devcontainer` config runs `pnpm install` automatically after the container starts. If you need to reinstall manually:

    ```bash
    pnpm install
    ```

  </Step>
  <Step title="Build">
    ```bash
    pnpm build
    ```
  </Step>
  <Step title="Run onboarding">
    ```bash
    pnpm openclaw onboard
    ```

    The wizard configures your model provider (Anthropic, OpenAI, etc.) and optional channels.

  </Step>
  <Step title="Start the Gateway">
    ```bash
    pnpm openclaw gateway run
    ```

    Port `18789` is automatically forwarded. Open the forwarded URL in your browser to reach the Control UI.

  </Step>
</Steps>

## Accessing the Control UI

When the Gateway is running, VS Code shows a notification for the forwarded port `18789`. Click **Open in Browser** or navigate to the forwarded URL. You can also run:

```bash
pnpm openclaw dashboard
```

to print the URL.

## Persisting config across Codespaces

Config and workspace state live under `~/.openclaw/` inside the container. To persist them across Codespace rebuilds, add a [dotfiles repo](https://docs.github.com/en/codespaces/setting-your-user-preferences/personalizing-github-codespaces-for-your-account#dotfiles) or commit the relevant files to your own fork.

## Running tests

```bash
pnpm test
```

For the full test suite with coverage:

```bash
pnpm test:coverage
```

## Port reference

| Port  | Service          |
| ----- | ---------------- |
| 18789 | OpenClaw Gateway |

## Troubleshooting

**Container takes too long to build:** Codespaces pre-builds are not yet configured for OpenClaw. The first build installs all `pnpm` dependencies; subsequent starts are faster because the environment is cached.

**`pnpm: command not found`:** The `postCreateCommand` normally installs pnpm automatically. If it failed or was skipped, run `npm install -g pnpm` and open a fresh terminal.

**Gateway not reachable:** Make sure port `18789` is forwarded and the Gateway is running (`pnpm openclaw status`). Check the Ports panel in VS Code.
