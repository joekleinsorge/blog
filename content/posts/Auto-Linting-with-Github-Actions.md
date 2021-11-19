---
title: 'Auto Linting With Github Actions'
date: 2021-11-18T19:29:21-05:00
draft: false
toc: true
cover: img/gitHubActions.png
tags:
  - git
  - GitHub_Actions
---

## Why use a Linter?

Linting ( the automated checking of source code for programmatic and stylistic errors ) is important, because it reduces errors and improves the overall quality of your code. Automating the linting so that it happens every time you push to your repo is a way to make to make your life easier. Nobody likes to sift through messy code, right?

## Why use GitHub Actions?

Because most of my repos live on GitHub and they made Actions very easy to use. They also have some great [documentation](https://docs.github.com/en/actions/quickstart).

## Steps

### Add the folders structure

GitHub looks for Actions (which are defined using **YAML**) files in the **.github/workflows** directory. So, first we have to create the directories in our repo's root.

```shell
mkdir -p .github/workflows
```

It should look like this:

```shell
repo
└──.github
     └── workflows
```

### Create linter YAML file

With the directory setup, we just have to create the linter.yaml file, which looks like this:

```yaml
---
name: Lint Code Base

on:
  push:

jobs:
  build:
    name: Lint Code Base
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v2
        with:
          fetch-depth: 0

      - name: Lint Code Base
        uses: github/super-linter@v4
        env:
          VALIDATE_ALL_CODEBASE: false
          DEFAULT_BRANCH: main
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Push the code and watch the linter run

Pushing your commit to Github will trigger the Action to run [super linter](https://github.com/github/super-linter) against your code .

head over the **Actions** tab in your repo.

{{< image src="/img/gitHubActionsScreenshot.JPG" alt="Github Actions Screenshot" style="border-radius: 8px;" >}}

### Check the workflow run

Check the output of Super Linter and see if there is anything you need to tidy up.

{{< image src="/img/lintCheck.JPG" alt="Lint Check" style="border-radius: 8px;" >}}

## Conclusion

If you haven't used GitHub Actions before, check them out. They offer a ton of functionality and this is just a very basic use case.

## Extra

You can also add this fun badge to your README to show off your code's cleanliness.

{{< image src="/img/lintBadge.JPG" alt="Lint Badge" style="border-radius: 8px;" >}}

Just add the below snippet where you want it to show up.

```shell
[![GitHub Super-Linter](https://github.com/<OWNER>/<REPOSITORY>/workflows/Lint%20Code%20Base/badge.svg)](https://github.com/marketplace/actions/super-linter)
```
