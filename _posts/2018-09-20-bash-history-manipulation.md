---
layout: post
title:  "Bash History Manipulation"
---

Just another blog post describing another rookie mistake (_the most fun kind of blog post_). You get a gold star if you can predict the mistake that I will make.

Today I wanted to rerun a command from my bash history.

I know that I can run `history` to get a list of my past commands, and I can then run `history | grep <search_term>` to pull back some nice clues to what I want to do (given that I have usually forgotten the _exact_ command).

However, I was guessing that the people that wrote `history` must have been clever enough to include functionality to directly run commands from the history command.

My normal debugging process was "helpful" here.
```
Peters-MacBook-Pro:~ peterreeves$ history --help
-bash: history: --: invalid option
history: usage: history [-c] [-d offset] [n] or history -awrn [filename] or history -ps arg [arg...]
```
And `man history` gave me the big BSD blurb..
```
BUILTIN(1)                BSD General Commands Manual               BUILTIN(1)

NAME
...
```

I noticed the -c flag from the `history: usage:` output and I decided to have a guess at `history -c 2133`. "_This must run the command #2133 from the output of history! I bet the -c stands for "count", like `ping -c`!_"
My computer hung for about 10 seconds. I panicked and googled.

Turns out the `-c` stands for clear.

I lost my >2200 command bash history. It slightly felt like the first time that you run `rm -rf` from the wrong directory, usually to wipe out your last hour's work.

I should have guessed, given by the "BSD General Commands Manual" man page, that running a command from one's history would be built into bash. Turns out, the correct way to run command 2133 from your history is `!2133`. It is so obvious now.
