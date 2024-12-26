# Overview

For a long time I have wanted a tool that could read a Markdown file and run shell code within
code-fenced blocks. In a lot of tutorials, people will describe something and provide a code sample
that you can copy to the clipboard and then run. My thought was: Why not have a Markdown processor
that can display the text part and provide a means to actually run the code blocks? By this means,
you can read some tutorial material and then watch as commands are executed. While not implemented
here, it also seemed that this might be a step in the direction of writing automated tests that
would verify that commands presented in a tutorial actually do what they say.

So this is not unlike Jupyter but I wanted it to be more basic and run right in the terminal. (For
something similar with Jupyter, check out
[zsh kernel for jupyter](https://github.com/dahn-zk/zsh-jupyter-kernel). You might also take a look
at [runme](https://docs.runme.dev/).)

I also wanted the Markdown file to look good when displayed with a regular Markdown processor as in
Github or the [Marked 2 application](https://marked2app.com/).

Running code turned out to be trickier than I thought. My first attempt was to use a Ruby
pseudo-terminal to run the commands and capture the output for display. But it was hard to get the
state right with a PTY. So I decided to write this in bash and use bash's eval functionality. By
doing so, the scripts can set environment variables in the current session, which is very convenient
for demonstrations. Of course there's some potential hazard here with a malicious tutorial, but
whatever. ¯\\\_(ツ)\_/¯

## Status

At this point the project is just a bunch of hackery.

## Setup

This project depends the `glow` command from Homebrew:

```
brew install glow
```

## How to run it

This repo provides a command-line app called `tutor` that can read a markdown file and display it by
h2 (`##`) sections. Just put it in your path.

When it encounters a fenced code block that is identified as `bash` with the `.run` attribute, it
will evaluate those commands in the current shell and will display the results.

Example:

````
## Easy commands

A lot of command-line commands are easy and trivial, for example
`date` and `pwd`.

```shell { .run }
date
pwd
```
````

## Limitations

* If there are loops inside a fenced code black, they must be on one line (no multi-line).
* If you want to change the stylesheet used for glow, edit the one in the templates directory,
and then compress and base64 it and put it into the tutor script.

## TODOs

* Provide a test harness. If the script to be delegated to `bash` is going to, say, create
a directory, it would be interesting to be able to test that and thereby validate that
the script actually works. (The reason I would want this is because I've seen tutorials
where the author has left out some detail and it doesn't really work.)
* It would be cool to be able to go backwards.
* Use the shell specified on the code fence.

## References

There was a lot of fiddly detail here, and I found the following resources helpful:

* Drawing: https://unix.stackexchange.com/questions/559708/how-to-draw-a-continuous-line-in-terminal
* ANSI escape codes: https://gist.github.com/fnky/458719343aabd01cfb17a3a4f7296797
* terminfo / tput: https://gist.github.com/izabera/9903f9d942e2667ef2cb
* Advanced tput: https://linuxcommand.org/lc3_adv_tput.php
* https://mywiki.wooledge.org/BashFAQ/037
* https://linuxcommand.org/lc3_adv_tput.php
* https://stackoverflow.com/questions/4842424/list-of-ansi-color-escape-sequences
* https://www.gnu.org/software/termutils/manual/termutils-2.0/html_chapter/tput_1.html
* https://github.com/Python-Markdown/markdown/blob/master/docs/extensions/fenced_code_blocks.md
