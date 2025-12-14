# 🧬 Sintesi
The AI Engineer that keeps your documentation in sync with your code.

Sintesi analyzes your code and verifies that the documentation (Markdown) is up-to-date. If it detects a discrepancy ("Drift"), it uses Generative AI to rewrite the documentation and automatically opens a Pull Request with the fix.

## 🚀 Quick Start

Add a new workflow in `.github/workflows/docs.yml`:

```yaml
name: Sintesi - Documentation AI
on:
  push:
    branches: [ main ]

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

permissions:
  contents: write
  pull-requests: write

jobs:
  sync-docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Sintesi Check & Fix
        uses: doctypedev/action@v0
        with:
          openai_api_key: ${{ secrets.OPENAI_API_KEY }}
          github_token: ${{ secrets.GITHUB_TOKEN }}
          targets: 'readme,docs' # Documentation targets to generate (comma-separated). Options: readme, docs
          docs_output: 'docs' # Optional: Directory for the output documentation - Default: docs
          readme_output: 'README.md' # Optional: Path for the output README file - Default: README.md
```

## ⚙️ Inputs

| Input | Description | Required | Default |
| :--- | :--- | :--- | :--- |
| `openai_api_key` | Your OpenAI API Key. Necessary to allow the agent to generate corrections. | ✅ | - |
| `github_token` | The token to interact with the repository. Usually `${{ secrets.GITHUB_TOKEN }}` is sufficient. | ✅ | - |
| `version` | The version of the Sintesi CLI to install (e.g., `latest`, `0.1.0`). | ❌ | `latest` |
| `readme_output` | Path for the output README file. | ❌ | `README.md` |
| `docs_output` | Directory for the output documentation. | ❌ | `docs` |
| `targets` | Documentation targets to generate (comma-separated). Options: readme, docs. | ❌ | `readme,docs` |

## 🛡️ Required Permissions

This action performs write operations (creating branches, committing, and opening PRs). It is mandatory to configure permissions in the workflow or in the repository settings.

If the workflow fails with "403 Forbidden" errors, verify that you have set:

```yaml
permissions:
  contents: write
  pull-requests: write
```

Or, in the repo settings: **Settings > Actions > General > Workflow permissions > Read and write permissions**.

Made with ❤️ by developers who hate outdated documentation.
