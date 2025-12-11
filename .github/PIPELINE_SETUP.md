# GitHub Actions CI/CD Pipeline

This project uses GitHub Actions for automated building, testing, and deployment to CloudHub 2.0.

## Workflows

### 1. CI Build (`ci-build.yml`)
- **Triggers**: Pull requests to `main`/`development`, pushes to `feature/**` or `bugfix/**` branches
- **Jobs**:
  - Build and Test: Compiles, tests, and packages the Mule application
  - Code Quality: Validates XML files and checks for sensitive data

### 2. Deploy to CloudHub 2.0 (`deploy-cloudhub2.yml`)
- **Triggers**: Pushes to `main` branch, or manual workflow dispatch
- **Deploys**: To Sandbox environment in CloudHub 2.0
- **App Name**: `dex-401-training-mule`

## Required GitHub Secrets

Only **2 secrets** needed:

| Secret Name | Description | Example |
|------------|-------------|---------|
| `ANYPOINT_CLIENT_ID` | Connected App Client ID | `abc123def456...` |
| `ANYPOINT_CLIENT_SECRET` | Connected App Client Secret | `xyz789...` |

### Configuration (Hardcoded in Workflow)
| Parameter | Value | Description |
|-----------|-------|-------------|
| CloudHub Target | `Cloudhub-US-East-2` | US East (Ohio) region |
| Business Group | `FPA` | Business group name |
| Business Group ID | `bdc363af-79af-48f0-9e26-e6f5fd1bee9c` | Used in deployment |

**Note**: These values are hardcoded in the workflow files. No secrets needed for these parameters.

## How to Set Up Secrets

1. Go to your GitHub repository
2. Click **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Add each secret with its corresponding value

## Creating a Connected App in Anypoint Platform

1. Log in to Anypoint Platform
2. Go to **Access Management** → **Connected Apps**
3. Click **Create App**
4. Select **App acts on its own behalf (client credentials)**
5. Add scopes:
   - `Design Center Developer`
   - `Cloudhub Organization Admin` or `Cloudhub Deploy`
   - `Exchange Contributor`
6. Copy the **Client ID** and **Client Secret**
7. Add them to GitHub secrets

## Getting Environment and Organization IDs

### Organization ID:
```bash
# From Anypoint Platform URL when logged in
https://anypoint.mulesoft.com/accounts/#/cs/profile/home
# The ID is in the URL or in Access Management
```

### Environment ID:
```bash
# Go to your environment in Anypoint Platform
# The ID is in the URL:
https://anypoint.mulesoft.com/cloudhub/#/console/home/applications?environmentId=YOUR_ENV_ID
```

### Business Group:
**Configured**: 
- Name: `FPA`
- ID: `bdc363af-79af-48f0-9e26-e6f5fd1bee9c`
- Find in: Access Management → Business Groups

### CloudHub 2.0 Target:
**Configured**: `Cloudhub-US-East-2` (Ohio)

Other common values:
- `Cloudhub-US-West-1` (Oregon)
- `Cloudhub-US-East-1` (N. Virginia)
- `Cloudhub-EU-Central-1` (Frankfurt)

## Manual Deployment

You can manually trigger deployments:
1. Go to **Actions** tab in GitHub
2. Select **Deploy to CloudHub 2.0**
3. Click **Run workflow**
4. Click **Run workflow** to deploy to Sandbox

## Branch Strategy

- `main` → Sandbox deployment (on merge)
- `feature/**` → CI build and test only
- `bugfix/**` → CI build and test only

## Version Management

Versions are auto-incremented based on git commit count:
- Format: `{major}.{minor}.{commitCount}`
- Example: `1.0.42` (42nd commit)
