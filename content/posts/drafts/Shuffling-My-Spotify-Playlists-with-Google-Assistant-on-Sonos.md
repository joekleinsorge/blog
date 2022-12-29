---
title: "Shuffling My Spotify Playlists With Google Assistant on Sonos"
date: 2022-12-29T12:11:42-05:00
draft: false
toc: true
cover:
tags:
  - go
---

## The Problem

I have a few themed Spotify playlists that I like to listen to when cooking and eating dinner ([Italian](https://open.spotify.com/playlist/6klxG6LmfYrgDeRrFDUsBd?si=1f554a67dcd348b4) when making pizza, [Mexican](https://open.spotify.com/playlist/3KqtjFkeRrtDbjlr0OClO1?si=c943e287981149e6) when making tacos, etc.). I have a Sonos system in my home and I use Google Assistant to control it. I wanted to be able to shuffle my playlists with Google Assistant. But found that I wasn't able to with the current Sonos<->Google integration.

## The Solution

So, I decided it would be a fun project to build and help me learn [Go](https://go.dev/). I looked around and found a wonderful [Spotify wrapper for Go](https://github.com/zmb3/spotify), that I could use to interact with the Spotify API.

I wrote a simply Go program that uses the Spotify wrapper to do a "hard" shuffle on a list of playlists. As of now, it requires user intervention to authenticate with Spotify. I plan to add a refresh token to the code and run it daily with a GitHub Action workflow in the future.

## The Code

The code is available on GitHub at [JoeKleinsorge/sonos-spotify-shuffle](https://github.com/JoeKleinsorge/sonos-spotify-shuffle)

## Running the Code

The code is written in Go and uses environment variables for configuration. The following environment variables are required:

- `SPOTIFY_ID` - The client ID for your Spotify application
- `SPOTIFY_SECRET` - The client secret for your Spotify application
- `SPOTIFY_PLAYLISTS` - A comma separated list of Spotify playlist names to shuffle

After setting the environment variables cloning the repo, you can run the code with the following command:

```bash
go run main.go
```

The code will prompt you to authenticate with Spotify. After authenticating, it will then  find the playlists playlists specified in the `SPOTIFY_PLAYLISTS` environment variable, figure out their Spotify IDs, and then randomly shuffle the tracks in each playlist.

## TODO

- [ ] Add a refresh token to the code
- [ ] Automate the code either with:
  - [ ] With a GitHub Action workflow run daily
  - [ ] Or with a serverless function on Google Cloud
    - [ ] Deploy the code to a [Google Cloud Run](https://cloud.google.com/run) instance
    - [ ] Expose the API with a [Cloud Endpoints](https://cloud.google.com/endpoints) API
    - [ ] Create a [Google Action](https://developers.google.com/assistant) that calls the API
