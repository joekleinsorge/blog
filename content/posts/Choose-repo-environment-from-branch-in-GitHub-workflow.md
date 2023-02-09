---
title: "Choose Repo Environment From Branch in GitHub Workflow"
date: 2023-02-09T09:13:21-05:00
draft: false
toc: false
cover:
tags:
  - github
  - actions
---

An issue I've run into a few times is that I want to run a GitHub Action workflow on a branch, but I want to use different environment variables and secrets for that branch than I do for the default branch.

For example, I may want to run a workflow on my `development` branch that uses a different go version and token than the `production` branch.

## How to do it

As of right now, the easiest way to do this is to use built-in  [`GITHUB.REF_NAME`](https://docs.github.com/en/actions/learn-github-actions/variables#default-environment-variables) environment variable to set the `environment` key in the workflow file.

This key allows you to specify the environment that the workflow will run in. This is useful for things like setting environment variables and secrets for the workflow.

{{< gist JoeKleinsorge 58a1c953207f4fd98d77fdfc8401935f >}}

```yaml
name: Using the Branch Name to select Environment

on:
  workflow_dispatch:

jobs:
  test:
      runs-on: ubuntu-latest
    # Run the following steps with the environment
    # variables based on what branch was chosen
      environment: ${{ github.ref_name }}
      steps:
        - name: Checkout
          uses: actions/checkout@v3

        - name: Setup Go environment
          uses: actions/setup-go@v3.5.0
          with:
          # Use environment variable for Go version
            go-version: ${{ env.GO_VERSION }}
          # Use environment secret for token
            token: ${{ secrets.GO_TOKEN }}
```
