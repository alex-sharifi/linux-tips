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

# What is the shell?

According to The Unix Programming Guide (pg 26):

> The shell is just an ordinary program like `date` or `who`, although it can do some remarkable things. The fact that the shell sits between you and the facilities of the kernel has real benefits. There are three main ones: _Filename shorthands_: pick up a whole set of filenames as arguments to a program by specifying a pattern for the name the shell will find the names to match your pattern. _Input-output redirection_: arrange for the output of any program to go into a file instead of onto the terminal, and for the input to come from a file instead of the terminal. Input and output can even be connected to other programs. _Personalising_: you can define your own commands and shorthands. 

# Useful Linux Commands

- Utilities
  - `${?}` is a [special parameter](https://www.gnu.org/software/bash/manual/bash.html#Special-Parameters) which examines exit status (an integer ranging between 0 to 255) where 0 indicates success.
    - Because it is a variable, remember to use it as `echo ${?}` otherwise the shell will return an error, trying to execute the value as a command.
    - `true` command always executes successfully (exit status 0); the `false` command always executes unsuccessfully (exit status 1).
  - `date` for current date and time.
  - `cal` for a nice calendar.
  - `free` displays the amount of free memory.
  - `df` displays hthe amount of free space on our disk drives.
  - `sed` does find and replace on text. Format is `sed 's/regexp/replacement/g [file]`
    - The _g_ flag changes all instances if there are multiple matches.
    - Back references can be used with \\_n_ (where _n_ is a number) to substitue the corresponding subexpression in the preceding regular expression. Each corresponding subexpression needs to be enclosed in brackets.
    - **Escape all special characters!!!**
    - `sed -f [sed-file] [file]` references a `sed` expression from a saved file.
    - `sed -i` edits the file in place, meaning that rather than sending the edited output to _stdout_, it will rewrite the file with the changes applied.
    - `sed 's/regexp/replacement/g/; s/regexp/replacement/g/' [file]` places more than one editing command on the line by separating them with a semicolon.
  - `printf` does not accept _stdin_ but is given a string containing a format description, which is then applied to a list of arguments.

- Navigation
  - `CTRL-A` moves cursor to the beginning of the line; `CTRL-E` moves cursor to the end of the line.
  - `CTRL-D` deletes the character at the cursor position.
  - `CTRL-K` cuts text from the cursor location to the end of the the line.
  - `cd `  changes the working directory to your home directory.
    - `cd -` changes the working directory to the previous working directory.
  - `history` stores the last 1000 commands. Commonly used as `history | grep filter`.

- Expansions
  - Full list of expansions [available here](https://www.gnu.org/software/bash/manual/html_node/Special-Parameters.html).
  - Using wildcards expands into matching filenames or directories.
  - `~`, or _tilde expansion_ expands into the name of the home directory of the named user, or if no user is named, the current user, i.e. `cd ~foo`.
  - `${parameter}` for _parameter expansion_. `printenv` shows a list of available variables.
    - In most cases, the syntax `"${parameter}"` [is preferred](https://google.github.io/styleguide/shellguide.html#s5.6-variable-expansion).
    - `${parameter:-word}` is basically coalesce: if _parameter_ is not set, expansion results in the value of _word_. If _parameter_ is not empty, the expansion results in the value of _parameter_.
    - `${#parameter}` expands into the length of the string contained by _parameter_.
    - There are more of these types of expansions to manage empty variables. Full list of parameter expansions [available here](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html)
  - `$(command)` for [_command substitution_](https://www.gnu.org/software/bash/manual/html_node/Command-Substitution.html). Allows us to use the output of a command as an expansion, i.e. `echo $(ls)`
    - Double quotes causes all special characters to lose meaning, _except for_ `$`, `backtick` and `\\` (so word splitting, pathname expansion, tilde expansion and brace expansion are supressed). We can use an _escape character_ `\\` to supress a single special character.
    - `\`command\`` syntax is also supported for command substitution.
    - Single quotes supresses _all expansions_.
  - `$((expression))` for arithmetic expansion, but only supports integers.
  - `{}` for _brace expansion_, i.e. `echo {A,B,C}`, `echo {A..K}`, `echo {Q..F}`, `echo {1..10}`, `echo {001..100}`, `echo a{A{1,2},B{3,4}}b`.

- Exploring the System
  - "Everything in UNIX is a file" (The UNIX Programming Environment, pg. 41)
  - `file` describes a file.
    - `file -i` gives more information about the file.
    - `od -tcx1 [file]` is useful for inspecting hex representation of each character. Can be useful when there are weird-encoded characters in files.
    - The file system does not determine file types and the kernel cannot tell you the type of a file - so the `file` command makes an educated guess. For example, `file` may read the first few hundred bytes of a file to look for clues, i.e. 'magic numbers' at the beginning of a runnable file, or '#include' words in the file to identify C source. In UNIX, there is just one kind of file, and all that is required to access a file is its name. Programmers need not worry about file types - everything is treated as text files. 
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
  - `grep` prints out the lines containing a pattern in a file, i.e. `grep [options] regex [file...]`.
    - `grep -E` enables extended regular expression support.
    - `grep -i` causes `grep` to ignore case.
    - `grep -v` causes `grep` to print only lines that do not match the pattern.
    - Remember `[file]` can be a wildcard expansion. Useful for searching across many files within a directory.
  - `find` is given one or more names of directories to search.
    - `find ~` produces a listing of our home directory.
    - `find [directory] -type d` lists only directories, `find [directory] -type f` lists only files.
    - `find [directory] -type f -name "*.JPG"` lists files ending in _.JPG_. Notice how the value of the _-name_ option is enclosed in quotes to suppress pathname expansion.
    - `find [directory] -type f -not -perm 0600` uses a _logical operator_ to filter results further. Note `-and` is implied by default when no operator is present.

- Redirection
  - I/O redirection allows us to change where input comes from and output goes. Normally standard input (_stdin_) comes from the keyboard and standard output (_stdout_) goes to the screen.
  - `>` sends results to another file, written from the beginning. Connects a command with a file.
  - `>>` sends results to another file, appended.
  - `&>` or `>` followed by `2>&1` redirects _stdout_ and _stderr_ to one file.
  - `1>` redirects _stdin_ to a file, `2>` redirects _stderr_ to a file.
    - `>` is actually shorthand for `1>`
    - The second file descriptor, i.e. `&1` begins with an ampersand to distinguish it from writing out to a file called `1`.
  - `tee` sends results to another file, and _stdout_, i.e. `tee file`
  - `|` connects the standard output of one command into the standard input of a second command, i.e. `command1 | command2 | command3`.
    - `xargs` is used to accept standard input for commands that do not typically read from _stdin_. Also useful for parallelising tasks with `-P` and `-L` options. i.e. `find . -type f | xargs -P4 -L4 -I{} cp {} ./new/{}.json` copies all files, 4 at a time, to a /new/ directory. A nice description can be [found here](https://adamdrake.com/command-line-tools-can-be-235x-faster-than-your-hadoop-cluster.html#parallelize-the-bottlenecks).
  - `<` changes the source of _stdin_ from the keyboard to an input file.
  - `command << token
     text
     token` is a _here document_, where _command_ is the name of a command that accepts _stdin_, and `token` is frequently _EOF_.
  - `<<<` is a _here string_. Like a _here document_ but only consisting of a single line of input

- Control Operators
  - `&&`, i.e. `command1 && command2` executes _command1_, and _command2_ is executed if _command1_ is successful.
  - `||`, i.e. `command1 || command2` executes _command2_ if _command1_ is unsuccessful.
  - `;`, i.e. `command1; command2; command3;...` sequences commands but not not rely on the previous command to be successful for subsequent commands to be executed.

- Environment
  - `printenv` displays what has been stored in the environment variables.
  - You can obtain the value of any shell variable by prefixing its name with a `$` (The UNIX Programming Environment, pg 37).
  - `set` builtin displays what has been stored in the shell and environment variables, as well as any defined shell functions.
  - `printenv [var]` or `echo ${var}` displays the value of a specific variable.
  - The `~/.bashrc` file is a user's personal startup file. It can be used to extend or override settings in the global configuration script.
    - The `PATH` variable provides a list of directories to search for commands. It can be extended to include additional paths by `export PATH="${HOME}"/bin:"${PATH}"`. To add directores to your `PATH` or define additional environment variables, place the changes in the `~/.bashrc` file. "Probably the most useful shell variable is one that controls where the shell looks for commands. The syntax is a bit strange: a sequence of directory names separated by columns." (The UNIX Programming Environment, pg. 37)
    - The `PS1` variable ('prompt string 1') defines how the prompt string looks. You can use various escape codes to set values and colours.
  - `variable=value` is how variables are assigned.
    - `declare -r VAR=VAL` defines a variable with constraints.
    - `export VAR=VAL` tells the shell to make the contents of _VAR_ available to child processes of this shell.
  - `source [file]` forces bash to read the shell script in [file].
    - The dot `.` command is a synonym for the `source` command, a shell builtin that reads a specified file of shell commands and treats it like input from the keyboard.

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
  - Common permissions sequence is: `chown user file; chmod 700 file`.
  - The file `/etc/passwd` contains all the login information about each user, so you can discover your uid and group-id. The password field is encrypted - when you enter your password into `login` it encrypts your password and compares it to this file.

- Processes
  - `ps` views processes. `ps a` is a common way of running the `ps` command.
  - `top` displays a continuously updating display of system processes.
  - `CTRL-C` _interrupts_ a program. 
  - `command &` puts a process in the background and is immune from terminal keyboard input, including any attempt to interrupt it with `CTRL-C`.
  - `jobs` lists jobs that have been launched from the terminal.
  - `fg` returns a process to the foreground, or `fg %[job number]` if there is more than one job.
  - `bg` moves a process back into the background, or `bg %[job number]` if there is more than one job.
  - `kill -signal [job number]` terminates processes that need killing.

- Networking
  - `ip a` examines the system's network intefaces and routing table.
  - `ssh` encrypts all of the communications between the local and remote hosts.
    - `ssh` allows us to execute a single command on a remote system and have the results displayed on the local system, i.e. `ssh remote-sys 'command'`.
  - `sftp` allows for file transfer from remote servers. Does not require an FTP server to be running on the remote host, only the SSH server.
  - `wget` is used for downloading content from web and FTP sites. Can recursively download, as well as download files in the background.

- Archiving
  - `gzip` compresses one or more files, and replaces the original version with the compressed version of the original.
    - `gzip -c` writes output the standard output and keeps the original file.
    - `gzip -d` decompresses the file.
  - `tar` is the classic tool for archiving files. Format is `tar mode[options] pathname...`
    - `tar c` creates an archive from a list of files/directories.
    - `tar x` extracts an archive.
    - `tar t` lists the contents of an archive.
    - `tar cf [name] pathname` specifies the name of the tar archive, but the mode must always be specified first, before any other option.
    - `tar [mode]f -` redirects to standard input/output.
    - `tar [mode]f -T=-` causes `tar` to read its list of pathnames from a file rather than the command line.
    - `tar xf [name].tar pathname` limits the extraction to a single file from an archive.
    - Extraction is relative to the pathname rather than absolute, so it often makes sense to change directory to `/` so that extraction is relative to the root directory.

- Building Environments
  - `make` describes relationships and dependencies among the components that comprise a finished program. The `make` program can be used for any task that needs to maintain a target/dependency relationship, not just for compiling source code.
  - Takes a _Makefile_ as input, but `make` command only needs to be run.
  - Most of the _Makefile_ consists of lines that define a _target_, and the files on which is it _dependent_. The remaining lines describe the commands needed to create the target from its components.

- Shell Scripts
  - Carry out the commands as though they have been entered directly on the command line.
  - Shell is unique in that is is both a command line interface to the system and a scripting language interpreter.
  - Steps to create and run a script:
    - Write a script.
        - Use the [_interpreter directive_](http://mywiki.wooledge.org/BashGuide/CommandsAndArguments#Scripts) (or _hashbang_) to specify which interpreter should be used when executing the script. For example, `#!/bin/bash` or `#!/bin/python`.
    - `chmod 755 [file]` to set permissions to allow execution.
    - `export PATH="${HOME}"/bin:"${PATH}"` or `mv [file]` to a location in the PATH variable.
      - Otherwise you have to run the command from the directory in which the script is saved, but that is not very convenient. A good location for scripts is `~/bin` for personal use, or `/usr/local/bin` for scripts for everyone on a system to use.
  - `function name {
      commands
      return
     }` is the syntax for a function.
    - Shell functions make excellent replacements for aliases and are actually the preferred method of creating small commands for personal use.
    - Shell functions must appear in the script before they are called.
    - `local` defines a local variable, which precedes the variable name, i.e. `local foo; foo=2`
  - Shell functions can return an exit status by including an integer argument to the `return` command. The `exit` command accepts single optional argument which becomes the scripts exit status, used as a formality to terminate with the appropriate _exit_ code. But `return` does this too. Error messages should be redirected to _stderr_ with `>&2`.
  - `read` is used to read a single line of _stdin_, either from the keyboard or redirected from a line of data from a file.
    - If no variables are listed after the `read` command, a shell variable `${REPLY}` will be assigned all the input.
    - There are various options for `read`, such as `read -p prompt` displays a _prompt_ for input; `read -t seconds` sets a timeout in _seconds_.
  - `${1}..${n}` are positional parameters.
    - `${0}` will always contain the first item appearing on the command line, which is the pathname of the command being executed.
  - There are a number of other [special parameters](https://www.gnu.org/software/bash/manual/bash.html#Special-Parameters) used frequently with shell functions:
    - `${#}` contains the number of arguments on the command line.
    - `${@}` expands into the list of positional parameters, starting with 1.
    - `shift` command causes all the parameters to _move down one_, i.e. `while test "${#}" -gt 0; do commands shift done` or `while test -n "${1}"; do commands shift done`.

- Flow Control: If
  - `if commands; then commands [elif commands; then commands...] [else commands] fi` is the format.
  - `test` performs checks and comparisons to return an exit status of 0 when the expression is true, and a status of 1 when an exit status is false.
    - `test -e file` checks for presence of _file_.
    - `test -d file` checks if _file_ exists and is a directory.
    - `test string` checks if _string_ is not null.
    - `test -n string` checks the length of _string_ is greater than 0.
    - `test string1 == string2` checks if _string1_ and _string2_ are equal.
    - `test string1 > string2` checks if _string1_ sorts after _string2_ (but remember to quote or escape '>' so it is not interpreted as a redirection operator)
    - `test integer1 -eq integer2` checks if _integer1_ equals _integer2_.
    - `test check1 -a check2` checks if _check1_ and _check2_ are both true.
    - Since all expressions used by `test` are treated as command arguments by the shell (unlike `[[ ]]` and `(( ))`), characters that have special meaning to bash, such as `<`, `>`, `(` and `)` must be quoted or escaped.

- Flow Control: Looping
  - Full list of looping constructs are [available here](https://www.gnu.org/software/bash/manual/html_node/Looping-Constructs.html).
  - `while commands; do commands; done` is the _while_ syntax.
    - Commonly used as `while test expr; do commands; done`. But not always used with `test` because... see below.
    - As long as the exit status of `command` is 0, it performs the commands inside the loop.
    - Using `test` with `while` is common because it provides comparison operators. But if the comparison operator is not required, it can be ommitted.
  - `break` immediately terminates a loop; `continue` causes the remainder of the loop to be skipped, and program control resumes with the next iteration of the loop.
  - `until` continues until it receives a non-zero exit status.
  - `for variable in words; do commands done` is the _for_ syntax.
    - Supports pathname expansion, i.e. `for i in distros*.txt; do ...`
    - Alternative syntax is `for (( expression1; expression2; expression3 )); do commands done`

- Flow Control: Branching
  - `case word in [pattern [| pattern]...) commands ;;]... esac`
  - Patterns used by `case` are the same as those used by pathname expansion.

# Useful Websites

- https://google.github.io/styleguide/shellguide.html#s5.6-variable-expansion
- http://mywiki.wooledge.org/BashGuide/
- https://www.gnu.org/software/bash/manual/bash.html
- https://stackoverflow.com/questions/818255/in-the-shell-what-does-21-mean
