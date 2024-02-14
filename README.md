# GitHub Action: Workflow Keepalive

GitHub Action to prevent GitHub from [disabling scheduled workflows due to
repository inactivity][1].

[1]: https://docs.github.com/en/actions/learn-github-actions/usage-limits-billing-and-administration#disabling-and-enabling-workflows

## Usage:

Invoke this action in all scheduled workflows that you need to continue
running even if there's no activity in the repo:

```yaml
name: Tests

on:
  schedule:
    - cron: "0 0 * * *"

jobs:
  tests:
    name: Run integration tests
    steps:
      - uses: actions/checkout@v3
      # … whatever other steps you need to run your tests

  workflow-keepalive:
    if: github.event_name == 'schedule'
    runs-on: ubuntu-latest
    permissions:
      actions: write
    steps:
      - uses: liskin/gh-workflow-keepalive@v1
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
