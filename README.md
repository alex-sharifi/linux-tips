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
  - `CTRL-A` moves cursor to the beginning of the line; `CTRL-E` moves cursor to the end of the line.
  - `CTRL-D` deletes the character at the cursor position.
  - `CTRL-K` cuts text from the cursor location to the end of the the line.
  - `cd `  changes the working directory to your home directory.
    - `cd -` changes the working directory to the previous working directory.
  - `history` stores the last 1000 commands. Commonly used as `history | grep filter`.

- Expansions
  - Using wildcards expands into matching filenames or directories.
  - `~`, or _tilde expansion_ expands into the name of the home directory of the named user, or if no user is named, the current user, i.e. `cd ~foo`.
  - `${parameter}` for _parameter expansion_. `printenv` shows a list of available variables.
  - `$(command)` for _command substitution_. Allows us to use the output of a command as an expansion, i.e. `echo $(ls)`
    - Double quotes causes all special characters to lose meaning, _except for_ `$`, `\`` and `\\` (so word splitting, pathname expansion, tilde expansion and brace expansion are supressed). We can use an _escape character_ `\\` to supress a single special character.
    - Single quotes supresses _all expansions_.
  - `$((expression))` for arithmetic expansion, but only supports integers.
  - `{}` for _brace expansion_, i.e. `echo {A,B,C}`, `echo {A..K}`, `echo {Q..F}`, `echo {1..10}`, `echo {001..100}`, `echo a{A{1,2},B{3,4}}b`.

- Exploring the System
  - `file` describes a file.
    - `file -i` gives more information about the file.
  - `ls` lists files in a directory.
    - `ls -d` ordinarily if a directory is specified `ls` will list the contents of the directory, not the directory itself. The `-l` option shows details about the directory rather than its contents.
    - `ls -h` displays file sizes in human-readable format.
    - `ls -l` displays in long format. The first part is access rights for the file. The second part is number of hard links. The third part is the username of the file's owner. The fourth part is the username of the file's owner. The fifth part is the name of the group that owns the file. The sixth part is size of the file in bytes. The seventh part is the date and time of the last modified date. The eighth part is the name of the file.

- Manipulating Files and Directories
  - `ln -s item symlink-name` creates a symbolic link to a file. Directory listings begin with a `l`. Symbolic links are identified with the _l_ attribute in the file attributes.

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

- Environment
  - `printenv` displays what has been stored in the environment variables.
  - `set` builtin displays what has been stored in the shell and environment variables, as well as any defined shell functions.
  - `printenv [var]` or `echo ${var}` displays the value of a specific variable.
  - The `~/.bashrc` file is a user's personal startup file. It can be used to extend or override settings in the global configuration script.
    - The `PATH` variable provides a list of directories to search for commands. It can be extended to include additional paths by `export PATH="${HOME}"/bin:"${PATH}"`. To add directores to your `PATH` or define additional environment variables, place the changes in the `~/.bashrc` file.
    - The `PS1` variable ('prompt string 1') defines how the prompt string looks. You can use various escape codes to set values and colours.
  - `export VAR=VAL` tells the shell to make the contents of _VAR_ available to child processes of this shell.
  - `source [file]` forces bash to read the shell script in [file].

- Permissions
  - Modern Linux practise is to create a unique, single-member group with the same name as the user.
  - `chmod` changes _file mode_. Only file's owner or the superuser can change the mode of a file or directory. `chmod 755 file` is common because it gives Owner read/write/execute permissions, and Group and World read/execute permissions.
  - `umask` sets the default file permissions.
  - `su [user]` starts a shell as another user.
    - `su -l [user]` loads the user's environment and changes the working directory to the user's home directory. You will often see this as `su -` because the _l_ letter is not required, and if the user is not specified, superuser is assumed.
    - `su -c 'command' [user]` executes a single command rather than starting an interactive shell.
    - `sudo` allows ordinary users to execute commands as a different user (usually the superuser) in a controlled way, and does not require access to the superuser's password but requires the user's own password. `sudo -i` starts an interactive superuser session. `sudo -l` lists superuser privileges granted by `sudo`.
  - `chown` changes Owner and Group of a file or directory (superuser privileges required).
    - `chown [owner][:[group]] file...` changes ownership of `file` from its current owner to the user `[owner]`
  - Common permissions sequence is: `chown user file; chmod 700 file`

- Processes
  - `ps` views processes. `ps a` is a common way of running the `ps` command.
  - `top` displays a continuously updating display of system processes.
  - `CTRL-C` _interrupts_ a program. 
  - `command &` puts a process in the background and is immune from terminal keyboard input, including any attempt to interrupt it with `CTRL-C`.
  - `jobs` lists jobs that have been launched from the terminal.
  - `fg` returns a process to the foreground, or `fg %[job number]` if there is more than one job.
  - `bg` moves a process back into the background, or `bg %[job number]` if there is more than one job.
  - `kill -signal [job number]` terminates processes that need killing.
