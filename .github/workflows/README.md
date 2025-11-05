# GitHub Actions Workflows

This directory contains automated workflows for maintaining code quality and keeping dependencies up-to-date.

## Workflows

### 1. CI Workflow (`ci.yml`)

**Purpose**: Continuous Integration workflow that runs on push and pull requests.

**Features**:
- Uses `actions/cache@v4` for efficient dependency caching
- Automatically detects and runs available npm scripts (lint, test, build)
- Runs on latest Ubuntu with Node.js 20

**Triggers**:
- Push to main/master branches
- Pull requests to main/master branches
- Manual workflow dispatch

### 2. Check Deprecated Actions (`check-deprecated-actions.yml`)

**Purpose**: Automated monitoring for deprecated GitHub Actions versions.

**Features**:
- Scans all workflow files for deprecated action versions
- Automatically creates/updates GitHub issues when deprecated actions are found
- Provides recommendations for updating to latest versions
- Runs weekly on Mondays at 9:00 AM UTC

**Triggers**:
- Scheduled: Every Monday at 9:00 AM UTC
- Manual workflow dispatch

**What it checks**:
- `actions/cache@v1` → should be `actions/cache@v4`
- `actions/cache@v2` → should be `actions/cache@v4`
- `actions/cache@v3` → should be `actions/cache@v4`
- `actions/checkout@v1|v2|v3` → should be `actions/checkout@v4`
- `actions/setup-node@v1|v2|v3` → should be `actions/setup-node@v4`
- `actions/setup-python@v1|v2|v3|v4` → should be `actions/setup-python@v5`

## Dependabot Integration

The repository uses Dependabot (`.github/dependabot.yml`) to automatically:

1. **Monitor GitHub Actions** (weekly):
   - Checks for updates to GitHub Actions used in workflows
   - Creates PRs for outdated actions
   - Keeps actions on the latest secure versions

2. **Monitor npm dependencies**:
   - Daily checks for security updates
   - Weekly checks for all updates
   - Automatically opens PRs for dependency updates

## Best Practices

### Using actions/cache@v4

The v4 version of `actions/cache` includes several improvements:
- Better performance and reliability
- Improved cache hit rates
- Better handling of concurrent workflows
- Support for larger cache sizes

**Example usage**:

```yaml
- name: Cache dependencies
  uses: actions/cache@v4
  with:
    path: |
      ~/.npm
      node_modules
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-
```

### Keeping Actions Updated

1. **Automated Updates**: Dependabot will automatically create PRs for action updates
2. **Manual Monitoring**: The `check-deprecated-actions` workflow creates issues for deprecated versions
3. **Review and Merge**: Review automated PRs and merge after validation

## Maintenance

- Review and merge Dependabot PRs regularly
- Check issues created by the deprecated actions workflow
- Test workflows after updating actions
- Keep this README updated with new workflows

## Security

- All workflows use pinned major versions (e.g., `@v4`)
- Dependabot monitors for security updates
- Regular automated scans for deprecated versions
- Follow GitHub's security best practices

## Contributing

When adding new workflows:
1. Use the latest stable versions of actions
2. Pin to major versions (e.g., `@v4` not `@v4.1.2`)
3. Add appropriate caching where beneficial
4. Document the workflow in this README
5. Test thoroughly before merging
