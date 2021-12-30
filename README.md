Most commands are often followed by options that modify command behaviour, and further by one or more arguments, the items upon which the command acts. So most commands look like this:

`command -option[s] argument[s]`

# What are Commands?

- An *executable program* as compiled binaries (written in C, etc) or as a scripting language (Python, Ruby, etc)
- A *shell builtin*, commands built into the shell itself. i.e. `cd` is a shell builtin.
- A *shell function*, miniature shell scripts incorporated into the environment.
- An *alias*, commands we define ourselves, built from other commands.

When square brackets appear in the description of a command's syntax, they indicate optional items. A vertical bar indicates mutually exclusive items.

# Wildcards

Because the shell uses filenames so much, it provides special characters to help you rapidly specify groups of filenames.

- `*` matches any characters, i.e. `*` matches all files, or `g*.txt` matches any file beginning with _g_ followed by any characters and ending with _.txt_.
- `?` matches any single character, i.e. `Data???` matches any file beginning with _Data_ followed by exactly three characters.
- `[characters]` matches any character that is a member of the set of `characters`.
- `[!characters]` matches any character that is not a member of the set of `characters`.
- Wildcards always expand in sorted order.<sup>pg 54</sup>

# Useful Linux Commands

- Date and Calendar
  - `date` for current date and time.
  - `cal` for a nice calendar.

- Navigation
  - `cd `  changes the working directory to your home directory.
    - `cd -` changes the working directory to the previous working directory.

- Expansions
  - Using wildcards expands into matching filenames or directories.
  - `~`, or _tilde expansion_ expands into the name of the home directory of the named user, or if no user is named, the current user, i.e. `cd ~foo`.
  - `${parameter}` for _parameter expansion_. `printenv` shows a list of available variables.
  - `$(command)` for _command substitution_. Allows us to use the output of a command as an expansion, i.e. `echo $(ls)`
    - Double quotes causes all special characters to lose meaning, _except for_ `$`, `\`` and `\\` (so word splitting, pathname expansion, tilde expansion and brace expansion are supressed). We can use an _escape character_ `\\` to supress a single special character.
    - Single quotes supresses _all expansions_.
  - `$((expression))` for arithmetic expansion, but only supports integers.
  - `{}` for _brace expansion_, i.e. `echo {A,B,C}`, `echo {A..K}`, `echo {Q..F}`, `echo {1..10}`, `echo {001..100}`, `echo a{A{1,2},B{3,4}}b`.

- Exploring the system
  - `file` describes a file.
    - `file -i` gives more information about the file.
  - `ls` lists files in a directory.
    - `ls -d` ordinarily if a directory is specified `ls` will list the contents of the directory, not the directory itself. The `-l` option shows details about the directory rather than its contents.
    - `ls -h` displays file sizes in human-readable format.

- Manipulating Files and Directories
  - `ln -s item symlink-name` creates a symbolic link to a file. Directory listings begin with a `l`

- Working With Commands
  - `which` determines the exact location of a given executable, useful if there is more than one version of an executable program installed on a system.
  - `apropos` or `man -k` searches the man pages for a keyword and lists relevant commands.
  - `alias _name_='_string_'` assigns a command(s) to an alias. `alias` lists all current aliases. `unalias _alias_` unassigns an alias.

- Finding Stuff
  - `grep` prints out the lines containing a pattern in a file, i.e. `grep pattern filename`.
    - `grep -i` causes `grep` to ignore case.
    - `grep -v` causes `grep` to print only lines that do not match the pattern

- Redirection
  - I/O redirection allows us to change where input comes from and output goes. Normally standard input (_stdin_) comes from the keyboard and standard output (_stdout_) goes to the screen.
  - `>` sends results to another file, written from the beginning. Connects a command with a file.
  - `>>` sends results to another file, appended.
  - `&>` or `>` followed by `2>&1` redirects _stdout_ and _stderr_ to one file.
  - `1>` redirects _stdin_, `2>` redirects _stderr_.
  - `tee` sends results to another file, and _stdout_, i.e. `tee file`
  - `<` changes the source of _stdin_ from the keyboard to an input file.
  - `|` connects the standard output of one command into the standard input of a second command, i.e. `command1 | command2 | command3`.

- Control Operators
  - `&&`, i.e. `command1 && command2` executes _command1_, and _command2_ is executed if _command1_ is successful.
  - `||`, i.e. `command1 || command2` executes _command2_ if _command1_ is unsuccessful.
  - `;`, i.e. `command1; command2; command3;...` sequences commands but not not rely on the previous command to be successful for subsequent commands to be executed.
