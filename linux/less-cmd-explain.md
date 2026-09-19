`less` Command in Linux

 The **`less`** command is a terminal-based **file viewer/pager**. It lets you read large files **one screen at a time** without loading the entire file into your terminal.

 It is especially useful for **log files, configuration files, source code, and command output**.

 ## 1\. Basic syntax

```
less [options] filename
```

 For example:

```
less notes.txt
```

 You can then move through `notes.txt` interactively.

---

 ## 2\. Why use `less`?

 Suppose a file contains 100,000 lines:

```
cat huge.log
```

 This may dump the entire file onto your terminal, making it difficult to read.

 Instead:

```
less huge.log
```

 Now you can:

 - Scroll up and down
- Search for text
- Jump to specific lines
- Go to the beginning/end
- Navigate between search results
- View the file without modifying it
- Follow a growing log file

---

 # 3\. Most important `less` keyboard commands

 When you're inside `less`, these commands are particularly important.

 ### Moving around

 | Key | Action |
| --- | --- |
| `↑` | Move up one line |
| `↓` | Move down one line |
| `Space` | Move down one screen |
| `b` | Move up one screen |
| `f` | Move forward one screen |
| `Enter` | Move down one line |
| `k` | Move up one line |
| `j` | Move down one line |
| `g` | Go to beginning of file |
| `G` | Go to end of file |
| `q` | Quit `less` |

For example:

```
less /var/log/syslog
```

 Then:

```
g
```

 takes you to the beginning.

 Press:

```
G
```

 to jump to the end.

 Press:

```
q
```

 to exit.

---

 # 4\. Searching inside a file

 One of the best features of `less` is searching.

 ## Search forward

 Press:

```
/error
```

 Then press **Enter**.

 For example:

```
/error
```

 This searches forward for `error`.

 Press:

```
n
```

 to find the **next** occurrence.

 Press:

```
N
```

 to find the **previous** occurrence.

 ### Example

```
less server.log
```

 Inside `less`:

```
/error
```

 Press Enter.

 You might find:

```
2026-09-10 09:01 ERROR: Database connection failed
```

 Then press `n` to find the next `error`.

---

 # 5\. Searching backward

 Use `?` instead of `/`.

```
?error
```

 This searches **backward** through the file.

 For example:

```
?ERROR
```

 Then:

 - `n` → next matching result in the search direction
- `N` → opposite direction

---

 # 6\. Case-insensitive searching

 You can use the `-i` option:

```
less -i server.log
```

 Now a search such as:

```
/error
```

 can match:

```
error
ERROR
Error
eRrOr
```

 You can also use:

```
less -I server.log
```

 depending on how you want case sensitivity handled.

---

 # 7\. Going to a specific line

 Suppose you want to go directly to line **500**.

 Inside `less`, type:

```
500g
```

 and press Enter.

 You will jump to line 500.

 You can also start `less` at a particular line:

```
less +500 file.txt
```

 This opens the file around line 500.

---

 # 8\. Going to the end of a file

 Press:

```
G
```

 This is extremely useful with log files.

 For example:

```
less /var/log/syslog
```

 Then:

```
G
```

 takes you to the latest part of the file.

---

 # 9\. Viewing command output with `less`

 `less` isn't limited to files.

 You can pipe command output into it:

```
ls -l /etc | less
```

 Another example:

```
ps aux | less
```

 Or:

```
dmesg | less
```

 This is useful when the command produces a lot of output.

 ### General pattern

```
command | less
```

 For example:

```
find / -type f 2>/dev/null | less
```

 Now you can inspect the results page by page.

---

 # 10\. `less` vs `cat`

 A common beginner question is:

 ### `cat`

```
cat file.txt
```

 `cat` prints the contents directly to the terminal.

 ### `less`

```
less file.txt
```

 `less` opens an interactive viewer.

 For a small file:

```
cat file.txt
```

 is often convenient.

 For a large file:

```
less file.txt
```

 is usually much better.

---

 # 11\. `less` vs `more`

 Linux also has a command called `more`:

```
more file.txt
```

 Historically, `more` was a basic pager.

 `less` provides more navigation and searching capabilities, which is why you'll commonly see:

```
command | less
```

 The name is a joke based on the idea that **"less is more."**

---

 # 12\. Useful command-line options

 ## `-N` — Show line numbers

```
less -N file.txt
```

 You will see something like:

```
     1  Hello
     2  Linux
     3  World
     4  Test
```

 This is very useful when debugging code or logs.

---

 ## `-S` — Don't wrap long lines

 Normally, a very long line may wrap onto multiple terminal lines.

 Use:

