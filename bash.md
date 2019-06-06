### Other resources
- <http://www.grymoire.com/Unix/Sh.html>
- <http://www.grymoire.com/Unix/Quote.html>
- <http://mywiki.wooledge.org/BashGuide>
- <http://mywiki.wooledge.org/BashFAQ>
- <http://wiki.bash-hackers.org/doku.php>

---

### [Terminal keybindings](http://ruslanspivak.com/2010/09/20/bash-key-bindings/)

Key binding   | Action
-----------   | ------
Ctrl-r        | History reverse search
Ctrl-a        | Jump to BOL 
Ctrl-e        | Jump to EOL 
Ctrl-l        | Clear terminal
Ctrl-k        | Delete from cursor to EOL 
Ctrl-         | Undo last operation
Ctrl-m        | Return
Ctrl-w        | Delete word left from cursor
Ctrl-u        | Delete from BOL to cursor
Ctrl-x Ctrl-e | Open the default editor $EDITOR and run edited command
Ctrl-p        | Previous command in history
Ctrl-n        | Next command in history
Ctrl-f        | Move forward a char
Ctrl-b        | Move backward a char
Alt-f         | Move forward a word
Alt-b         | Move backward a word
Ctrl-d        | Delete char under cursor Exit shell if empty
Alt-d         | Delete forward word
Ctrl-y        | Paste content of the kill ring
Ctrl-t        | Swap current char with previous char
Alt-t         | Swap current word with previous word
Alt-u         | Uppercase word at cursor
Alt-l         | Lowercase word at cursor
Ctrl-s        | Freeze terminal
Ctrl-q        | Restore frozen terminal
Shift-PgUp    | Scroll screen up
Shift-PgDn    | Scroll screen down

---

### Bash substitutions

```bash
$ !!               # run last command
$ sudo !!          # run the last command as root
$ ^foo^bar         # run the last command but replacing foo with bar, first occurence
$ !ffm:p           # look for cmd starts with "ffm" from history, without executing the cmd
$ !?mpeg?:p        # look for cmd contains "mpeg" from history, without executing the cmd
$ !:s/foo/bar/     # replace last command for foo with bar, first occurence
$ !:gs/foo/bar/    # replace last command for foo with bar, globally
$ !:gs/foo/bar/:p  # replace last command for foo with bar globally, and without execution
```

---

### [String manipulation](http://mywiki.wooledge.org/BashGuide/Parameters)

Syntax                       | Description
------                       | -----------
`${parameter:-value}`        | Use Default Value. If 'parameter' is unset or null, 'value' is substituted. Otherwise, the value of 'parameter' is substituted.
`${parameter:=value}`        | Assign Default Value. If 'parameter' is unset or null, 'value' (which may be an expansion) is assigned to 'parameter'. The value of 'parameter' is then substituted.
`${parameter:+value}`        | Use Alternate Value. If 'parameter' is null or unset, nothing is substituted, otherwise 'value' is substituted.

Syntax                       | Description
------                       | -----------
`${parameter:offset:length}` | Substring Expansion. Expands to up to 'length' characters of 'parameter' starting at the character specified by 'offset' (0-indexed). If ':length' is omitted, go all the way to the end. If 'offset' is negative (use parentheses!), count backward from the end of 'parameter' instead of forward from the beginning. If 'parameter' is @ or an indexed array name subscripted by @ or *, the result is 'length' positional parameters or members of the array, respectively, starting from 'offset'.
`${#parameter}`              | The length in characters of the value of 'parameter' is substituted.

Syntax                       | Description
------                       | -----------
`${parameter#pattern}`       | The 'pattern' is matched against the beginning of 'parameter'. The result is the expanded value of 'parameter' with the shortest match deleted.
`${parameter##pattern}`      | As above, but the longest match is deleted.
`${parameter%pattern}`       | The 'pattern' is matched against the end of 'parameter'. The result is the expanded value of 'parameter' with the shortest match deleted.
`${parameter%%pattern}`      | As above, but the longest match is deleted.

Syntax                       | Description
------                       | -----------
`${parameter/pat/string}`    | Results in the expanded value of 'parameter' with the first (unanchored) match of 'pat' replaced by 'string'.
`${parameter//pat/string}`   | As above, but every match of 'pat' is replaced.


