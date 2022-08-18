---
title: simple code generator
description: generating repetitive code snippets
section: technology
keywords: [code generation, cpp, morse code]
---
Last semester I was taking a mandatory \"Programming 101\" course, aka.
drawing stairs made out of stars and watching my classmates struggle
with basic tasks, the course - a time truly well spent. Even though the
course problems weren't too enlightening, the silliness of one of the
tasks' requirements gave me a chance to write a simple code generator.

## Problem

Without using an associative table (they weren't covered in class),
write a program that encodes a message using [morse
code](https://en.wikipedia.org/wiki/Morse_code). The
characters should map as follow (yes, a picture; who needs copy-paste
anyway?):

------------------------------------------------------------------------

[![ASCII to morse
mapping](/files/images/simple_code_generator/morse_code_mapping.avif "ASCII to morse mapping")](/files/images/simple_code_generator/morse_code_mapping.avif)

------------------------------------------------------------------------

## Implementation

First, I looked online for the table in a [text
format](https://stackoverflow.com/questions/48563810/why-does-my-morse-code-converter-display-no-output),
downloaded it and, using a [vim
macro](https://vim.fandom.com/wiki/Macros), formatted
it so that each line would contain one of the ASCII symbols and its
Morse representation separated by a space. Since I wanted to use
[envsubst](https://www.tutorialspoint.com/unix_commands/envsubst.htm),
I created a file like so:

```bash
    else if (morse_representation == "${CASE}") {
        return '${VALUE}'
    }
```

Then, I wrote a script that reads each line from the 'translation
file', and places the Morse representation inside the *if's* condition
and the ASCII symbol next to the *return* statement:

```bash
    #!/usr/bin/env sh
    while read line; do
        line=($line)
        export CASE=${line[1]}
        export VALUE=${line[0]}
        truncate -s-1 from_morse_code.cc
        envsubst < switch_case.cc >> from_morse_code.cc
    done < morse_code.txt
```

I ran it, ran it again with the values of the *CASE* and *VALUE*
variables switched as to generate the other way translation, removed the
unneeded *elses* at the bottom, copy-pasted the big *if* chains into the file where I
would write the wrapper code\... and voila! An hour of time was just
saved (if you're not counting the hour it took me to write this lol)!

