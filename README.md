# pi-context-hub

[![npm version](https://img.shields.io/npm/v/pi-context-hub.svg)](https://www.npmjs.com/package/pi-context-hub)
[![pi package](https://img.shields.io/badge/pi-package-purple)](https://pi.dev/packages/pi-context-hub)
[![GitHub](https://img.shields.io/badge/GitHub-JeancarloBarrios%2Fpi--context--hub-black?logo=github)](https://github.com/JeancarloBarrios/pi-context-hub)

Pi extension wrapping [Context Hub](https://github.com/andrewyng/context-hub) (`@aisuite/chub`) so Pi can search and fetch current API/SDK docs without going through generic bash or MCP.

## Install

From npm:

```bash
pi install npm:pi-context-hub
```

From GitHub:

```bash
pi install git:github.com/JeancarloBarrios/pi-context-hub
```

Try without installing permanently:

```bash
pi -e npm:pi-context-hub
```

## What it adds to Pi

Tools available to the agent:

- `chub_search` — search Context Hub docs and skills.
- `chub_get` — fetch a doc/skill by ID, language, version, specific file, or full entry.
- `chub_annotate` — manage local persistent annotations.
- `chub_feedback` — optionally send non-sensitive up/down doc feedback.

Manual command:

```text
/chub search openai
/chub get openai/chat --lang py
```

It also bundles a `get-api-docs` skill so Pi is more likely to fetch current docs before writing code against third-party APIs, SDKs, frameworks, or libraries.

## Example prompts

```text
Use Context Hub docs and implement Stripe Checkout in TypeScript.
```

```text
Search current OpenAI Python SDK docs before writing the integration.
```

```text
Use current LangGraph docs before changing this workflow.
```

## Local development

```bash
git clone https://github.com/JeancarloBarrios/pi-context-hub.git
cd pi-context-hub
npm install
pi -e .
```

Package validation:

```bash
npx -p typescript tsc --noEmit --module NodeNext --moduleResolution NodeNext --target ES2022 --skipLibCheck extensions/context-hub.ts
npm pack --dry-run
```

## Chub binary resolution

By default the extension runs the package-local `@aisuite/chub` binary with Node, so it does not depend on a global `chub` on `PATH`.

Overrides:

- `PI_CONTEXT_HUB_CHUB_BIN=/absolute/path/to/chub` — run a specific executable.
- `PI_CONTEXT_HUB_ALLOW_GLOBAL_CHUB=1` — explicitly allow fallback to `chub` from `PATH` if package-local resolution fails.

## Security notes

`chub_annotate` stores local notes under Context Hub's normal local config/cache area. Do not put secrets, private code, credentials, or sensitive architecture details in annotations or feedback.

`chub_feedback` sends feedback to Context Hub maintainers. Ask the user before sending feedback unless they explicitly requested it.
