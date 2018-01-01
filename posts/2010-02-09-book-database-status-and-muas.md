slug: "book-database-status-and-muas"
title: Book database status and MUAs.
published_date: "2010-02-09 03:37:02 +0000"
layout: default.liquid
data:
  comments: true
  wordpress_id: 261
  author: admin
  layout: post
---
Have felt like a little break from my [book database](http://blog.sambodata.com/?p=245) project. It needs one more weekend of polishing before it is usable. Hopefully I will get it done this weekend as it is the last weekend before it's back to the books.

I have been looking at MUAs recently and can't really find one that fits my needs exactly. [Sup](http://sup.rubyforge.org/) is probably the closest but I am not a huge fan of Ruby. Emacs' [RMail](http://www.gnu.org/software/emacs/manual/html_node/emacs/Rmail.html) and [VM](http://www.nongnu.org/viewmail/) are interesting. RMail feels a little primitive. VM looks very nice but I don't like how it adds a bunch of headers to the message. Not sure if you can turn this off or not. Both RMail and VM do support tags/labels though. I'm currently using Google Apps and have become quite used to labels and am not sure I could now do without them. After giving it some thought I have started throwing together an MUA prototype in Python. The features of an ideal MUA for **me** would be:




  * Does **not** retrieve mail via POP3 or IMAP. This is the job of an MTA or MRA. I use getmail for this purpose. 


  * Does **not** send mail via SMTP. This is the job of an SMTP client like msmtp.


  * Does **not** edit mail. I use Vim for my text editing needs and this is no different.


  * Does allow me to read, tag/label and search my mail.



Thanks to [Python's email package](http://docs.python.org/library/email.html) I have the basic viewing functionality working. I am not sure whether to store the message tags/labels in the messages themselves as custom headers or externally. I assume I will need an index to make searching fast so it may be best to include tag/label management in that. Sup uses [Xapian](http://xapian.org/) for it's indexing though I am thinking of writing my own and have been reading up on [inverted indexes](http://en.wikipedia.org/wiki/Inverted_index).

While Python has allowed me to get something running quickly I would like to eventually switch over to C. I would need to use a couple of third party libraries such as [pcre](http://www.pcre.org/), as well as ncurses of course, to make life easier. My ideal MUA is really only concerned with processing and displaying text. C will easily do this and I have been wanting to get my hands dirty with C again. The last time I did any significant C coding, GCC was at version 2.95. :)

