---
title: browser dark mode
description: how to enable dark mode in your browser?
publication_date: 2021-04-24T22:04:01
section: technology
keywords: [dark, mode, browser]
---
It turns out that you don't need fancy JavaScript to have your webpage change
the color theme based on user's preference.

You can access system's color theme preference with [\@media (prefers-color-scheme dark)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme).
Now your you only need to write two separate CSS styles and you're good to go.


Sadly, most pages don't seem to use the feature. It's good to know about it,
but for a dark mode **conesouir** (like myself) would yern for something better.

Inside of chrome (and Chromium based browsers, I assume) you can got to
the *chrome://settings/appearance* page, and set the theme
there to a some dark one. This only changes the looks of your
start page and the window's border, but it's a start.


We need to dig deeper.


A moment of browsing the internet, reveals the *chrome://flags/* page,
where you can the *Auto Dark Mode for Web Contents* option which, when enabled,
forces *every* page to render in dark theme. It breaks some pages a little
(especially the ones making extensive use of fancy CSS animations):

------------------------------------------------------------------------

[![](/files/images/browser_dark_mode/clunky.avif "there is some white stuff there, yuck!")](/files/images/browser_dark_mode/clunky.avif)

------------------------------------------------------------------------

[![](/files/images/browser_dark_mode/clunky1.avif "the empty levels are not visible, the puppets arms are barely visible")](/files/images/browser_dark_mode/clunky1.avif)

------------------------------------------------------------------------

but it surly doesn't render them unusable. Great stuff.

## What if I don't use Chrome?

You're probably stuck with using [Dark Reader](https://darkreader.org/) then.
It works with every popular browser, although it in my opinion it produces
worse looking webpages than the native Chromium's feature. If you're a using
an extensible browser like [surf](https://surf.suckless.org/) there's probably
a script or even a CSS file that does the same. Although, the kinds of people
that use browsers like surf probably have known about since before I was born
or something (I'm bad at role-playing a boomer, sry)

