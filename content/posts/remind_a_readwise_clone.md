---
title: "Remind: A Readwise Clone"
date: 2023-06-26T13:26:47-04:00
draft: false
toc: false
cover:
tags:
  - go
  - github
  - actions
---

I have been starting to play with Go and was looking for a (very) small project to tackle with it, when my [Readwise](https://readwise.io/) free trial ran out.
For those who don't know, Readwise is a service that sends you a daily email with a few random highlights from books you've read.

I love to read with my kindle and use the highlight feature to mark interesting passages, with the intent of reviewing them latter.
I really enjoyed that Readwise would send me a few random highlights from my Clippings file (the .txt file that Kindle stores all your hightlights in).
So I figured I would build a small script that did the same.

This blog post explores how I built it, if you'd just like to use it, follow the instructions in the [README](https://github.com/JoeKleinsorge/remind#prerequisites).

## Step 1: Understanding the Problem

Before I write any code, I like to capture what my requirements are.
In this case, I wanted a script, written in golang, that could parse a Kindle clippings file, select a variable amount of highlights from it and send that in a nicely formatted email once a day.

## Step 2: Deciding on how to handle the requirements

Working backwards:

- To handle the triggering of the script, I decided to use a GitHub Action workflow to run it on a cron schedule
- To handle the nice email formatting, I found that an email template would work best
- To handle the sending of the email, a quick search found that go has sendgrid library and sendgrid has a free tier that suites my use case
- To handle the parsing, regex will always be the winner
- To handle the random selection, I will a random seed and shuffle the array of highlights


## Step 3: Write the test first

Test driven development is a great way to ensure that your code is doing what you expect it to do.
There wasn't much to test in this case, but I did write a few tests to ensure that the parsing and randomization of the clips were working as I expected.

```go
package main

import (
	"reflect"
	"testing"
)

func TestParseClippings(t *testing.T) {
	data := "clipping 1(Author 1) on page 1 Added on 2023-06-01\nHighlight 1\n==========\nclipping 2(Author 2) on page 2 Added on 2023-06-02\nHighlight 2\n"
	expected := []Clipping{
		{
			Title:     "clipping 1",
			Author:    "Author 1",
			Page:      "1",
			When:      "2023-06-01",
			Highlight: "Highlight 1",
		},
		{
			Title:     "clipping 2",
			Author:    "Author 2",
			Page:      "2",
			When:      "2023-06-02",
			Highlight: "Highlight 2",
		},
	}
	result := parseClippings(data)

	if len(result) != len(expected) {
		t.Errorf("Unexpected number of clippings. Got: %d, Expected: %d", len(result), len(expected))
	}

	for i := range result {
		if result[i].Title != expected[i].Title ||
			result[i].Author != expected[i].Author ||
			result[i].Page != expected[i].Page ||
			result[i].When != expected[i].When ||
			result[i].Highlight != expected[i].Highlight {
			t.Errorf("Unexpected clipping at index %d. Got: %+v, Expected: %+v", i, result[i], expected[i])
		}
	}
}

func TestSelectRandomClippings(t *testing.T) {
	clippings := []Clipping{
		{
			Title:     "clipping 1",
			Author:    "Author 1",
			Page:      "1",
			When:      "2023-06-01",
			Highlight: "Highlight 1",
		},
		{
			Title:     "clipping 2",
			Author:    "Author 2",
			Page:      "2",
			When:      "2023-06-02",
			Highlight: "Highlight 2",
		},
		{
			Title:     "clipping 3",
			Author:    "Author 3",
			Page:      "3",
			When:      "2023-06-03",
			Highlight: "Highlight 3",
		},
		{
			Title:     "clipping 4",
			Author:    "Author 4",
			Page:      "4",
			When:      "2023-06-04",
			Highlight: "Highlight 4",
		},
	}

	selectedClippings := selectRandomClippings(clippings, 2)

	if len(selectedClippings) != 2 {
		t.Errorf("Unexpected number of selected clippings. Got: %d, Expected: 2", len(selectedClippings))
	}

	for _, clip := range selectedClippings {
		found := false
		for _, c := range clippings {
			if reflect.DeepEqual(clip, c) {
				found = true
				break
			}
		}
		if !found {
			t.Errorf("Selected clipping not found in the original clippings. Clipping: %v", clip)
		}
	}
}
```

## Step 4: Write the code

I won't go into the specifics of the code here, but you can check out the [remind.go](https://github.com/JoeKleinsorge/remind/blob/main/remind.go) file in the repo. The code is pretty simple, but I did have to do some research on how to use the sendgrid library and how to parse the clippings file.

## Step 5: Set up the GitHub Action(s)

As a "DevOps" engineer in my day job, the first thing I setup was a ci.yaml workflow that would run the tests on every push to the main branch.

```yaml
name: CI

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Set up Go
        uses: actions/setup-go@v2
        with:
          go-version: 1.16

      - name: Resolve missing dependency
        run: go mod download github.com/davecgh/go-spew

      - name: Verify dependencies
        run: go mod verify

      - name: Build
        run: go build -v ./...

      - name: Run go vet
        run: go vet ./...

      - name: Install golint
        run: go install golang.org/x/lint/golint@latest

      - uses: golangci/golangci-lint-action@v3.4.0
        with:
          args: "--out-${NO_FUTURE}format colored-line-number"

      - name: Run tests
        run: go test -race -vet=off ./...

```
Now for the actual orchestration of the script, I wanted to run the script once a day, so I set up a cron job in the [workflow](https://github.com/JoeKleinsorge/remind/blob/main/.github/workflows/remind.yml) to trigger the script at 5:00 UTC every day and send me an email with the highlights before I wake up.

```yaml
name: Remind

on:
  schedule:
    - cron: "0 5 * * *"
  workflow_dispatch:

jobs:
  readwise_clone:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Set up Go
        uses: actions/setup-go@v4
        with:
          go-version: 1.16

      - name: Resolve missing dependency
        run: go mod download github.com/davecgh/go-spew

      - name: Send email
        env:
          SENDGRID_API_KEY: ${{ secrets.SENDGRID_API_KEY }}
          RECIPIENT_EMAIL: ${{ secrets.RECIPIENT_EMAIL }}
          SENDER_EMAIL: ${{ secrets.SENDER_EMAIL }}
        run: go run remind.go

```

## Step 6: Profit

I now have a script that runs once a day and sends me a few random highlights from my Kindle Clippings file.
This was a fun little project to work on and I learned a lot about Go and automated emails in the process.
Feel free to clone the repo and use it for yourself, or modify it to fit your needs. The README has instructions on how to set it up for yourself.
