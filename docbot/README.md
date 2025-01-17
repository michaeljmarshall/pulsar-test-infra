# Documentation Bot 

Automatically label pull requests based on the checked task list.

## Usage

Create a workflow `.github/workflows/ci-docbot.yml` with below content:

```yaml
name: Documentation Bot

on:
  pull_request_target:
    types:
      - opened
      - edited
      - labeled
      - unlabeled

jobs:
  label:
    permissions:
      pull-requests: write
    runs-on: ubuntu-latest
    steps:
      - name: Checkout action
        uses: actions/checkout@v3
        with:
          repository: maxsxu/action-labeler
          ref: master

      - name: Set up Go
        uses: actions/setup-go@v3
        with:
          go-version: 1.18

      - name: Labeling
        uses: maxsxu/action-labeler@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          LABEL_WATCH_LIST: 'doc,doc-required,doc-not-needed,doc-complete,doc-label-missing'
          LABEL_MISSING: 'doc-label-missing'
```

## Configurations

| Name                    | Description                            | Default                   |
| ----------------------- |----------------------------------------| ------------------------- |
| `GITHUB_TOKEN`          | The GitHub Token                       | &nbsp;                   |
| `LABEL_PATTERN`         | RegExp to extract labels               | `'- \[(.*?)\] ?`(.+?)`' ` |
| `LABEL_WATCH_LIST`      | Label names to watch, separated by `,` | &nbsp; |
| `ENABLE_LABEL_MISSING`  | Add a label missing if none selected   | `true`                    |
| `LABEL_MISSING`         | The label mssing name                  | `label-missing` |
| `ENABLE_LABEL_MULTIPLE` | Allow multiple labels selected         | `false`                   |