http://www.tldp.org/LDP/abs/html/string-manipulation.html
```bash
${#string}                          # string length

${string:position}                  # extracts substring from $string at $position
${string:position:length}           # extracts $length characters of substring from $string at $position

${string#substring}                 # deletes shortest match of $substring from front of $string
${string##substring}                # deletes longest match of $substring from front of $string
${string%substring}                 # deletes shortest match of $substring from back of $string
${string%%substring}                # deletes longest match of $substring from back of $string

${string/substring/replacement}     # replace first match of $substring with $replacement
${string//substring/replacement}    # replace all matches of $substring with $replacement
${string/#substring/replacement}    # if $substring matches front end of $string, substitute $replacement for $substring
${string/%substring/replacement}    # if $substring matches back end of $string, substitute $replacement for $substring
```

http://www.cyberciti.biz/tips/bash-shell-parameter-substitution-2.html

Syntax                          | Explantion
------                          | ----------
`${parameter:-defaultValue}`    | Get default shell variables value
`${parameter:=defaultValue}`    | Set default shell variables value
`${parameter:?"Error Message"}` | Display an error message if parameter is not set
`${#var}`                       | Find the length of the string
`${var%pattern}`                | Remove from shortest rear (end) pattern
`${var%%pattern}`               | Remove from longest rear (end) pattern
`${var:num1:num2}`              | Substring
`${var#pattern}`                | Remove from shortest front pattern
`${var##pattern}`               | Remove from longest front pattern
`${var/pattern/string}`         | Find and replace (only replace first occurrence)
`${var//pattern/string}`        | Find and replace all occurrences

---

### [Redirection](http://wiki.bash-hackers.org/syntax/redirection)

- redirection operation can be anywhere in a simple command, so these examples are equivalent:
```bash
cat foo.txt bar.txt >new.txt
cat >new.txt foo.txt bar.txt
>new.txt cat foo.txt bar.txt
```

- redirect stdout and stderr to TARGET, the following two are equivalent
```bash
> TARGET 2>&1
&> TARGET
```

- append redirected stdout and stderr to TARGET, the following two are equivalent
```bash
>> TARGET 2>&1
&>> TARGET
```

- transporting stdout and stderr through a pipe
```bash
COMMAND1 2>&1 | COMMAND2
COMMAND1 |& COMMAND2
```

---

### [Here documents](http://tldp.org/LDP/abs/html/here-docs.html)
```bash
<<TAG
...
TAG

<<-TAG
...
TAG
```

---

### [`if` tests](http://www.tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_01.html)

### `FILE` is a path to file.
File tests            | Description
----------            | -----------
`[ -a FILE ]`         | True if FILE exists.
`[ -b FILE ]`         | True if FILE exists and is a block-special file.
`[ -c FILE ]`         | True if FILE exists and is a character-special file.
`[ -d FILE ]`         | True if FILE exists and is a directory.
`[ -e FILE ]`         | True if FILE exists.
`[ -f FILE ]`         | True if FILE exists and is a regular file.
`[ -g FILE ]`         | True if FILE exists and its SGID bit is set.
`[ -h FILE ]`         | True if FILE exists and is a symbolic link.
`[ -k FILE ]`         | True if FILE exists and its sticky bit is set.
`[ -p FILE ]`         | True if FILE exists and is a named pipe (FIFO).
`[ -r FILE ]`         | True if FILE exists and is readable.
`[ -s FILE ]`         | True if FILE exists and has a size greater than zero.
`[ -t FD ]`           | True if file descriptor FD is open and refers to a terminal.
`[ -u FILE ]`         | True if FILE exists and its SUID (set user ID) bit is set.
`[ -w FILE ]`         | True if FILE exists and is writable.
`[ -x FILE ]`         | True if FILE exists and is executable.
`[ -O FILE ]`         | True if FILE exists and is owned by the effective user ID.
`[ -G FILE ]`         | True if FILE exists and is owned by the effective group ID.
`[ -L FILE ]`         | True if FILE exists and is a symbolic link.
`[ -N FILE ]`         | True if FILE exists and has been modified since it was last read.
`[ -S FILE ]`         | True if FILE exists and is a socket.
`[ FILE1 -nt FILE2 ]` | True if FILE1 has been changed more recently than FILE2, or if FILE1 exists and FILE2 does not.
`[ FILE1 -ot FILE2 ]` | True if FILE1 is older than FILE2, or is FILE2 exists and FILE1 does not.
`[ FILE1 -ef FILE2 ]` | True if FILE1 and FILE2 refer to the same device and inode numbers.