```
less -S file.txt
```

 to prevent line wrapping.

 You can then use the **left/right arrow keys** to see the rest of a long line.

 This is particularly useful for:

 - CSV files
- Log files
- Long URLs
- JSON
- Source code

---

 ## `-R` — Preserve ANSI colors

 If command output contains terminal color codes:

```
command | less -R
```

 This can preserve the colors.

 For example:

```
grep --color=always "ERROR" server.log | less -R
```

---

 ## `-F` — Quit if output fits on one screen

```
less -F file.txt
```

 If the content fits on one screen, `less` may exit automatically instead of opening an interactive viewer.

---

 ## `-X` — Don't clear the terminal

```
less -X file.txt
```

 This prevents `less` from clearing the screen when it exits.

---

 # 13\. Combining options

 You can combine options.

 For example:

```
less -NS file.txt
```

 means:

 - `-N` → show line numbers
- `-S` → don't wrap long lines

 You can also write:

```
less -N -S file.txt
```

 Both are valid.

---

 # 14\. Follow a growing log file

 One particularly useful feature is watching a file while it is being updated.

 Start with:

```
less +F server.log
```

 The `+F` tells `less` to follow the file, similar to:

```
tail -f server.log
```

 As new lines are added to the log, they appear automatically.

 For example:

```
less +F /var/log/application.log
```

 ### Important

 While following the file, press:

```
Ctrl+C
```

 to stop following.

 You remain inside `less`, so you can then search and navigate normally.

---

 # 15\. Useful navigation commands

 Here is a more complete cheat sheet.

 ### Basic movement

```
j       Down one line
k       Up one line
↓       Down one line
↑       Up one line
Ctrl+D  Half screen down
Ctrl+U  Half screen up
Space   One screen down
b       One screen up
```

 ### File navigation

```
g       Beginning of file
G       End of file
50g     Go to line 50
```

 ### Searching

```
/error  Search forward for "error"
?error  Search backward for "error"
n       Next match
N       Previous match
```

 ### Exiting

```
q       Quit
```

---

 # 16\. Jumping by percentage

 You can also jump to a percentage of the file.

 For example:

```
50%
```

 takes you approximately halfway through the file.

```
90%
```

 takes you near the end.

 This can be useful for extremely large files.

---

 # 17\. Opening multiple files

 You can give `less` multiple filenames:

```
less file1.txt file2.txt file3.txt
```

 Inside `less`, you can move between files.

 Useful commands include:

```
:n
```

 Go to the next file.

```
:p
```

 Go to the previous file.

---

 # 18\. Viewing compressed files

 On many Linux systems, you can use specialized commands such as:

```
zless file.txt.gz
```

 This allows you to view a gzip-compressed file without manually extracting it.

 For example:

```
zless access.log.gz
```

 Similarly, depending on the compression format, systems commonly provide tools such as:

```
bzless file.txt.bz2
xzless file.txt.xz
```

---

 # 19\. Very useful real-world examples

 ### Read a log

```
less /var/log/syslog
```

 ### Read a configuration file

```
less /etc/ssh/sshd_config
```

 ### Read source code

```
less program.c
```

 ### Examine command output

```
ps aux | less
```

 ### Search command output

```
dmesg | less
```

 Then:

```
/error
```

 ### Show line numbers

```
less -N program.c
```

 ### Don't wrap long lines

```
less -S access.log
```

 ### Start at the end

```
less +G server.log
```

 ### Follow a log

```
less +F server.log
```

---

 # 20\. A practical example

 Imagine `server.log` contains:

```
INFO Server started
INFO Listening on port 8080
INFO User connected
ERROR Database connection failed
INFO Retrying connection
ERROR Database connection failed
INFO Server stopped
```

 Run:

```
less server.log
```

 Then:

```
/error
```

 Press **Enter**.

 You jump to:

```
ERROR Database connection failed
```

 Then:

```
n
```

 takes you to the next error.

 Then:

```
N
```

 takes you back to the previous error.

 Press:

```
g
```

 to return to the beginning.

 Press:

```
G
```

 to go to the end.

 Finally:

```
q
```

 to quit.

---

 # 21\. The most important commands to memorize

 If you're learning Linux, don't try to memorize everything at once. Start with these:

```
q       Quit
Space   Page down
b       Page up
j       Down
k       Up
g       Beginning
G       End
/word   Search forward
?word   Search backward
n       Next search result
N       Previous search result
```

 And from the shell:

```
less filename
```

```
command | less
```

```
less -N filename
```

```
less -S filename
```

```
less +F filename
```

 ### In one sentence

 **`less` is an interactive Linux pager that lets you efficiently read, navigate, search, and inspect large files or command output without dumping everything onto the terminal.**
