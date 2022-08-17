---
title: website update 2
description: describing how I improved the website
section: maintenance
keywords: [website, update]
---
 1. the site's layout was completely overhauled, [see for yourself](/)
 2. there's fancy, new, CSS only 'slider' on the [bookmarks page](/bookmarks.html)
 3. added a table of contents to the rolling blog page
 4. added a light and a dark theme throught CSS, [how to enable dark mode in Chromium](https://wiki.archlinux.org/title/Chromium#Dark_mode)
 5. fixed RSS' date attribute on posts
 6. broke RSS - old posts may appear as new
 7. website's RSS will be moved to [here](/files/rss.xml)
 8. the 'link' buttons on the blog page now takes you to the article's title
 9. the posts are now written in markdown and then converted with pandoc to HTML
 so you can read them in plain markdown under /files/markdown/posts/POST-NAME, [here's this post](/files/markdown/posts/website_update_2.md)
 If you really want, you can even read them in PDF, assuming [pandoc](https://pandoc.org/) and [zathura](https://pwmt.org/projects/zathura/) are installed:
```bash
    pandoc -o - --to pdf --from markdown https://jacadzaca.github.io/files/markdown/posts/website_update_2.md | zathura -
```
 10. reworded the [dark mode article](/blog/browser_dark_mode.html)
 11. there's a [donation page](/donate.html) now

 Criticism or ideas are welcome.
