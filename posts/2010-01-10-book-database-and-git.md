slug: "book-database-and-git"
title: Book Database and Git
categories:
  - Linux
published_date: "2010-01-10 12:43:49 +0000"
layout: default.liquid
data:
  layout: post
  comments: true
  author: admin
  wordpress_id: 245
---
I haven't posted anything since before the end of year holidays as I have been
busy with a project. I have always wanted a simple database application to
manage my book collection. I have previously started work on a web based
solution using MySQL and PHP however I never finished it. I am currently working
on a Python application using curses and sqlite3. It is coming along nicely.
Sqlite3 has been great and I am very impressed with it. I plan on using it for a
couple of work related projects this year. This is the first project I have used
curses on and it has been a learning experience. Some of the UI code is still
quite untidy though I am slowly cleaning it up. I wanted to have the application
usable on a standard 80 column terminal and this has been a challenge. I am
studying Human Computer Interaction this semester and am hoping to pick up some
ideas regarding UI design. Not sure if they will translate from GUI to a text
mode interface though.

Below is a current screenshot. The status bar contains some debug info which
will be removed eventually.

[![Click to view full size.](/images/book_database_thumb.png)](/images/book_database.png)

Feature wise it is almost at the point where it is usable. I need to implement
record editing, record searching and clean up some of the database
initialisation code and it will be in good enough shape to release and start
using it.

While working on this project I have been using Git more extensively than usual.
I normally get by with `git init`, `git add` and `git commit` as my work is
normally fairly linear. With this project I have been using multiple branches
which has required the usage of `git reset`, `git rebase`, `git merge` and `git
show-branch`. `gitk` and `tig` have also been useful. I found Bart Trojanowski's
[Git presentation](http://excess.org/article/2008/07/ogre-git-tutorial/) along
with [Version Control with
Git](http://www.amazon.com/Version-Control-Git-collaborative-development/dp/0596520123)
by Jon Loeliger very helpful. I have also watched [Linus's Google Tech Talk Git
video](http://www.youtube.com/watch?v=4XpnKHJAok8) a couple of times simply for
its entertainment value.
