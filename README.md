# PR Refresh

A GitHub Action that automatically re-triggers workflow checks on all open pull requests whenever the main branch is updated. This helps prevent merging pull requests that might be incompatible with recent changes in the main branch.

## Purpose

When changes are pushed to the main branch, existing pull requests might become outdated or incompatible. This action:
- Automatically triggers checks on all open PRs when main branch updates
- Ensures PRs are tested against the latest main branch code
- Prevents merging of potentially incompatible PRs
- Maintains code quality by keeping PRs in sync with main

## Usage

```yaml

name: PR Refresh Workflow
on:
  push:
    branches:
      - main # or your default branch
  jobs:
    refresh:
      runs-on: ubuntu-latest
    steps:
      - name: PRs-Refresh
        if: github.ref == 'refs/heads/main'
        uses: debuide/pr-refresh@v1.0.0
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## How It Works

1. When new code is pushed to main branch
2. This action detects all open pull requests
3. Triggers a new workflow run for each open PR
4. Updates PR status based on latest main branch code
5. Prevents merging of PRs that fail checks against updated main

## Configuration

The action uses the GitHub token for authentication, which is automatically provided by GitHub Actions.

### Required Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `github-token` | Token for GitHub API access | Yes | `${{ github.token }}` |

## Example Scenario

1. Team member A creates PR #1
2. PR #1 passes all checks
3. Team member B pushes/merges changes to main
4. This action automatically re-runs checks on PR #1
5. If PR #1 is incompatible with new main changes, checks will fail
6. Team member A is notified to update their PR

## License

MIT

## Support

For issues or questions, please open an issue in the GitHub repository.

## Usage

Add this workflow to your repository (e.g., `.github/workflows/pr-refresh.yml`):
