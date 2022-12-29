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

I have a few themed Spotify playlists that I like to listen to while cooking and eating dinner ([Italian](https://open.spotify.com/playlist/6klxG6LmfYrgDeRrFDUsBd?si=1f554a67dcd348b4) when making pizza, [Mexican](https://open.spotify.com/playlist/3KqtjFkeRrtDbjlr0OClO1?si=c943e287981149e6) when making tacos, etc.). I have a Sonos system at home and I use Google Assistant to control it. I wanted to be able to shuffle my playlists with Google Assistant but found that I wasn't able to with the current Sonos<->Google integration. Of course I could have pulled out my phone and hit the shuffle button on the Spotify App, but between an infant and dinner to make, I didn't have time for that.

## The Solution

So, I've been meaning to learn [Go](https://go.dev/) and decided it would be a fun project to build with it. I've found that building practical projects, helps me retain me learn and retain more about a language. With that in mind, I decided to build a simple Go script that would shuffle my Spotify playlists.

I looked around and found a wonderful [Spotify wrapper for Go](https://github.com/zmb3/spotify), that I could use to interact with the Spotify API. This allowed me to focus on the playlist manipulation.

The script uses the Spotify wrapper to authenticate with Spotify (as of now, it requires user intervention, for the elevated permissions). It then uses the wrapper to find the playlist IDs for the names specified in the `SPOTIFY_PLAYLISTS` environment variable, and then does a "hard" shuffle of the tracks in each playlist.

### TODO

The code is currently run manually, which fulfills my immediate need. But I do love automation, so I plan to add a refresh token to the code and either run it daily with a GitHub Action workflow (Quick and easy) or use it as an excuse to learn more about creating a serverless Google Action and running it on Google Cloud Run (More complex, but something new).

## The Code

The code is available on GitHub at [JoeKleinsorge/sonos-spotify-shuffle](https://github.com/JoeKleinsorge/sonos-spotify-shuffle)

## Running the Code

The code uses a few environment variables for configuration. The following environment variables are required:

- `SPOTIFY_ID` - The client ID for your Spotify application
- `SPOTIFY_SECRET` - The client secret for your Spotify application
- `SPOTIFY_PLAYLISTS` - A comma separated list of Spotify playlist names to shuffle

After setting the environment variables and cloning the repo to your local, you can run the code with the following command:

```bash
go run main.go
```

The code will prompt you to authenticate with Spotify. After authenticating, it will then  find the playlists playlists specified in the `SPOTIFY_PLAYLISTS` environment variable, figure out their Spotify IDs, and then randomly shuffle the tracks in each playlist.
