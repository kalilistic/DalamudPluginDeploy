# Deploy Dalamud Plugin

This GitHub reusable workflow deploys a Dalamud plugin to the appropriate release channel.
It uses a manual dispatch and requires manual versioning but will update the version in the csproj file for you.

## Inputs

- `public_name`: The name the user can see (required)
- `internal_name`: The assembly name (required)
- `project_dir`: Folder containing csproj (required)
- `project_name`: Csproj file name (required)
- `version`: Version of plugin to deploy (required)
- `deployment_type`: Stable or testing/live (required)
- `github_username`: GitHub username for commits (required)
- `github_email`: GitHub email for commits (required)
- `open_pr`: Open pull request (optional, default `false`)

## Secrets

- `DEPLOY_TOKEN`: Personal Access Token for deployment (required)

## Example Usage

To use this reusable workflow, create a workflow file in your repository's `.github/workflows/` directory with the following content:

```yaml
name: Deploy Plugin

on:
  workflow_dispatch:
    inputs:
      deployment_type:
        default: 'stable'
        required: true
        type: string
        description: 'stable or testing'
      version:
        required: true
        description: 'version (e.g. 1.0.0.0)'
        type: string

jobs:
  deploy-plugin:
    uses: kalilistic/DalamudPluginDeploy/.github/workflows/deploy_plugin.yml@master
    with:
      public_name: "PlayerTrack"
      internal_name: "PlayerTrack"
      project_dir: "PlayerTrack.Plugin"
      project_name: "PlayerTrack.Plugin"
      github_username: "kalilistic"
      github_email: "35899782+kalilistic@users.noreply.github.com"
      open_pr: false
      deployment_type: ${{ github.event.inputs.deployment_type }}
      version: ${{ github.event.inputs.version }}
    secrets:
      DEPLOY_TOKEN: ${{ secrets.PAT }}
```

## .csproj
```xml
<PropertyGroup>
    <Version>5.6.0.0</Version>
    <PackageVersion>$(Version)</PackageVersion>
    <AssemblyVersion>$(Version)</AssemblyVersion>
</PropertyGroup>
```

## Other Notes
- Only supports semver with 4 digits (e.g. 1.0.0.0).
- You can use this to open PRs but it's disabled by default.
- Expects a secret with a personal access token with sufficient perms called PAT.

## How to Run
1. Go to Actions tab.
2. Click "Deploy Plugin" workflow.
3. Click "Run workflow" button.
4. Fill out the Version and Deployment Type inputs.
5. Click "Run workflow" button.
6. Wait for it to finish.
7. Open the DalamudPlugins17 repo and create your PR.
