# README

## See what people have to say:

![relevant](https://user-images.githubusercontent.com/30324885/40633997-d267bd8c-62c0-11e8-8a08-a3b77d6cddc3.png)

## Overview

Contate is like a "cp" command that processes the files during the copy.

It looks for
```
...whatever text up here...
<!-- contate:
#!/bin/bash
echo "Some script output"
:contate -->
...whatever text down here...
```
and executes the script by copying it to a temporary file and running it.

You get:

```
...whatever text up here...
Some script output
...whatever text down here...
```

Basically, you can embed arbitrary scripts in HTML documents.

Also, you can run it without a destination, so it dumps all output to standard out.

Since ANY output that occurs between the '<!-- contate:' makes it to the script, you can chain the contate command.

You can also exclude files and cp files wholesale. By default, anything '.\*', 'Makefile', '\*.contate', '\*/\*contate/\*' is excluded.

## Quick Start

Clone this into a directory where you want to make some documents (git submodule?), copy the Makefile from this to the root (presumably ../ if you cloned contate like I said), and edit a few variables at the head of the Makefile to set your "build" "stage" and "release" directories, if you need all that.

## Usage

### Flags:

**`--inherit`**

Any `contate` run from a script inherit the output instructions (below): the --quiet or --debug flags, and any patterns sent through --copy or --exclude. This must be first. Inputs are always unset.

**`-i` or `--input`**

Which file will we be contating? Overrides any --exclude, but not --copy.

**`-o` or `--output`**

What file should the result be output to? Can't have this and -p at the same time.

**`-p` or `--print`**

Don't store the output in a file, print it to stdout. Can't have this an -o at the same time.

**`-r` or `--recursive`**

Essential if -i is a directory, in true penguin fashion.

**`-d` or `--debug`**

Print out a whole bunch of absolutely poorly formed crap to stderr.

**`-e` or `--exclude`**

Whatever you put here (quote it) gets sent wholesale to the `find` command that handles "recursion". So "-not -path \"*whatever*\"", for example.

**`-q` or `--quiet`**

Die quietly. No stderr. I think.

**`-s` or `--script`**

Not being used, was meant to run scripts instead of embedding them.

**`-v` or `--var`**

Set or retreive variables, depending on whether or not there is a Key=Value format. Doesn't do any escaping, yet. Internally, the script uses a setvar, getvar function that stores all variables in a big list in a temporary file, and passes the name of that to each script it calls in the variable TMP_VAR. You can set TMP_VAR yourself, if you want, then you'll have a copy of it and can pass it between processes to share variable lists. By default, it's always inherited.

**`-c` or `--copy`**

Use quotes to include search patterns, just uses a test FILE1 -ef FILE2 type checking.

## Issues

Don't use -v or setvar,getvar (which you CAN use in your bash scripts) for complicated variables. Make your own temporary file, in that case. It's just not escaped or anything properly. Be ultra conservative with quoting exclude and copy, for the same reason.
