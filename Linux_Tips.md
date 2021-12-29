Most commands are often followed by options that modify command behaviour, and further by one or more arguments, the items upon which the command acts.

# Wildcards

- `*` matches any characters.
- `?` matches any single character.
- `[characters]` matches any character that is a member of the set of `characters`.

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
  - `cd ` changes the working directory to your home directory.
  - `cd -` changes the working directory to the previous working directory.

- Exploring the system
  - `file` describes a file
  - `ls -d` ordinarily if a directory is specified `ls` will list the contents of the directory, not the directory itself. The `-l` option shows details about the directory rather than its contents.
  - `ls -h` displays file sizes in human-readable format.

- Manipulating Files and Directories
  - `ln -s item symlink-name` creates a symbolic link to a file. Directory listings begin with a `l`

- Working With Commands
  - `which` determines the exact location of a given executable, useful if there is more than one version of an executable program installed on a system.
  - `apropos` or `man -k` searches the man pages for a keyword and lists relevant commands.
  - `alias _name_='_string_'` assigns a command(s) to an alias. `alias` lists all current aliases. `unalias _alias_` unassigns an alias.

- Redirection
  - 

- Control Operators
  - `&&`, i.e. `_command1_ && _command2_` executes _command1_, and _command2_ is executed if _command1_ is successful.
  - `||`, i.e. `_command1_ || _command2_` executes _command2_ if _command1_ is unsuccessful.
  - `;`, i.e. `_command1_; _command2_; _command3_;...` sequences commands but not not rely on the previous command to be successful for subsequent commands to be executed.
