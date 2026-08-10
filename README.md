# automations 🔧

Reusable GitHub Actions and workflows for my projects 💪😎

## What's Inside? 🤔

### Workflows 🚀

#### `deploy.yml`

Reusable workflow for building and deploying projects to Cloudflare Workers 🌬️☁️

**Features:**

- 🏗️ Builds the project and stores artifacts
- ✅ Runs opt-in checks before deploy
- 🎯 Deploys to staging on every PR
- 🚀 Deploys to production on push to default branch
- 💬 Comments on PR with all staging deployment URLs

**Inputs:**

- `working-directory` - Working directory for the project (default: `.`)
- `artifact-path` - Path to the build output directory (default: `.output`)
- `artifact-name` - Name of the deployment artifact (default: `deployment-artifact`)
- `build-command` - pnpm script name for building the project (default: `build`)
- `test-typecheck-command` - pnpm script name for type checking, skipped if empty (default: empty)
- `lint-markdown-command` - pnpm script name for markdown linting, skipped if empty (default: empty)
- `lint-oxlint-command` - pnpm script name for oxlint, skipped if empty (default: empty)
- `test-unit-command` - pnpm script name for unit tests, skipped if empty (default: empty)
- `test-e2e-command` - pnpm script name for end-to-end tests, skipped if
  empty (default: empty)
- `playwright-browsers` - Space-separated Playwright browsers installed before
  end-to-end tests (default: empty)
- `deploy-to-staging-command` - pnpm script name for deploying to staging
  (default: `deploy:versions:staging`). Both version previews and direct Worker
  deployments are supported.
- `deploy-to-production-command` - pnpm script name for deploying to production (default: `deploy:production`)

Optional checks gate both staging and production deploys. If a check command is not provided, its job is skipped.

**Secrets:**

- `cloudflare-account-id` - Cloudflare account ID (required)
- `cloudflare-api-token` - Cloudflare API token (required)

**Setup:**

1. Create a workflow file `.github/workflows/deploy.yml` in your repository
2. Generate a Cloudflare API token with proper permissions (you can use the "Edit Cloudflare Workers" template)
3. Add the token as `CLOUDFLARE_API_TOKEN` secret in your repository settings
4. Add your Cloudflare account ID as `CLOUDFLARE_ACCOUNT_ID` secret
5. Create a `.node-version` file in the root of your repository with the Node.js version (e.g., `24.13.0`)

**Usage:**

```yaml
name: Build and Deploy

permissions:
  contents: read
  pull-requests: write

# Deploy to staging on PRs, to production on pushes to default branch
on:
  pull_request:
    branches:
      - master
    types:
      - opened
      - synchronize
  push:
    branches:
      - master

jobs:
  deploy:
    uses: Perdolique/automations/.github/workflows/deploy.yml@v3
    with:
      working-directory: '.'
      artifact-path: '.output'
      artifact-name: 'my-app'
      build-command: 'build'
      test-typecheck-command: 'test:typecheck'
      lint-markdown-command: 'lint:markdown'
      lint-oxlint-command: 'lint:oxlint'
      test-unit-command: 'test:unit:ci'
      test-e2e-command: 'test:e2e:ci'
      playwright-browsers: 'chromium webkit'
      deploy-to-staging-command: 'deploy:versions:staging' # will run as `pnpm run deploy:versions:staging`
      deploy-to-production-command: 'deploy:production' # will run as `pnpm run deploy:production`
    secrets:
      cloudflare-account-id: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
      cloudflare-api-token: ${{ secrets.CLOUDFLARE_API_TOKEN }}
```

### Actions 🎬

#### `setup-pnpm`

Sets up pnpm and Node.js environment 📦

**Inputs:**

- `install-dependencies` - Whether to install dependencies (default: `false`)
- `cache` - Whether to cache the pnpm store directory (default: `true`)
- `registry-url` - Optional registry to configure for authentication

**Example:**

```yaml
- uses: Perdolique/automations/.github/actions/setup-pnpm@v3
  with:
    install-dependencies: true
    cache: false
    registry-url: https://registry.npmjs.org
```

## Dependabot 🤖

Configured to auto-update GitHub Actions every Monday at 18:00 (Tallinn time) 🕐

### Dependabot Secrets 🔑

For Dependabot PRs to trigger deployment workflows, you need to add secrets for Dependabot specifically. It has its own secret storage! 🤫

Make sure to add `CLOUDFLARE_API_TOKEN` to your repository's Dependabot secrets. Otherwise, deployment workflows on Dependabot PRs will fail! 💥

Check out the [GitHub docs on configuring Dependabot secrets](https://docs.github.com/en/code-security/how-tos/secure-your-supply-chain/manage-your-dependency-security/configuring-access-to-private-registries-for-dependabot#storing-credentials-for-dependabot-to-use) for the full guide 📚

## License 📄

Do whatever you want, I don't care 🤷
