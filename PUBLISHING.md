# Publishing Workflow

This document describes how content is published from the private source repository to this public repository.

## Overview

This repository serves as the public distribution point for PDCVLabelsWebApp. All development occurs in a private repository, and selected content is published here automatically.

## Publishing Process

### Automated Publishing

The publishing process is automated through GitHub Actions:

1. **Trigger**: Publishing can be triggered via:
   - `repository_dispatch` event from the private repository
   - Manual workflow dispatch in this repository

2. **Workflow**: The `.github/workflows/publish.yml` workflow handles the publishing process

3. **Content Filtering**: Only distributable content is published (source code, documentation, release artifacts)

### What Gets Published

The following content types are typically published:
- ✅ Application source code (distributable)
- ✅ Public documentation
- ✅ LICENSE and README files
- ✅ Release binaries and assets
- ✅ Public configuration examples

### What Stays Private

The following content remains in the private repository:
- ❌ Development documentation and internal notes
- ❌ Private configuration and secrets
- ❌ Internal tooling and scripts
- ❌ Proprietary dependencies or assets
- ❌ Work-in-progress features

## Publishing from Private Repository

### Setup Requirements

To publish from the private repository, you need:

1. **Personal Access Token (PAT)** with appropriate permissions:
   - `repo` scope for the public repository
   - `workflow` scope to trigger workflows

2. **Repository Secret**: Store the PAT as a secret in the private repository (e.g., `PUBLIC_REPO_TOKEN`)

3. **Workflow in Private Repository**: Create a workflow that triggers publishing

### Example Private Repository Workflow

```yaml
name: Publish to Public Repository

on:
  push:
    branches: [main]
    tags: ['v*']
  workflow_dispatch:

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout private repository
        uses: actions/checkout@v4
        
      - name: Trigger public repository publish
        uses: peter-evans/repository-dispatch@v2
        with:
          token: ${{ secrets.PUBLIC_REPO_TOKEN }}
          repository: Popular-Design/PDCVLabelsWebApp-Publish
          event-type: publish-release
```

## Manual Publishing

For manual publishing:

1. Navigate to the Actions tab in this repository
2. Select the "Publish from Private Repository" workflow
3. Click "Run workflow"
4. Specify parameters if needed
5. Review and approve the changes

## Version Management

- Releases are tagged in both repositories
- Version numbers follow semantic versioning (semver)
- Public releases are synchronized with private repository milestones

## Rollback

If a published version needs to be rolled back:

1. Identify the commit to revert to
2. Create a new commit reverting the changes
3. Trigger a new publish workflow
4. Update release notes to reflect the rollback

## Best Practices

1. **Test Before Publishing**: Always test in the private repository before publishing
2. **Review Content**: Ensure no sensitive information is included in published content
3. **Update Documentation**: Keep public documentation synchronized with releases
4. **Version Consistency**: Maintain version consistency between private and public repositories
5. **Security**: Regularly audit what content is being published

## Troubleshooting

### Publishing Workflow Fails

- Check GitHub Actions logs for error messages
- Verify token permissions
- Ensure no merge conflicts exist

### Content Not Appearing

- Verify the workflow completed successfully
- Check .gitignore settings
- Ensure content was committed in the workflow

### Unauthorized Errors

- Verify PAT has correct permissions
- Check token hasn't expired
- Ensure secret is properly configured

## Support

For issues with the publishing process:
- Review workflow logs in the Actions tab
- Check this documentation
- Open an issue in this repository
- Contact repository maintainers
