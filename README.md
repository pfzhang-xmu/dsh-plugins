# DSH Plugins

[中文文档](./README.zh.md)

A collection of plugins for [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness).

## Available plugins

| Plugin | Description |
| --- | --- |
| [`dsh-github-workspace`](./dsh-github-workspace) | Local GitHub CLI workspace for browsing repositories and committing text-file changes from the DSH Web GUI. |
| [`dsh-image-tools`](./dsh-image-tools) | Host plugin registering OpenAI Images-compatible `generate_image` / `edit_image` tools for DSH agents, with project-local output storage. |
| [`dsh-cowart`](./dsh-cowart) | tldraw infinite canvas embedded in the DSH Web GUI with project-backed storage, a persistent floating window, and agent workflows for generate → annotate → refine. |

Each plugin is self-contained in its own directory with installation, configuration, security, and verification instructions. Add future plugins as sibling directories and list them here.

## Contribution requirements

Every plugin must include both an English `README.md` and a Chinese `README.zh.md`. Keep the two documents aligned when functionality, installation, configuration, security boundaries, or verification steps change.
