---
title: my calendar system<
description: how i control my time allocation
publication_date: 2021-06-18T23:42:06
section: lifestyle
keywords: [calendar, ics]
---
**TL;DR**: I spend too much time on the Internet, should find some
[frens](https://www.urbandictionary.com/define.php?term=fren)
and go out more lol?

Over the past three years, I've developed this odd habit of writing
down **everything** I do in my calendar. I've developed the habit
mostly out of curiosity: I wanted to know how much time I spend on the
Internet, playing video games, programming etc. Besides, I'm rather
sentimental.

Last week, I realized that having one calendar doesn't actually allow
me to do that, because each event has a unique name (e.g if a play some
video concrete game I write down that I'm playing concretely it). That
means that running some *simple* script, on the calendar, to sum the
events' durations would not be plausible (just imagine the regex!). I
should have several calendars, so that I could sort each event in to its
proper category.

## Categories

Coming up with the categories turned out to be more difficult that I've
anticipated. They should be both not too specific, yet not too broad, so
the 'time spent on X statistic' wouldn't be complete bullocks. Should
working out be considered its own category, or should I just put it
under the 'necessary, cannot live without it, but boring' label? Is
distro-hopping programming related? Should competitive programming be
considered as studying or as programming? In the end, I've converged on
these ten different categories:

-   relaxing (basically every time, when I don't do anything
    'productive' and don't browse the Internet)
-   studying (that includes reading books; dose not include homework)
-   programming (includes distro-hopping and such)
-   chores (includes homework, working, being at, going to and coming
    back from school, shopping etc.)
-   social (includes writing emails)
-   internet (does NOT include responsible usage; e.g. looking up public
    transport info)
-   games (video games, not board games or whatever - that goes into
    social)
-   movies (watching them, NOT going to cinema - that's social\...
    sometimes)
-   blog
-   life (the 'necessary, cannot live without it, but boring'
    category. Does include working out, because my life quality would
    decrease without it)

Splitting the calendar (nearly 10k events) proved to be impossible by
hand; wrote a
[script](https://raw.githubusercontent.com/jacadzaca/ical-utils/main/split_calendar.py)
for it. I concur that some events that should be in the 'chores'
category landed in the 'life' category, but only pedants would notice
that.

## Results

----------------------------------------------

[![](/files/images/my_calendar_system/calendar.avif)](/files/images/my_calendar_system/calendar.avif)

----------------------------------------------

Basically, over the last three years I've spent around 36.8% of my time
on mindless Internet browsing, while allocating about 2.3% of it to
being 'off the grid' (relaxing + social). On the other hand, I've
used about 37% of my time on doing useful stuff (i.e programming,
studying/reading and chores; the number goes down to 22.7%, if doing
chores would NOT to be included).

## Conclusion

Well, it's kind of problematic to draw conclusion from my
'experiment', because\... I have no clue what the average might be.
How can I tell if I'm doing 'good', whether I'm spending too much
(or too little?) time on the Internet, studying etc? Besides, the
[antecedent](https://www.merriam-webster.com/dictionary/antecedent)
(that's literally the fanciest word I know) presupposes that the
average even exist or is possible to 'compute'! Nonetheless, I surely
should try leaving my basement more often, because with that 2.3%
staying constant I will either lose eyesight or die from vitamin D
deficiency - not going outside is surely not
[lindy](https://www.urbandictionary.com/define.php?term=Lindy&page=2).
I also like how calendaring makes me think about my life - when you go
on to write down yet another five hour long mindless Internet browsing
session, you kinda start to doubt your life decisions lol. It kinda
functions like Benjamin Franklin's [13
Virtues](https://en.wikipedia.org/wiki/Benjamin_Franklin#Thirteen_Virtues)
\[[sample
chart](https://daringtolivefully.com/wp-content/uploads/2013/06/13-virtues-chart.jpg)\]

