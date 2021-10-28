# JoeyKleinsorge.com

[![Netlify Status](https://api.netlify.com/api/v1/badges/9d9f896a-16b7-42f9-b874-d2493b10be37/deploy-status)](https://app.netlify.com/sites/confident-ride-4e3bfb/deploys)

Code, articles and utilities for the [JoeyKleinsorge.com](https://JoeyKleinsorge.com) website.

- [Introduction](#introduction)
- [Hugo](#hugo)
- [Netlify](#netlify)
- [Theming](#theming)
- [CI/CD](#cicd)

## Introduction

This is the code for the [JoeyKleinsorge.com](https://JoeyKleinsorge.com) website. It is a static site build from a repository hosted at [github.com/JoeyKleinsorge/JoeyKleinsorge.com](https://github.com/JoeyKleinsorge/JoeyKleinsorge.com) generated with [Hugo](https://gohugo.io/) and [netlify](https://www.netlify.com).

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

The address of the deployed site is: https://JoeyKleinsorge.com/
