---
title: "Using Go to shuffle my Spotify playlists because Google Assistant on Sonos cannot"
date: 2022-12-29T12:11:42-05:00
draft: false
toc: true
cover:
tags:
  - go
  - spotify
  - sonos
---

## What's the Problem?

I have a few themed Spotify playlists that my family and I like to listen to while cooking and eating dinner ([Italian](https://open.spotify.com/playlist/6klxG6LmfYrgDeRrFDUsBd?si=1f554a67dcd348b4) when making pizza or pasta, [Mexican](https://open.spotify.com/playlist/3KqtjFkeRrtDbjlr0OClO1?si=c943e287981149e6) when making tacos, etc.).

I have a Sonos speaker setup at home and I use Google Assistant to control it. I wanted to be able to shuffle my playlists with Google Assistant but found that I wasn't able to with the current Sonos <-> Google integration.

Now, the easy solution is that I could pull out my phone and hit the shuffle button on the Spotify App. But what's the point of having a smart speaker if I have to pull out my phone to use it? Seems kind of silly to me, so I decided to figure out a solution.

## How was it solved?

I've been meaning to learn a bit of [Go](https://go.dev/) and decided it would be a fun project to build with it. I've found that building practical projects helps me retain me learn and retain more about a language than anything else.

And so I built a simple Go script that would shuffle the Spotify playlists I gave it.

I looked around GitHub and found a wonderful [Spotify wrapper for Go](https://github.com/zmb3/spotify), that I could use to interact with the Spotify API. This allowed me to focus on the playlist manipulation and not the underlying API calls.

The script uses the Spotify wrapper to authenticate with Spotify (it currently requires user intervention for the elevated permissions). It then uses the API wrapper to find the playlist IDs for the names specified in the `SPOTIFY_PLAYLISTS` environment variable, and then does a "hard" (meaning it overwrites the playlist order) shuffle of the tracks in each playlist.

#### Future Improvements

The code is currently run manually, which fulfills my immediate need. However, I do love automation and these shuffled tracks will get old again eventually.

So my plan is to either:

- Run the script weekly without intervention using a GitHub Action workflow (quick and easy)
- Or to run it at time of voice input using a Google Action and running it with Google Cloud Run (more complex, but something new to learn)

### How to run the script

You can find the script here [JoeKleinsorge/sonos-spotify-shuffle](https://github.com/JoeKleinsorge/sonos-spotify-shuffle)

The code uses a few environment variables for configuration. The following variables are required:

- `SPOTIFY_ID` - The client ID for your Spotify application
- `SPOTIFY_SECRET` - The client secret for your Spotify application
- `SPOTIFY_PLAYLISTS` - A comma separated list of Spotify playlist names to shuffle

After setting the environment variables and cloning the repo to your local, you can run the code with the following command:

```bash
go run main.go
```

The code will prompt you to authenticate with Spotify. After authenticating, it will then find the playlists playlists specified in the `SPOTIFY_PLAYLISTS` environment variable, figure out their Spotify IDs, then randomly shuffle the tracks in each playlist and overwrite the playlist order. Leaving you with permanently shuffled playlists.
