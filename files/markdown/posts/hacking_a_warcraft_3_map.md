---
title: hacking a Warcraft 3 map
description: use m4d h4ck32 5k115 to unprotect a warcraft 3 map
publication_date: 2021-12-29T16:15:35
section: technology
keywords: [warcraft, warcraft-3, map-deprotecting, map, protection]
---
I'm a big Warcraft fan. Actually, I consider Warcraft 3 to be the best
video game of all time. I like the story, the voice-acting (polish
voice-over was great until Reforged), and the music fits game-play. But
what's best about Warcraft 3 is its universal game engine that combined
with the powerful game editor would lead to creation of many unique
mods. Of course, I have my favorite: a polish modification called
*Alchemicy* - a combination of a hero defense and a crafting game.
Although the mod is loads of fun, one aspect of it always bugged me:
there are characters that you can only play as after either making a
donation to the author or if you know him in-game. What's more, the
donation service was [shut-down in
2012](https://warrttssgames.blogspot.com/p/premium-alchemicy.html)
with the guy quting the game. Since the map is protected, meaning nobody
can open the mod file in the editor, the characters seemed to have been
lost to the sands of time\... Until I figured out a way to produce a
patch that removes the paywall.

## Protections, how do they work?

First, a brief introduction to the Warcraft map file format (w3x). For
my purposes, every Warcraft 3 mod is basically an archive ([see here for
proper
docs](https://world-editor-tutorials.thehelper.net/cat_usersubmit.php?view=42787))
that contains [a few
files](https://github.com/ChiefOfGxBxL/WC3MapTranslator#file-support),
each relating to different aspects of the mod. [Certain
files](http://www.3ice.hu/blog/protect-warcraft-maps/#delete)
(from now on - *header files*) only carry data that's required by the
game editor to open up the map, other files are mandatory (for example,
the map file), there also might be some auxiliary files; most probably
custom models, icons or sounds. What's crucial is that each file
contains information that allows a human to identify it (e.g, the file
defining custom units contains units' names, hitpoints etc.)

All map protection software dose is remove the header files, therefore
making the editor crush each time you try to open a protected map with
it. It also removes the MPQ's (name of the archiving format) listfile,
therefore obfuscating files' names.

## The plan

Since I'm familiar with the Warcraft ecosystem, a glance at the map
in-game revealed that there must be a certain upgrade that is granted to
all 'premium' users. Since all of the custom upgrades are in the
*war3map.w3q* file, I figured that all I need to do is:

identify the .w3q file

byte patch the mod-file directly or:

i.  convert the file into editable form
ii. make applicable changes
iii. convert the file back and patch the mod-file using an editor

Sadly, this approach doesn't work, because:

1.  files in the MPQ are somehow compressed, so you cannot byte-patch
    them directly
2.  the MPQ's listfile is gone, so you cannot add new files. You cannot
    replace a file without adding a file
3.  the .w3q files only cotains the upgrade's definition (ID,
    description etc). The correct place to apply the patch is in the
    *war3map.w3u* file

Luckily, the new insight allowed me to conjure an approach that works:

1.  acquire a Warcraft map with a listfile - a template
2.  extract and identify the files inside the *Alchemicy* mod file
3.  make applicable changes to the *war3map.w3u* file
4.  replace the files inside the template with the ones extracted

We are going to need:

1.  the [MPQ
    editor](http://www.zezula.net/en/mpq/download.html)
    (it runs under Wine) to both extract, and overwrite files found in a
    .w3x file
2.  [this w3x
    parser](https://github.com/ChiefOfGxBxL/WC3MapTranslator/tree/v1.1.0)
    to identify the files
3.  [*Alchemicy* map
    file](https://www.epicwar.com/maps/download/231379/688fa38f47c622d9e3e3afddb95520c5a2a4707c64c5ec23c5ee994b1e3826f461c63b9f/Alchemicy%203.22v.w3x){download=""}
4.  a 128 by 128, *Cityscape* tileset, Warcraft map to use as a template

## The implementation

First, I downloaded one of my old, unfinished maps for a template. Then,
I created two directories: *identified-files* and *unidentified-files*
and extracted all files from the map (save for .blp and .mdx files) into
the second one. Second, I noticed that it's possible to identify few of
the files just by either looking at them or by opening them in your
[favorite text editor](https://neovim.io/):

1.  since File00000000.w3e has an extension, it's probably the
    war3map.w3e file
2.  by comparsion with your template map, File00000001.xxx is the
    war3map.w3i file
3.  since File00000002.xxx is fillied with strings, it must be the
    war3map.wts file
4.  since File00000003.xxx looks like computer code, it must be the
    war3map.j file
5.  since File00000007.wpm has an extension, it's probably the
    war3map.wpm file
6.  the File0000000008.doo is either the the war3map.doo or the
    war3mapUnits.doo
7.  by comparsion with your template map, File00000015.xxx is the
    war3mapMisc.txt file
8.  by comparsion with your template map, File00000016.xxx is the
    war3mapExtra.txt file

I renamed the identified files and moved them into the other directory.
I also ran [this example
script](https://raw.githubusercontent.com/ChiefOfGxBxL/WC3MapTranslator/v1.1.0/examples/jsonToWar/entity/doodads.js)
that is provided by the parser to figure out that the .doo file is the
war3Map.doo file. Nearly all other files can be identified using [this
script](https://gist.githubusercontent.com/jacadzaca/9f094f8aa8131278a375f5d96caeb748/raw/4be413027c1444b3e9ebf27d6306258371e05cd5/check.sh),
after applying this 'patch' inside the
*WC3MapTranslator/examples/warToJson* directory:

`                     find . -name '*js' -exec sed -i -e 's|fs.writeFileSync(jsonPath, |console.log(|g' {} \;                 `

File00000004, File00000006 and File00000012 couldn't be parsed into
anything sensible. I would later figure out that (using a method of
trail and error) File00000004 is the .shd file and that File00000006 is
the .mmp file. I couldn't figure what File00000012 is.

Thirdly, I would convert both the [.w3q]() and the
[.w3u file]() into JSON and compare them:

------------------------------------------------------------------------

[![a side-by-side comparision of JSON representation of the .w3q and
.w3u](/files/images/hacking_a_warcraft_3_map/json_comparison.png "a side-by-side comparision of JSON representation of the .w3q and .w3u")](/files/images/hacking_a_warcraft_3_map/json_comparison.png)

------------------------------------------------------------------------

I figured that the simplest solution would be to blank-out the *value*
record with:

`                     sed -i -e 's|R000||g' units.json                 `

Then, I would convert the edited [JSON back into a .w3u
file](https://raw.githubusercontent.com/ChiefOfGxBxL/WC3MapTranslator/v1.1.0/examples/jsonToWar/object/units.js)
and continue as planned.

## Conclusions

Although the patch works (see \[1\]), it's somehow rough around (points
\[2-5\]) the edges:

------------------------------------------------------------------------

[![a side-by-side comparison of original Alchemicy map and its
'hacked'
version](/files/images/hacking_a_warcraft_3_map/hero_selection_comparison.avif "a side-by-side comparison of original Alchemicy map and its 'hacked' version")](/files/images/hacking_a_warcraft_3_map/hero_selection_comparison.avif)

------------------------------------------------------------------------

I also think it's also worth mentioning that:

although my way of identifying the files works, it turns out that [MPQ
Editor](http://www.zezula.net/en/mpq/war3maps.html)
automates this process.

although the patched map can be opened in the editor, the game crushes
when I try to launch the map

there exist [a
program](https://www.youtube.com/watch?v=98nXuBiKVig)
that can automate the whole process, but:

i.  it's literally a sketchy, Russian program from the internet
ii. I've never seen any explanation of it's inner workings

I later used the MPQ Editor's *Name Scanner* feature to fix issues
\[2-5\], it turns out that:

i.  File00000016 is actually war3mapSkin.txt - renaming the file fixes
    \[2\] and \[4\]
ii. one of the .blp files was the wa3mapMap.blp - including this file in
    the patch fixes \[3\]
iii. other .blp and .mdx files were custom models/icons - including them
     in the patch fixes \[5\]

*warcraft3mapUnit.doo* is a header file

