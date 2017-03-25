# Customizable add-ons for Bash
**Custom prompt, additionally available environment variables, and maybe more.**

This is a collection of all the self-written bash "extensions" I use for myself.
In the main part this is a git repo to store these scripts for myself -- but feel free to clone and use these scripts just as you want if you find them useful.

## Installation
When you clone the repo (via `git clone https://github.com/woalk/bash-customize.git`) or download and extract it, you get a directory with a bunch of \*.rc files.

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

**You need to have to comment out or remove the following lines** from each user's `.bashrc` file if you installed bash-customize for all users ( which means in `/etc/bash.bashrc` or similar file):
```sh
if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
```
(on Ubuntu, may vary on other systems, just look after the line(s) setting the `PS1` variable)

Following can be **customized** by setting environment variables in an bashrc file (add `export VARIABLE="value"`):

- `WOALK_USERCOLOR` -- The color in which the username is colored. Should be set in each user's .bashrc file.
- `WOALK_MACHINECOLOR` -- The color in which the host name (@...) is colored. Should only be set in the global bashrc file, so that it is the same for everyone.
- `WOALK_CHROOTCOLOR` -- The color in which a named Debian chroot environment name is colored. Should only be set in the global bashrc file, so that it is the same for everyone.
- `WOALK_DIRECTORYCOLOR` -- The color in which the current directory is colored.
- `WOALK_GITCOLOR`-- The color in which the git info (current branch & branch state) is colored.
- `WOALK_ROOTUSERCOLOR` -- The color in which the username "root" is colored. Should only be set in the global bashrc file, so that it is the same for everyone.
- `WOALK_EXITCODECOLOR` -- The color in which the exit code of the last command is colored, if shown.

Use one of the colors from `${_C_...}` or an escape sequence color as variable value.
Every variable can contain both, foreground and background color.
Don't include a printable character in these variables, otherwise your bash input may get distorted.

Also:

- `WOALK_MACHINENAME` -- This variable can contain any string, which is then displayed as the hostname. If not set, the true hostname is used.
- `WOALK_SHOWEXITCODE` -- This variable can be set to show the exit code after every command execution. It can be:
 - `"no"` or unset to not show any exit code
 - `"errors"` to only show non-zero exit codes
 - `"yes"` to show all, also zero exit codes
- `WOALK_SUPPRESS_EXITCODE` -- Set this at the end of a script or command to any value to not show its exit code. Will automatically be unset after that.

You can add an additional \*.rc file to your `$BASHCUSTOMIZE_DIR` named `configs.rc` (and include it in your bashrc if you did it manually), and store all the variable exports there. This makes identifying and sorting of the configuration variables easier.

### Quickly jumping to a project directory like `~/git/...` or `~/workspace/...`

`File:` `git-cd.rc`

`Depends on:` *`no feature, independent`*

`Needs applications:` *`none, independent`*

After including this, you can jump to a project directory of the kind `~/git/type/projectname` by using the command `gcd` or `git-cd`.

(Sadly, Git does not support function resolving, so you can **not** use `git cd`.)

**Example:**
```sh
[~]$ gcd android my-super-app
[~/git/android/my-super-app]$
```

This function supports Tab auto-completion if you have `bash-completion` installed.

For the first parameter, there is a abbreviation list, so you can replace the following with single letters:

- `a` for `android`
- `j` for `java`
- `p` for `php`
- `o` for `other`

You can also use more than two arguments - the command will treat them as more sub-folders.
For example, you can use `gcd o company projectname subproject` to jump to `~/git/other/company/projectname/subproject`.

You can customize the name of the base directory for your projects (`git` in all these examples) to another directory inside your home directory:

- `WOALK_GITCD_DIR=workspace` to use `~/workspace` instead.

### ZSH-like "replacing cd"

`File:` `replace-cd.rc`

`Depends on:` *`no feature, independent`*

`Needs applications:` *`none, independent`*

With this you can now use the command `replace-cd` or `rcd` to quickly replace a part of your current working path with another substring.

**Example:**
```sh
[/usr/local/man]$ rcd local share
[/usr/share/man]$
```

ZSH has this feature if you use `cd` with two parameters (in this example: `cd local share`).
I guess some will appreciate bash being capable of exactly the same thing.

This command supports Tab auto-completion if you have `bash-completion` installed.

**_Path expansion, more complex completion and longer path replacing with this command is a work-in-progress though._**

### *More features may be added in the future.*
Just keep this repo up-to-date using `git pull` on your local clone to get updates when available.


```
COPYRIGHT (C) 2017 Alexander Köster.
http://woalk.com  https://github.com/woalk
Free to use, as is, without any warranty and at your own risk.
```
