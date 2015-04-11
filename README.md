# Customizable add-ons for Bash
**Custom prompt, additionally available environment variables, and maybe more.**

This is a collection of all the self-written bash "extensions" I use for myself.
In the main part this is a git repo to store these scripts for myself -- but feel free to clone and use these scripts just as you want if you find them useful.

## Installation
When you clone the repo (via `git clone https://github.com/woalk/bash-customization.git`) or download and extract it, you get a directory with a bunch of \*.rc files.
**If you want to install just for yourself**, add the following snippet to your `~/.bashrc`, `~/.bash_profile` or `~/.profile` file (your choice, which of them):
```sh
BASHCUSTOMIZE_DIR="/path/to/bash-customize"
for f in ${BASHCUSTOMIZE_DIR}/*.rc; do . $f; done
```
Where you should replace the `/path/to/bash/customize` with the appropriate path to the directory containing the \*.rc files.

**If you want to install for everyone on the system** (which is *recommended*, because of the advantages of some features when used by every user you log in with), place the directory in a place where everyone can access it (that means that you shouldn't place it in an encrypted home directory, or an home directory only viewable by its owner), e.g. `/opt`.
Now add the snippet above, with the correct directory path, of course, to `/etc/bash.bashrc` (on Ubuntu, on other systems any other path your system puts the global bashrc file, if you don't know, this can easily be found out by googling this or searching yourself in the /etc directory).

**If you just want to add a single feature from this pack**, remove the line starting with `for f in ...` and add each \*.rc file separately by adding `source ${BASCUSTOMIZE_DIR}/filename.rc` with the correct filename.
*Note that some features depend on each other*, so read the feature documentation and only leave out features that are no dependency for another feature you want to add, otherwise add both.

## Feature documentation
### Bash colors.
`File:` `colors.rc`
`Depends on:` *`no feature, independent`*
`Needs applications:` `tput`

With this bash extension, you get an easier way to color expressions you write in bash or in own scripts.
Just add `$_C_RED`, `$_C_BLUE`, `$_C_GREEN`, ... to any `echo` or similar command, and your expression will be colored that way.
To go back to normal color, use `$_C_DEFAULT` or simply `$_C_`. This way you can chain multiple colors to a single expression.
Note that you should always add a color reset (`$_C_`) when your colored script or command ends, otherwise your terminal will keep the last selected color.
Use the command `colorhelp` at any time to get a short help on which color is named how. Use tab-completion on `$_C_→` to get suggestions on which color names are possible.

### PS (Bash prompt) configuration
`File:` `ps.rc`
`Depends on:` `colors.rc`
`Needs applications:` `git`

With this, your bash becomes way more individual and comfortable.
Your bash prompt (which is `$PS1` technically and e.g. `user@computer:~$ ` visually) now gets individual colors, so that you can easily distinguish input lines from command output lines.
Also (if you installed this feature for everyone on your machine), if you log in using root or `sudo -s`, your bash prompt will be showing in red color that you did so, so that you always can easily see whether you're running a command as root or not.
Having Git installed, your bash prompt shows you the current branch and if there are uncommitted changes when you enter a git repo with your terminal. (using the `__git_ps1` function made for exactly this purpose).


### *More features may be added in the future.*
Just keep this repo up-to-date using `git pull` on your local clone to get updates when available.


```
COPYRIGHT (C) 2015 Alexander Köster.
http://woalk.com  https://github.com/woalk
```
