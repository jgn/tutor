# Example tutorial file

A section at the h1 (`#`) level is ignored. By this means you can have some non-printing front
matter. This means that you can have an overview section on Github, say, that is not shown when
running a tutorial. But after a bit let's repeat this now so it *does* show when run in the
tutorial.

## What this is for

The `tutor` command provides for paging through a Markdown tutorial with certain sections getting
delegated to your shell so that the commands will be executed in your session. By this means, you
can see the actual result of commands. You could use this to write up the creation of a Rails
project, and have the `tutor` actually build it for you.

(Of course, a particular tutorial could do malicious things.)

This file itself is such a tutorial, and you can run it with

```shell
tutor example.md
```

## Example section

The `tutor` script takes special notice of certain sections. A section starting with `##` will be
formatted as Markdown. After presentation, the user may press return to continue. If for some reason
the user has changed the terminal width, the user may press `r` to re-render. Before running
commands, the user may press `s` to skip the execution. There is finally a `q` option to quit.

The section introduced with `##` may itself have other sections.

### So this subhead is fine

#### And so is this one

(Notice that the Markdown formatter renders subheads at `####` with the pound signs. This can be
changed in a style sheet.)

The difference between `##` and `###` and `####` is that the display will be paused for sections
introduced with `##`.

## Top-level `#`

A section at the h1 (`#`) level is ignored. By this means you can have extra explanatory front
matter that will look good when published on, say, Github, but will not show in the tutorial.

## Wrapped text

Paragraphs will be wrapped to the width if they exceed the width. That makes the output a bit
prettier.

For example, in the previous paragraph, there's a line break in the markdown after the word
"output." If the width of the terminal is 58, glow will wrap at the word "exceed" and will then
respect the line break after "output," which is kind of gross.

So we have added a `wrap` processor (not exposed to the user) that will fill and wrap properly.

This makes the behavior much more like what happens with Markdown in a browser or with the nice
[Marked 2](https://marked2app.com/) application when its setting "retain line breaks in paragraphs"
is off.

## Code fences

The `tutor` script will respect ordinary code fencing for pretty output, such as this:

````
```shell
% date
```
````

Which looks like this:

```shell
% date
```

You can continue with further text after the code fence.

## The CLI

The `tutor` script can also execute commands in the current session. This is done with a code
fence where the fence is introduced with \`\`\` with a language specification of `shell { .run }`.

It looks something like this, but in your actual code fencing, you have to use \`\`\` not ~~~:

````
~~~shell { .run }
date
~~~
````

(These runnable code fencing sections cannot be surrounded by \`\`\`\`.)

So let's actually run the `date` command (and get the weather, too):

```shell { .run }
date
curl -s wttr.in | head -7
```

So that's a case where the code fence introduced a section that needs to be sent to the shell. More
Markdown text can come after the code fence.

Note that these commands run in the current session -- and all in the same session. This means that
there's no extra work for environment variables to be defined and be seen in subsequence runnable
code fences.

## Loops

If you have a loop, you'll need to write it so as to be one one line.

```shell { .run }
for i in {1..10}; do echo $i; done
```

## The shell

Also note that the shell to which commands are sent is shell you're actually using. So on the Mac,
that's typically zsh.

```shell { .run }
echo $SHELL
```

## Config

I don't think you will want to configure anything, but there are a couple of things you can do.

First off, `glow` really likes there to be a margin around what is displayed. I'm not sure I like
that. If you want to change that, you can go into the code and change the value of `GLOW_MARGIN`.
The default is 2. A setting of 0 looks a little cramped. 1 isn't bad. `tutor` uses 1.

Second, you can tweak the wrapper by changing `FORMATTER`. For instance, you might want to use the
`par` command which provides for justifying text.

## Summary

So that's it. You can created a Markdown tutorial, page throßugh it, and have some sections be
dispatched to the shell so that you can see the operation of the code.
