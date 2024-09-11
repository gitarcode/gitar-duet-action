# gitar-duet-action
Extends Gitar Bot with Additional Automated Changes

# Gitar Duet Action

Gitar Duet Action is a GitHub Action that extends Gitar Bot with additional automated changes. This action is specifically designed to work with Gitar Bot pull requests. Read more at [Gitar Docs](https://gitar.co/docs)

## Usage

To use this action in your workflow, add the following step:

```yaml
- name: Gitar Duet
  uses: gitarcode/gitar-duet-action@v1
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
```

Make sure to include this step after any steps that modify files in your repository.

## Inputs

- `github-token`: The GitHub token used for authentication. By default, it uses the `github.token` provided by the GitHub Actions runner.
- Make sure the action has `content:write` permission scope and `pull-requests:read` permission (if not already provided). See [example](#example-workflow)

## Example Workflow

Here's an example of how to use the Gitar Duet Action in a complete workflow:

```yaml
name: Gitar Duet

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  gitar-duet:
    if: startsWith(github.actor, 'gitar-bot') && startsWith(github.event.pull_request.user.login, 'gitar-bot')
    runs-on: ubuntu-latest
    name: Gitar Duet
    permissions:
      pull-requests: read
      contents: write

    steps:
      - uses: actions/checkout@v4
        with:
          ref: ${{ github.head_ref }}
          fetch-depth: 0

      # Add any steps that update files here

      - name: Run Gitar Duet Action
        uses: gitarcode/gitar-duet-action@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```
