# [Screenguppy](https://github.com/relsqui/screenguppy)

(c) 2017 [Finn Ellis](mailto:relsqui@chiliahedron.com). Free to use, copy,
modify, etc. under the MIT license. See included LICENSE file.

Screenguppy is a little friend to keep you company in your GNU screen session.
Specifically, it adds an extra line at the bottom (in the caption field, above
the hardline if you have one) with a text-based fish that swims back and forth.

If you're not running screen or don't want to put a fish there, you can also
just run it in a bash loop (or use similar logic to add it to your i3 status
bar or anywhere else that you can insert script output). See
[Running Screenguppy Without Screen](#running-screenguppy-without-screen).

If you'd like to use Screenguppy with tmux, there are partial instructions in
an [open pull request](#2). If you can improve them, please leave comments!

Contributions of both code and ideas to Screenguppy are welcome. Check out the
issue tracker to see what's already planned and suggest other things.


## Contents
* [Installing Screenguppy](#installing-screenguppy)
* [Running Screenguppy Without Screen](#running-screenguppy-without-screen)
* [Adding Screenguppy to Screen](#adding-screenguppy-to-screen)
* [Advanced Customization](#advanced-customization)
* [Removing Screenguppy](#removing-screenguppy)


## Installing Screenguppy

Install screen, if necessary.

```bash
sudo apt-get screen
```

Clone this repository.

```bash
git clone https://github.com/relsqui/screenguppy.git

```

(You can use the SSH address instead if you have SSH keys configured on Github already.)

Make yourself a bin directory if it doesn't already exist, and then link the
Screenguppy executable into it.

```bash
mkdir -p ~/bin
ln -s ~/screenguppy/screenguppy ~/bin
```

(Replace the first path with wherever you cloned screenguppy, if it's not
your home directory.)

Make sure that `~/bin` is already in your `$PATH` by trying to run:

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

Now you should be able to run `screenguppy` and see one fish.


## Running Screenguppy Without Screen

If you just want to watch the animated fish without setting it up in your
screen caption line (or you already have a caption line, don't use screen,
etc.) then you can run the script independently in a bash loop, like so:

```bash
while true; do
  clear
  screenguppy
  sleep 1
done
```

Type `ctrl-c` to stop.


## Adding Screenguppy to Screen

Open your `~/.screenrc` file in a text editor. Do a quick search for any
lines that already have the command `backtick` in them. If you don't find any
(including if the file is brand new), add these lines to the end of the file:

```
backtick 1 1 1 screenguppy
caption always '%{yk}%=%1`%='
```

If there are some backtick lines, find the highest number that appears right
after the word `backtick`, then add one to it and note the result. For example,
if you saw `backtick 1` and `backtick 2`, note `3`.

Add the following lines to the end of the file, substituting your noted number
for `N` in both places (once per line).


```
backtick N 1 1 screenguppy
caption always '%{yk}%=%N`%='
```

NOTE: If you already have a `caption` command in your `.screenrc` file,
that line and Screenguppy can't work at the same time. If you're not sure
which one you want to keep, you can comment out the other one temporarily
by adding a # at the beginning of that line.

Finally, if you're already running a screen, refresh the configuration
by typing `C-a :` and then `source ~/.screenrc`. If you see errors about
the screenguppy command not being found, type `C-a :` again and then
`setenv PATH "$PATH:$HOME/bin"`. 

If you're not running a screen yet, type `screen` to start one.

Enjoy your fishy friend!


## Advanced Customization

* The `%{yb}` part of the caption line in `.screenrc` defines the fish and
  background colors. See the
  [String Escapes section of the GNU Screen Manual](https://www.gnu.org/software/screen/manual/html_node/String-Escapes.html)
  for other options.
* The `fish_options` block near the top of the `screenguppy` executable
  provides quick access to several configuration choices:
  * Screenguppy assumes your terminal is at least 80 characters wide and uses no
    more of it than that. You can give your fish more or less room to swim by
    editing the value for `max_range`.
  * Screenguppy will try to display Unicode art, and revert to ASCII if it
    fails.  You can force ASCII output by setting `use_unicode` to `False`.
    * You can also change the details of the art by editing the values of
      `unicode_art` and `ascii_art` which are initialized in the `Fish` class.
      Each one should be an array of two strings showing the right-facing
      variant and the left-facing variant, respectively.
  * You can change the fish's speed by changing the value for `speed`. This
    doesn't change how often it moves (which is capped at once per second by
    screen itself), but how far it moves each time.
    * The speed doesn't have to be an integer, but the distance will always be
      rounded.
    * If the speed is less than 1, the fish will sometimes skip its move.
    * If the speed is negative, the fish will swim backwards.


## Removing Screenguppy

You can remove Screenguppy from your screen by deleting the two lines you added
to your `.screenrc` file. If you have a screen open that you would like to keep
running, refresh its configuration by typing `C-a :` and then
`source ~/.screenrc`.

You can also delete Screenguppy entirely just by removing the directory it's in
and the link you created:

```bash
rm -rf ~/screenguppy # or wherever you cloned it
rm ~/bin/screenguppy
```
