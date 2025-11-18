# GitHub Pages Setup Instructions

## Problem

This repository has a custom GitHub Actions workflow (`.github/workflows/deploy.yml`) that builds and deploys Slidev slides to GitHub Pages. However, if GitHub Pages is configured incorrectly, an auto-generated `pages-build-deployment` workflow will be created that interferes with the custom workflow.

## Solution

Configure GitHub Pages to use **GitHub Actions** as the publishing source instead of deploying from a branch.

### Steps to Configure

1. Go to the repository settings:
   ```
   https://github.com/fiji/i2k-2025-appose/settings/pages
   ```

2. Under **"Build and deployment"**:
   - Set **Source** to: `GitHub Actions`
   - Do NOT use "Deploy from a branch"

3. Save the settings

4. The next push to the `main` branch will trigger the custom deploy workflow and publish the Slidev slides correctly

### What This Fixes

- **Prevents auto-generated workflow**: When Pages is set to "Deploy from a branch", GitHub creates an auto-generated `pages-build-deployment` workflow that publishes the repository root (README). This interferes with our custom Slidev deployment.

- **Enables custom workflow**: Setting the source to "GitHub Actions" allows our custom `.github/workflows/deploy.yml` to deploy the built Slidev slides instead.

### Workflow Details

The custom workflow (`.github/workflows/deploy.yml`):
- ✅ Has proper permissions (`pages: write`, `id-token: write`)
- ✅ Builds Slidev slides to the `dist` directory
- ✅ Uploads the build artifact for Pages
- ✅ Deploys using `actions/deploy-pages@v4`

No code changes are needed - the workflow is already correctly configured!

### Verification

After configuring Pages correctly:
1. Push a commit to the `main` branch
2. Check the Actions tab - you should see "Deploy Slidev to GitHub Pages" workflow run successfully
3. The slides will be available at: `https://fiji.github.io/i2k-2025-appose/`

### Troubleshooting

If the deployment still fails:
- Verify the workflow has completed successfully in the Actions tab
- Check that the artifact was uploaded (should see "Upload artifact" step succeeded)
- Ensure the repository has Pages enabled in settings
- Confirm the `GITHUB_TOKEN` has appropriate permissions (should be automatic)
