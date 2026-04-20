# 🔍 ApiNotes OpenAPI Validate & Diff

**Validate your OpenAPI specs and detect breaking changes on every pull request.**

Supports OpenAPI 3.0, 3.1, 3.2, and Swagger 2.0.

## ✨ Features

- ✅ **Validate** your OpenAPI spec against the full specification
- 🔀 **Diff** against the base branch to detect breaking changes  
- 💬 **PR Comments** with detailed error reports and change summaries
- ⚡ **Fast** — runs in seconds, no Docker required
- 🆓 **Free tier** — 20 validations/month, no credit card needed

## 🚀 Quick Start

```yaml
# .github/workflows/openapi-check.yml
name: OpenAPI Check
on:
  pull_request:
    paths:
      - 'openapi.yaml'
      - 'openapi.json'
      - 'docs/api/**'

permissions:
  contents: read        # needed for actions/checkout
  pull-requests: write  # needed for posting PR comments

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0  # Needed for diff against base branch

      - uses: apinotes/openapi-validate@v1
        with:
          api-key: ${{ secrets.APINOTES_API_KEY }}
          spec-path: 'openapi.yaml'
```

## 🔑 API Key Setup (Private Repos Only)

Public repositories work out of the box — **no account or API key needed.**

For private repositories, set up an API key:

1. Create a free account at [apinotes.io](https://apinotes.io?utm_source=github-action)
2. Go to **Dashboard → Settings → API Keys**
3. Copy your key
4. In your repo, go to **Settings → Secrets → Actions** and add `APINOTES_API_KEY`

## ⚙️ Configuration

| Input | Default | Description |
|-------|---------|-------------|
| `api-key` | *required* | Your ApiNotes API key |
| `spec-path` | `openapi.yaml` | Path to your OpenAPI spec |
| `diff-enabled` | `true` | Diff against base branch for breaking changes |
| `fail-on-errors` | `true` | Fail the workflow if spec is invalid |
| `fail-on-breaking` | `false` | Fail the workflow if breaking changes found |
| `comment-on-pr` | `true` | Post results as a PR comment |

## 📊 Example PR Comment

When the action runs, it posts a comment like this on your PR:

> ## ✅ OpenAPI Validation Report
> | Property | Value |
> |----------|-------|
> | File | `openapi.yaml` |
> | Status | **Valid** |
> | OpenAPI Version | 3.1 |
> | Errors | 0 |
> | Warnings | 2 |
>
> ---
> ### 🔀 API Diff (vs base branch)
> | Metric | Count |
> |--------|-------|
> | ➕ Added endpoints | 2 |
> | ✏️ Changed endpoints | 1 |
> | ➖ Removed endpoints | 0 |
> | ⚠️ **Breaking changes** | **1** |

## 🔀 Breaking Change Detection

The diff engine detects:
- 🔴 Removed endpoints
- 🔴 Required parameters added
- 🔴 Response schema fields removed
- 🔴 Authentication requirements changed  
- 🟠 OperationId changes
- 🟠 Deprecated endpoints
- ℹ️ New endpoints, tags, descriptions

## 🚫 Block PRs with Invalid Specs

The action fails the check when `fail-on-errors: true` (the default).
To prevent merging broken specs:

1. Go to your repo → **Settings** → **Branches**
2. Add a branch protection rule for `main`
3. Enable **"Require status checks to pass before merging"**
4. Search for and select the **`validate`** job name
5. Save

Now PRs with invalid OpenAPI specs cannot be merged.

## 💰 Pricing

| Plan | Repo Type | Validations/month | Account Required | Price |
|------|-----------|-------------------|------------------|-------|
| Open Source | Public | Unlimited | No | Free |
| Starter | Private | 8 | Yes | Free |
| Developer | Private | 1,000 | Yes | $6.99/mo |

