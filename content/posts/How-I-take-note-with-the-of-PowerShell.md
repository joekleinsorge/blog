---
title: 'How I take notes with the help of PowerShell'
date: 2021-09-14T18:23:49-04:00
draft: false
cover: img/notes.webp
tags:
  - PowerShell
  - Notes
---

A coworker of mine once showed me a cool python script he used to take notes in [vi](https://en.wikipedia.org/wiki/Vi). It seemed to simplify and organize how he took notes and at the time I was still trying to figure out a better method than I currently had (OneNote is not fast). I decided to build upon his idea and try to develop it using my preferred tools ( PowerShell and Visual Studio Code). Anyways here is what I created and how I use it.

The Code
For those that want to just read the code themselves or who just want to jump into using it, here's the script:

{{< gist JoeyKleinsorge f0af451afea2ac0ada2f0f654e4de423 >}}

## How I use it

I like taking notes but find that unorganized notes can be a bit of a mental burden instead of a useful helper. This system of note-taking is really basic in that most notes are titled by the day they were created. This helps keep them organized in the folder by date.

I store all my notes in a folder on my OneDrive, this allows me to access them from anywhere and makes sure that I won't lose them if my computer goes kaput. In the script, I set the directory parameter to default to this folder so that the script will always create/ search for the notes there unless I specify otherwise.

When I just want to take a quick note of something, I can just run the script as-is and it will pop open the daily note in VSC.

> `> Notes.ps1`

Say I want to create a note about a specific topic, however. I  can use the -name parameter to create a note with that name.

> `> Notes.ps1 -name terraform`

I can use that same parameter to open up a note that I specify if it already exists (If I ran the same command again it would open the terraform.txt file).

I can use the -list parameter to list out all the notes I have in the directory.

> `> Notes.ps1 -list`

One of my favorite uses of the script is the -search parameter, this will take the string input and do a pattern match against all the text files in the directory and bring back the exact path of the match to the line number as well as the other text on that line of the note.

> `> Notes.ps1 -search AWS`
