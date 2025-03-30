# 7. The Bash Shell

**Exam obj:**

1. **Use input-output redirection (>, >>, |, 2>, &>, etc.)** 
2. **Use `grep` and regular expressions to analyze text** 

EXAM  TIP: For persistence, per-user settings must be added to an appropriate per-user shell startup file. 

RULE:
$ = Whenever referring to a variable (i.e. using a variable in a command) or Setting a variable with a command in it $(<cmd> <dir>)

No $ = When setting a variable without a command 

| Command | Use | Switches | Syntax |  |
| --- | --- | --- | --- | --- |
| `echo` | Observe value(s) stored in variables |  | `echo $<variable_name>` |  |
| `<variable_name>=”<fullpath>”` | Set a variable that stores the full path to a directory  |  | `mydir=”/home/user1/backups/2023-08-16”` |  |
| `<variable_name>=”text”`  | Set a variable that stores the text “Hello World!” |  | `myvar=”Hello World!”` |  |
| `<command> $<variable_name>` | Use the variable you made in a command |  | `<command> $mydir
<command> $myvar` |  |
| `<variable_name>=$(<command> and/or <directory>)` | Set a variable of an output |  | `file_list=$(ls -l /etc)`
→ `echo $file_list` |  |
| `<variable_name>=”< $<variable_name> on $(<command >) in \$<variable_name> “`  | Customize a variable using command and variable substitution  |  | `export PS1=”< $LOGNAME on $(hostname) in \$PWD > “`  | Customizes the primary shell prompt to display information within the quotes, using the variable **($LOGNAME)** and command (**($hostname)**) substitution features. 

Output: < user1 on server1.example.com in /home/user1 >  

Note: the PS1 env variable defines the primary shell (command) prompt
Note: To make this change persistent: Edit the ***.bash_profile*** file using vim (add a line to the file that matches the exact command) |
| `env` | Print environment variables |  |  |  |
| `set` | Print all shell (local) and environment variables  |  |  |  |
| `export` | Convert shell variable to environment variable |  | `export <variable_name>` —> ***NO $***  |  |
| `unset` | Erase a variable |  | `unset <variable_name>`  —> ***NO $***  |  |
| `>`  | Redirect standard output |  | `<command>  >  <destination_path>` |  |
| `>>` | Append redirected standard output |  | `<command>  >>  <destination_path>` |  |
| `set -o noclobber` | Prevent the overwriting of existing files when redirecting output |  |  |  |
| `2>`  | Redirect error messages |  | `find / -name core -print 2> /dev/null`  |  |
| `&>` 
`&>>` | Redirect or append both standard output and error |  | `ls /usr /cdr &> outerr.out` |  |
| `alias` | Set an alias substitution |  | `alias search=’find / -name core -exec ls -l {} \;’` |  |
| `unalias` | Unset an alias |  |  |  |
| `*` | Wildcard character |  |  | `.*` = list all hidden files |
| `?`  | Matches exactly one character  |  |  | `ls /var/log/????` = list all files and directories with exactly four characters in their names |
| `[ ]`  | Match either a set of characters or a range of characters for a single character position |  |  | `[yw]*` = will display all files/dir that begin with either “y” or “w” 
`[m-o]*` = will display all files/dir that begin with “m” through “o” 
`[!m-o]*` = will exclude all files/dir that begin with “m” through “o” and display everything else
`!` = inverse matches |
| `|` | Used to send the output of one command as input to the next  |  | `ls -l /proc | grep -v root | grep -iv dec | nl | tail -4` | Sends output of ls to grep for the lines that do not contain the pattern “root” The new output is further piped for a case-insensitive selection of all lines that exclude the pattern “dec” The filtered output is numbered (`nl`), and the final result shows the last four lines on the display (`tail - 4`) |
| `\`  | “Escape” character - Masks the meaning of any special character that follows it |  | `rm \*` = Will remove a file named “*” treating “*” as a normal character |  |
| `‘ ‘`  | Will mask meaning of all special characters encapsulated |  |  |  |
| `grep` | Search for text in a file or command output | `^` = mark the beginning of a line

`-v` = exclude
`-n` = number lines
`^$` = exclude all empty lines
`-i` = Case-insensitive search
`-w` = exact match for a word
`.` = single position filler
`-E` = either (use with single quotes (`’`) and pipe to include multiple patterns (`|`)  | `grep [options] pattern [file...]`

`grep -nv nologin /etc/passwd` 

`grep -v ^$ <full_path>`

`ls -l /etc | grep -E ‘cron|ly’` | 

Search for exclude (-v) the lines in the output that contain pattern “nologin” in the passwd file while also numbering (-n) each line. 

Exclude empty lines

Print all lines that include either the pattern “cron” or “ly”

 |
|  |  |  |  |  |

Shell (command) —> Kernel —> Hardware and/or Software (to carry out command) —> Returns to Shell (print output)

The bash shell is identified by the $ sign for normal users and the # sign for the root user

## Variables

- A variable is a temporary storage for data in memory. You can store a value (i.e. a file/directory location, command and/or directory to produce an output, etc.) in a variable.

2 Types of Variables: 

1. **Local (or shell) Variables**
- Private to the shell in which it is created
- Cannot be used by programs that are not started in that shell
- Current shell: The interactive shell you're actively using in your terminal session, maintaining environment variables, shell variables, functions, aliases, and working directory
- Subshell: A separate instance of the shell spawned from the current shell, inheriting environment variables but not local shell variables or functions. Changes made in a subshell do not affect the parent shell

1. **Environment Variables**
- Persistent between current shell and sub-shell(s)
- Any environment variable set in a sub-shell is lost when the sub-shell terminates
- Predefined environment variables are set for each user upon logging in (~25)
- use `env` command to view their values

**Primary shell (command) prompt = [user1@server1 ~]$ = PS1** 

Declare (create) variable: `<variable_name>=<desired_output>`

Reference variable: `$<variable_name>` 

Set back to default primary shell prompt: `export PS1=”[\u@\h \W]\\$ “`

## Input, Output, and Error Redirections (>, >>, |, 2>, etc.)

The default locations for the three streams are referred to as:

1.  *standard input*    OR   |  <  |  *stdin*    | 0  |
2. *standard output*  OR   |  >  | *stdout*  | 1  |
3. *standard error*     OR  |   > | *stderr*    | 2  |

Redirecting Standard Error 

- Error redirection forwards any error msgs generated to an alt destination rather than to that terminal window.
- /dev/null is a special file that is used to discard data
- Redirecting error messages here is ideal so only the useful output is exhibited on the screen and errors are thrown away

Ex: 

`find / -name core -print 2> /dev/null` 

- To avoid overwriting an existing file run: `set -o noclobber`
- Disable it: `set +o noclobber`

## Alias Substitution

Allows you to define a shortcut for a command or set of commands

- 

## Piping Output of One Command as Input to Another

- Pipe = **| = Used to send output of one command as input to the next**
- A construct of multiple pipes in a given line is called a pipeline

## Regular Expressions

- A text pattern or expression matched against a string of characters in a file or any input in a search operation (i.e. `grep`)
- `grep` = A command to extract needed info from a file or command output. The extracted info can then be redirected to a file.