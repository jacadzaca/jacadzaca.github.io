---
title: btw i use arch
description: my arch linux setup
publication_date: 2021-05-23T15:29:14
section: technology
keywords: [arch, setup]
---
I've finally got around to installing Arch Linux on my laptop.
Installing Arch was simple enough, the only problem that I've had was
understanding the difference between a boot partition and a 'boot
partition'. A BIOS boot partition is a type of a partition, whereas a
'boot partition' is a partition you create for your /boot/ directory -
you don't actually need a 'boot partition', while a boot partition is
more or less required - manually telling your boot loader (most likely
GRUB) where everything is located is way too tedious.

## Programs I run

The first thing that I installed was GNOME3. I run GNOME on my Ubuntu
desktop and it works just fine, I especially like the DE; it looks nice
out of the box and the Activities tab is amazing. But that's mutter36
(mutter is the GNOME DE). mutter40 is nothing like it. As of now I run
[xfce4](https://wiki.archlinux.org/title/Xfce) on my laptop and it's
alright, albeit a little retro. Actually, xfce's so vintage that it
might be the best desktop environment for an eldery person, but that's
neither here or there. I'm actually thinking of switching to
[dwm](https://dwm.suckless.org/) or [i3](https://i3wm.org/), but I'm
yet to decide which one is better. For the login manager I've chosen
LightDM - it just worked.

I don't run
[NetworkManager](https://developer.gnome.org/NetworkManager/stable/NetworkManager.html),
because I have no need for it - just connecting to wi-fi with
[iwd](https://wiki.archlinux.org/title/Iwd) and running a dhcpcd daemon
in the background works for me. I also installed an [iwd frontend
program](https://github.com/J-Lentz/iwgtk), because some normal person
might have to input a wi-fi password into my laptop someday.

I use [udisks](https://wiki.archlinux.org/title/Udisks) with
[udiskie](https://man.archlinux.org/man/community/udiskie/udiskie.8.en)
for automating USB mounting. I also run gvfs, so I can have a Trash bin
(which is less useful than I've thought). gvfs also seems to provide an
alternative to [insync](https://wiki.archlinux.org/title/Insync), but it
remains unclear whether you can actually use gvfs effectively without
all the GNOME bloat.

I use [CUPS](https://wiki.archlinux.org/title/CUPS) directly for printer
manging. I even have a desktop shortcut to a [chromium
app](https://www.mankier.com/1/chromium-browser#--app) that starts the
CUPS web gui.

I also considered installing a cron-like program (most probably
[anacron](https://en.wikipedia.org/wiki/Anacron)) and running a simple
pacman update script, but after a little
[research](https://bbs.archlinux.org/viewtopic.php?id=83118), I've
decided to postpone creating such cron job. If after a while of updating
nothing breaks, I will surely consider it.

## Final thoughts

As of now, Arch Linux is my favourite distro; it boots faster than
Ubuntu, it eats a lot less RAM (which is cool, becouse now I can brag on
the internet about how little memory my setup needs) and the package
manager seems to be full of useful programs, especially compared to apt.
Also, there's AUR, which looks like a both more managable and faster
version of snap,
