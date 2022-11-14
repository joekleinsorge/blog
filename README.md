# blog.kleinsorge.dev

[![Netlify Status](https://api.netlify.com/api/v1/badges/9d9f896a-16b7-42f9-b874-d2493b10be37/deploy-status)](https://app.netlify.com/sites/confident-ride-4e3bfb/deploys) [![Lighthouse Check](https://github.com/JoeyKleinsorge/blog/actions/workflows/lighthouse_check.yml/badge.svg)](https://github.com/JoeyKleinsorge/blog/actions/workflows/lighthouse_check.yml)

Code, articles and utilities for my [blog](https://blog.kleinsorge.dev).

- [blog.kleinsorge.dev](#blogkleinsorgedev)
  - [Introduction](#introduction)
  - [Hugo](#hugo)
  - [Netlify](#netlify)
  - [Theming](#theming)
  - [CI/CD](#cicd)

## Introduction

This is the code for my [blog](https://blog.kleinsorge.dev). It is a static site build from a this repo, generated with [Hugo](https://gohugo.io/) and [netlify](https://www.netlify.com).

## Hugo

This website uses the [Hugo](https://gohugo.io/) static site generator. For how to get started with it see this [guide](https://gohugo.io/getting-started/quick-start/).

## Netlify

This website uses [netlify](https:/netlify.com) to automate the build and deployment. For more information read their [guide](https://docs.netlify.com/?_gl=1%2a1y0luh0%2a_gcl_aw%2aR0NMLjE2MzE2Njk4MjYuQ2owS0NRandrSUdLQmhDeEFSSXNBSU5NaW9MUTRTMFliaDc5U24wVVVIcDlhRHRILWZ4U3RRWllVeldaRVljc1VsNW1takUtRlJLT3laY2FBcHcwRUFMd193Y0I.)

## Theming

The site uses the [hello-friend-ng](https://github.com/rhazdon/hugo-theme-hello-friend-ng) theme.

To use my own fork, which I use for slight modifications, run:

```
git submodule add https://github.com/JoeyKleinsorge/hugo-theme-hello-friend-ng.git
```

And update the `config.toml` to use this theme.

## CI/CD

Any changes to the `main` branch, will trigger Netlify to build and publish the commit.
