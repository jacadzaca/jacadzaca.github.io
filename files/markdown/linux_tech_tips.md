---
title: Linux Tech Tips
descriptors: useful linux facts
section: technology
keywords: [linux, tips, commands, unix]
---

This is a list of useful facts about Linux that I\'ve discovered by
either reading the man pages or browsing the Internet.

-   With
    [atexit](https://www.man7.org/linux/man-pages/man3/atexit.3.html){target="_blank"}
    it is possible to call a function when the process terminates.
-   [You can list background processes in your current shell with
    [jobs](https://www.man7.org/linux/man-pages/man1/jobs.1p.html){target="_blank"}.]{#jobs}
-   [You can place a process into the foreground with
    [fg](https://www.man7.org/linux/man-pages/man1/fg.1p.html){target="_blank"}.]{#fg}
-   [/proc/ directory can turn out to be useful; especially if you are
    interested in process file descriptors.]{#proc}
-   [In order to find a process ID (a PID) of a running program, use
    [pidof](https://www.man7.org/linux/man-pages/man1/pidof.1.html){target="_blank"}.]{#pidof}
-   [You can determine file type of an file with the
    [file](https://www.man7.org/linux/man-pages/man1/file.1.html){target="_blank"}
    command.]{#file}
-   [There is a more *sophisticated* version of the
    [top](https://www.man7.org/linux/man-pages/man1/top.1.html){target="_blank"}
    command, it is called
    [htop](https://linux.die.net/man/1/htop){target="_blank"}.]{#htop}
-   [Have no clue where a file is? Using
    [find](https://linux.die.net/man/1/find){target="_blank"} gives no
    results? Try
    [locate](https://linux.die.net/man/1/locate){target="_blank"}, it
    searches the *whole* system!]{#locate}
-   [You can programmatically send desktop notifications with a [notify
    server](https://wiki.archlinux.org/index.php/Desktop_Notifications){target="_blank"}.]{#notify-servers}
-   [If you don not have you computer running 24/7, to schedule a job
    use
    [anacron](https://www.man7.org/linux/man-pages/man8/anacron.8.html){target="_blank"}.]{#anacron}
-   [You can attach to a process in order to see what system calls it
    makes with
    [strace](https://www.man7.org/linux/man-pages/man1/strace.1.html){target="_blank"}.]{#strace}
-   [With [w](https://www.man7.org/linux/man-pages/man1/w.1.html) you
    can see which users are logged in and what they are doing]{#w}
-   [With
    [watch](https://www.man7.org/linux/man-pages/man1/watch.1.html) you
    can execute a program perodicly and see its output]{#watch}
-   [Need a dead simple templating mechanism? Try
    [envstub](https://www.tutorialspoint.com/unix_commands/envsubst.htm)]{#envsubst}

