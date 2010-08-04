
rpbar
======

Intro
-----

[Here's a screenshot](http://github.com/downloads/dimatura/rpbar/shot.png).
rpbar is the small bar at the bottom.

The 'default' way to switch windows in Ratpoison is to press `C-t-w` to get the
window list, find the number of the window you want to switch to (let's say
it's 2), and press `C-t-2` to switch. rpbar is a simple taskbar that gives a
permanent display of the windows in the current group in order to make the
first `C-t-w` unnecesary. As a concession to The Rat, clicking on a window title
will select that window. This is useful for one-handed window selection.

The main inspiration is the simple task bars you get in other minimalistic
window managers such as [Awesome](http://awesome.naquadah.org) or
[wmii](http://wmii.suckless.org). rpbar's appearance is modeled on these task
bars.

The other inspiration is [ratbar.pl](http://xenotrout.com/prog/ratbar/).
rpbar does less that ratbar.pl, but does it better. As far as I can tell,
ratbar.pl uses polling to update its window list. This makes it unresponsive,
which is very annoying. rpbar uses Ratpoison's hook feature to refresh as
soon as you select windows, close windows, change groups, etc. It's not
as fast as it would be if it were part of Ratpoison, but it's pretty usable.

How it works
-------------

rpbar is a pretty simple C++ project. It consists of a single executable,
`rpbar`, which runs in the background and displays the window list using the
[FLTK](http://www.fltk.org) library. It refreshes the window list whenever it
receieves a message through a FIFO pipe. The messages are sent using
Ratpoison's hook feature. 

Compilation and usage
---------------------

Obviously, you should have a working g++ toolchain.  You'll also need the
[FLTK](http://www.fltk.org) library, version 1.1.x (I'm using 1.1.9).  If
you're using Debian or Ubuntu, what you need is in libfltk and libfltk-dev.

Then follow this steps:

1. Type `make` to compile.
1. Put `rpbar` and `rpbarsend` in your path. 
1. Add the following to your `.ratpoisonrc`:

        # tell ratpoison to ignore rpbar
        unmanage rpbar
        # leave space for bars
        set padding 0 14 0 14
        # start rpbar 
        exec rpbar
        # hooks
        addhook switchwin exec echo r > /tmp/rpbarfifo
        addhook switchframe exec echo r > /tmp/rpbarfifo
        addhook switchgroup exec echo r > /tmp/rpbarfifo
        addhook deletewindow exec echo r > /tmp/rpbarfifo
        # The latest versions of RP have this hook
        # addhook titlechanged exec echo r > /tmp/rpbarfifo

1. Restart Ratpoison or manually execute `rpbar` (it should run in the background).

Now, what if you don't like the colors? The font size? The bar height?  Right
now, the only way to change them is changing `settings.hh` and recompiling, a
la dwm. I know, it's unfriendly. I'll eventually add the capability to read a
config file at startup and/or take command line arguments.

Status
----------

I consider rpbar to be alpha. Maybe beta.  It's "good enough" for my daily
use, but it's pretty rough in the edges. Caveat emptor.
