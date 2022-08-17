---
title: transfer files with netcat
description: use netcat to easily transfer files
publication_date: 2021-07-27T01:25:29
section: technology
keywords: [netcat, nc, file-syncing]
---
As I don't use my [arch
laptop](/blog/btw_i_use_arch.html) that much, I've
figured that I can use it as a backup storage for my collections -
collections of memes and movies.

## Rsync is bloat?

Using some kind of a USB as an intermediary wouldn't do the trick,
because my meme collection is 14GB big (pathetic), whereas my movies
take 139GB. My first though was to use
[rsync](https://rsync.samba.org/examples.html). I've
never used rsync before, so I perused its man page and quickly found out
that you can use:

```shell
    rsync -vzP /some/path user@remote:/some/other/path
```

to sync the files. Then\... I got a ssh error. I couldn't be bothered
to install a ssh server on my laptop, because that would only bloat my
precious ***Arch*** Linux install. I continued the search.

## Rsync is bloat, so let's pipe stuff into some program

I've [stumbled on
this](https://superuser.com/questions/153176/how-to-rsync-a-large-file-with-as-little-cpu-and-bandwidth-expense-as-possible#answer-1020737)
StackOverflow answer. Not only does this guy have a method that would
allow me to transfer my movies without installing a ssh server, but he
also claims that it's faster (rsync shines more when it comes to
syncing [files that are
'similar'](https://fedoramagazine.org/copying-large-files-with-rsync-and-some-misconceptions/)).
It turns out that you can compress the files with tar and then use
netcat to send them over the network with:

```shell
    tar -cpv --atime-preserve=system /path/to/ | nc -q 10 -l -p 45454
```

on the source machine, and

```shell
    nc -w 10 X.X.X.X 45454 | tar -xpv
```

on the destination computer. The traffic isn't encrypted at all, but
that's not a problem over LAN (e.g sending movies to my laptop).
Albeit, [encryption can be provided with
ssh](https://screenage.de/blog/2007/12/30/using-netcat-and-tar-for-network-file-transfer/#comment-35)
or [with a
VPN](https://screenage.de/blog/2007/12/30/using-netcat-and-tar-for-network-file-transfer/#comment-936).

I think this method is very linux-like, because it combines two programs
in some unusual fashion to create something new. I might consider
replacing
[scp](https://www.man7.org/linux/man-pages/man1/scp.1.html)
with the ssh version of this trick.

