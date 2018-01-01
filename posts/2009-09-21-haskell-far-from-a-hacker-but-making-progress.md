slug: "haskell-far-from-a-hacker-but-making-progress"
title: Haskell. Far from a hacker but making progress.
categories:
  - Haskell
  - xmonad
published_date: "2009-09-21 10:18:26 +0000"
layout: default.liquid
data:
  wordpress_id: 148
  layout: post
  author: admin
  comments: true
---
Have been playing around with my xmonad.hs file and have come up with my first
bit of original Haskell though I have seen something similar in the config
archive. Since I have been using xmonad I have been using something like this
for my list of workspace names:

    myWorkspaces :: [String]
    myWorkspaces = ["1:dev", "2:web", "3:doc", "4:gfx", "5:tmp", "6:tmp", "7:tmp", "8:tmp", "9:tmp"]

This works fine and does the job but there are two things wrong with it.

  * Numbering the workspaces manually makes re ordering them a nuisance. For example swapping "dev" and "web" means swapping the 1 and 2.

  * Repeating the "tmp" is inelegant.

This is what I came up with tonight:

    myWorkspaces :: [String]
    myWorkspaces = map (\x -> show (fst x) ++ ":" ++ snd x)
                       (zip [1..9] $ words "dev web doc gfx" ++ repeat "tmp")

It's a lot more code for sure but it _is_ more maintainable.

  * The number of workspaces can be changed simply by changing the list [1 ..9].

  * Renaming the workspaces is as simple as editing the "dev web doc gfx" string.

These terms could be pulled out into a where clause to make it even easier. One
cool thing is that the **repeat "tmp"** function actually generates an infinite
list of "tmp" strings however due to Haskell's lazy evaluation only enough "tmp"
strings are generated to pad the list length out to match the length of the
first list argument to the zip function. That is nifty.

It's only a little chunk of code, which could probably be simplified, but it is
a start. [Baby steps][0].

Going to sit down in front of the TV tonight and watch [The Andromeda Strain][1].
I own the [original 1971 movie][2] on DVD as well as the novel and
have watched/read them several times. I wasn't aware there had been a remake
made and am looking forward to seeing what the modern interpretation looks like.

[0]: http://en.wikipedia.org/wiki/What_About_Bob/
[1]: http://www.imdb.com/title/tt0424600/
[2]: http://www.imdb.com/title/tt0066769/
