Most commands are often followed by options that modify command behaviour, and further by one or more arguments, the items upon which the command acts.

# Wildcards

- `*` matches any characters.
- `?` matches any single character.
- `[characters]` matches any character that is a member of the set of `characters`.
- Wildcards always expand in sorted order.<sup>pg 54</sup>

# What are Commands?

- An *executable program* as compiled binaries (written in C, etc) or as a scripting language (Python, Ruby, etc)
- A *shell builtin*, commands built into the shell itself. i.e. `cd` is a shell builtin.
- A *shell function*, miniature shell scripts incorporated into the environment.
- An *alias*, commands we define ourselves, built from other commands.
- When square brackets appear in the description of a command's syntax, they indicate optional items. A vertical bar indicates mutually exclusive items.

# Useful Linux Commands

- Date and Calendar
  - `date` for current date and time.
  - `cal` for a nice calendar.

- Navigation
  - `cd `  changes the working directory to your home directory.
  - `cd -` changes the working directory to the previous working directory.

- Exploring the system
  - `file` describes a file.
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
