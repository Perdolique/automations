# automations 🔧

Reusable GitHub Actions and workflows for my projects 💪😎

## What's Inside? 🤔

### Workflows 🚀

#### `deploy.yml`

Reusable workflow for building and deploying projects to Cloudflare Workers 🌬️☁️

**Features:**

- 🏗️ Builds the project and stores artifacts
- ✅ Type-checks TypeScript (optional)
- 🎯 Deploys to staging on every PR
- 🚀 Deploys to production on push to default branch
- 💬 Comments on PR with preview URL

**Inputs:**

- `working-directory` - Working directory for the project (default: `.`)
- `artifact-path` - Path to the build output directory (default: `.output`)
- `artifact-name` - Name of the deployment artifact (default: `deployment-artifact`)
- `build-command` - pnpm script name for building the project (default: `build`)
- `typecheck-command` - pnpm script name for type checking, skipped if empty (default: `test:typecheck`)
- `deploy-staging-command` - pnpm script name for deploying to staging (default: `deploy:versions:staging`)
- `deploy-production-command` - pnpm script name for deploying to production (default: `deploy:production`)

**Secrets:**

- `cloudflare-account-id` - Cloudflare account ID (required)
- `cloudflare-api-token` - Cloudflare API token (required)

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
    uses: Perdolique/automations/.github/workflows/deploy.yml@v1.0.2
    with:
      working-directory: '.'
      artifact-path: '.output'
      artifact-name: 'my-app'
      build-command: 'build'
      typecheck-command: 'test:typecheck' # empty to skip
      deploy-staging-command: 'deploy:versions:staging' # will run as `pnpm run deploy:versions:staging`
      deploy-production-command: 'deploy:production' # will run as `pnpm run deploy:production`
    secrets:
      cloudflare-account-id: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
      cloudflare-api-token: ${{ secrets.CLOUDFLARE_API_TOKEN }}
```

### Actions 🎬

#### `setup-pnpm`

Sets up pnpm and Node.js environment 📦

**Inputs:**

- `install-dependencies` - Whether to install dependencies (default: `false`)

**Example:**

```yaml
- uses: Perdolique/automations/.github/actions/setup-pnpm@v1.0.2
  with:
    install-dependencies: true
```

#### `pre-deploy`

Prepares deployment artifact 📥

**Inputs:**

- `artifact-name` - Name of the artifact to download
- `unpack-path` - Path to unpack the downloaded artifact

**Example:**

```yaml
- uses: Perdolique/automations/.github/actions/pre-deploy@v1.0.2
  with:
    artifact-name: 'deployment-artifact'
    unpack-path: '.output'
```

## Dependabot 🤖

Configured to auto-update GitHub Actions every Monday at 18:00 (Tallinn time) 🕐

## License 📄

Do whatever you want) I don't care 🤷
