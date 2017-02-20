# Screenguppy

(c) 2017 Finn Ellis. Free to use, modify, etc. under the MIT license. See
included LICENSE file.

Screenguppy is a little friend to keep you company in your GNU screen session.
Specifically, it adds an extra line (in the `caption` field, above the hardline
if you have one) with an ASCII fish that swims back and forth.


## Installation

Install screen, if necessary. (If you want to add a guppy to your
already-running screen, see the `.screenrc` section below.)

```bash
sudo apt-get screen
```

Grab this repository.

```bash
git clone git@github.com:relsqui/screenguppy.git

```

Link the executable into your bin directory.

```bash
cd screenguppy
ln -s ./screenguppy ~/bin
```

Make sure that directory is already in your `$PATH` by trying to run:

```bash
screenguppy
```

If it prints a single line with a fish on it, you're golden. If not,
add this line to your `~/.bashrc` file:

```
export PATH="$PATH:$HOME/bin"
```

And then re-execute that file:

```bash
source .bashrc
```

Now you should be able to run `screenguppy` and get one fish.


## Setup

Open your `~/.screenrc` file in a text editor. Do a quick search for any
lines that already have the command `backtick` in them. If you don't find any,
add these lines to the end of the file:

```
backtick 1 1 1 screenguppy
caption always '%{yb}%= %1` %='
```

If you do find some, make a note of the highest number that appears right
after the word `backtick`, then add one to it. Add the following lines
to the end of the file, substituting the number you got for N. (For
example, if you saw `backtick 1` and `backtick 2`, replace both
instances of N with 3.)


```
backtick N 1 1 screenguppy
caption always '%{yb}%= %N` %='
```

NOTE: If you already had a `caption` command in your `.screenrc` file,
that line and screenguppy can't work at the same time. If you're not sure
which one you want to keep, you can comment out the other one by putting a #
in front of it for now.

Finally, if you're already running a screen, re-initialize the configuration
by typing `C-a :` and then `source ~/.screenrc`. If you get errors about
the screenguppy command not being found, type `C-a :` and then
`setenv PATH "$PATH:$HOME/bin"`. 

If you're not running a screen yet, type `screen` to start one. Enjoy
your fishy friend!