### `STRING` is a string
String tests                  | Description
------------                  | -----------
`[ -z STRING ]`               | True of the length if "STRING" is zero.
`[ -n STRING ] or [ STRING ]` | True if the length of "STRING" is non-zero.
`[ STRING1 == STRING2 ]`      | True if the strings are equal. "=" may be used instead of "==" for strict POSIX compliance.
`[ STRING1 != STRING2 ]`      | True if the strings are not equal.
`[ STRING1 < STRING2 ]`       | True if "STRING1" sorts before "STRING2" lexicographically in the current locale.
`[ STRING1 > STRING2 ]`       | True if "STRING1" sorts after "STRING2" lexicographically in the current locale.


### `ARG1` and `ARG2` are integers
Arithmetic tests    | Description
----------------    | -----------
`[ ARG1 -eq ARG2 ]` | True if ARG1 equal to ARG2
`[ ARG1 -ne ARG2 ]` | True if ARG1 not equal to ARG2
`[ ARG1 -lt ARG2 ]` | True if ARG1 less than ARG2
`[ ARG1 -le ARG2 ]` | True if ARG1 less than or equal to ARG2
`[ ARG1 -gt ARG2 ]` | True if ARG1 greater than ARG2
`[ ARG1 -ge ARG2 ]` | True if ARG1 greater than or equal to ARG2



Other tests                   | Description
-----------                   | -----------
`[ -o OPTIONNAME ]`           | True if shell option "OPTIONNAME" is enabled.

### Combining expressions

Operation            | Effect
---------            | ------
`[ ! EXPR ]`         | True if EXPR is false.
`[ ( EXPR ) ]`       | Returns the value of EXPR. This may be used to override the normal precedence of operators.
`[ EXPR1 -a EXPR2 ]` | True if both EXPR1 and EXPR2 are true.
`[ EXPR1 -o EXPR2 ]` | True if either EXPR1 or EXPR2 is true.

---

### [Comparing Integers and Strings](http://www.tldp.org/LDP/abs/html/comparison-ops.html)


### Integer comparison

syntax | meaning                                                 | usage
------ | -------                                                 | -----
`-eq`  | is equal to                                             | `if [ "$a" -eq "$b" ]`
`-ne`  | is not equal to                                         | `if [ "$a" -ne "$b" ]`
`-gt`  | is greater than                                         | `if [ "$a" -gt "$b" ]`
`-ge`  | is greater than or equal to                             | `if [ "$a" -ge "$b" ]`
`-lt`  | is less than                                            | `if [ "$a" -lt "$b" ]`
`-le`  | is less than or equal to                                | `if [ "$a" -le "$b" ]`
`<`    | is less than (within double parentheses)                | `(("$a" < "$b"))`
`<=`   | is less than or equal to (within double parentheses)    | `(("$a" <= "$b"))`
`>`    | is greater than (within double parentheses)             | `(("$a" > "$b"))`
`>=`   | is greater than or equal to (within double parentheses) | `(("$a" >= "$b"))`

### String comparison

syntax | meaning                                      | usage
------ | -------                                      | -----
`=`    | is equal to                                  | `if [ "$a" = "$b" ]`
`==`   | is equal to (a synonym for =)                | `if [ "$a" == "$b" ]`		(see note below)
`!=`   | is not equal to                              | `if [ "$a" != "$b" ]`
`<`    | is less than, in ASCII alphabetical order    | `if [[ "$a" < "$b" ]]`
       |                                              | `if [ "$a" \< "$b" ]`		(Note that the "<" needs to be escaped within a [ ] construct)
`>`    | is greater than, in ASCII alphabetical order | `if [[ "$a" > "$b" ]]`
       |                                              | `if [ "$a" \> "$b" ]`		(Note that the ">" needs to be escaped within a [ ] construct)
`-z`   | string is null, that is, has zero length     |
`-n`   | string is not null                           |

Examples

```bash
[[ $a == z* ]]   # True if $a starts with an "z" (pattern matching).
[[ $a == "z*" ]] # True if $a is equal to z* (literal matching).

[ $a == z* ]     # File globbing and word splitting take place.
[ "$a" == "z*" ] # True if $a is equal to z* (literal matching).
```


```bash
String=''   # Zero-length ("null") string variable.

if [ -z "$String" ]
then
  echo "\$String is null."
else
  echo "\$String is NOT null."
fi     # $String is null.
```

---

### Bash single vs double bracket
```bash
if [ $count -gt 0 ] && [ $somevar != $var ]; then
    ... do something
fi

if [[ $count -gt 0  &&  $somevar != $var ]]; then
    ... do something
fi
```
