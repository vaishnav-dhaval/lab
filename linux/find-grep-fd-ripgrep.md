if you're learning Linux/Unix command-line searching, **`find`**, **`grep`**, **`fd`**, and **`ripgrep (rg)`** cover most file and text-search tasks.

 ## 1\. `find` — Find files and directories

 `find` searches the filesystem based on things like **name, type, size, time, permissions**, etc.

 ### Basic syntax

```
find [path] [conditions] [actions]
```

 ### Find a file by name

```
find . -name "file.txt"
```

 - `.` → search from the current directory
- `-name` → match the filename
- `"file.txt"` → filename to search for

 Case-insensitive:

```
find . -iname "file.txt"
```

 ### Find all `.txt` files

```
find . -name "*.txt"
```

 ### Find directories

```
find . -type d
```

 Find files:

```
find . -type f
```

 Find symbolic links:

```
find . -type l
```

 ### Search a specific directory

```
find /home/user/Documents -name "*.pdf"
```

 ### Find by size

 Larger than 100 MB:

```
find . -type f -size +100M
```

 Smaller than 10 KB:

```
find . -type f -size -10k
```

 ### Find by modification time

 Modified within the last 7 days:

```
find . -type f -mtime -7
```

 Modified more than 30 days ago:

```
find . -type f -mtime +30
```

 `-mtime` counts in **24-hour periods**.

 For more precise time:

```
find . -type f -mmin -60
```

 This finds files modified in the last 60 minutes.

 ### Find empty files/directories

```
find . -type f -empty
```

```
find . -type d -empty
```

 ### Execute a command on results

 For example, remove `.tmp` files:

```
find . -type f -name "*.tmp" -delete
```

 Or:

```
find . -type f -name "*.log" -exec wc -l {} \;
```

 Here:

 - `{}` → current result
- `\;` → end of `-exec`

---

 # 2\. `grep` — Search text inside files

 `grep` searches for **text patterns inside files**.

 Basic syntax:

```
grep [options] "pattern" file
```

 ### Search for a word

```
grep "hello" file.txt
```

 ### Search multiple files

```
grep "hello" *.txt
```

 ### Ignore case

```
grep -i "hello" file.txt
```

 Matches:

```
hello
Hello
HELLO
HeLLo
```

 ### Show line numbers

```
grep -n "hello" file.txt
```

 Output might look like:

```
15:Hello world
42:hello again
```

 ### Search recursively

```
grep -r "hello" .
```

 This searches files under the current directory.

 A commonly useful version:

```
grep -rn "TODO" .
```

 - `-r` → recursive
- `-n` → line numbers

 ### Search for whole words

```
grep -w "cat" file.txt
```

 This matches:

```
cat
```

 but not necessarily:

```
catalog
copycat
```

 ### Invert the match

 Show lines that **don't** contain `hello`:

```
grep -v "hello" file.txt
```

 ### Show only matching text

```
grep -o "hello" file.txt
```

 ### Count matches

```
grep -c "hello" file.txt
```

 ### Show filenames only

```
grep -l "hello" *.txt
```

 ### Show filenames that DON'T match

```
grep -L "hello" *.txt
```

 ### Use regular expressions

```
grep "^Hello" file.txt
```

 Lines beginning with `Hello`.

```
grep "world$" file.txt
```

 Lines ending with `world`.

```
grep "error|warning" file.txt
```

 For extended regular expressions, use:

```
grep -E "error|warning" file.txt
```

---

 # 3\. `fd` — Modern alternative to `find`

 `fd` is designed to be a **simpler and faster alternative to `find`** for many everyday searches.

 Basic syntax:

```
fd [pattern] [path]
```

 ### Find a file

```
fd file.txt
```

 ### Find all Python files

```
fd "\.py$"
```

 Or usually:

```
fd -e py
```

 `-e` means extension.

 ### Search in a specific directory

```
fd config ~/projects
```

 ### Find directories

```
fd -t d
```

 Files:

```
fd -t f
```

 Symlinks:

```
fd -t l
```

 ### Case-insensitive search

 `fd` is generally case-insensitive for simple patterns unless the pattern requires otherwise.

 You can explicitly use:

```
fd -i README
```

 ### Hidden files

 By default, `fd` generally ignores hidden files.

 Use:

```
fd -H
```

 ### Include ignored files

 For example, files ignored by Git:

```
fd -I
```

 Combine:

```
fd -HI
```

 ### Search by extension

```
fd -e js
```

```
fd -e jpg -e png
```

 ### Search directories by name

```
fd -t d node_modules
```

 ### Execute commands

 For example:

```
fd -e tmp -x rm {}
```

 `-x` executes a command for every result.

 A safer alternative is to inspect results first:

```
fd -e tmp
```

---

 # 4\. `ripgrep` / `rg` — Modern alternative to `grep`

 `ripgrep` is usually invoked as:

```
rg
```

 It's designed for **fast recursive text searching**.

 ### Search current directory

```
rg "hello"
```

 Unlike basic `grep`, `rg` recursively searches the current directory by default.

 ### Search a specific directory

```
rg "hello" src/
```

 ### Ignore case

```
rg -i "hello"
```

 ### Show line numbers

```
rg -n "hello"
```

 ### Search a particular file type

```
rg "TODO" -t py
```

 Search JavaScript:

```
rg "console.log" -t js
```

 ### Search by glob

```
rg "TODO" -g "*.py"
```

 ### Search hidden files

```
rg --hidden "password"
```

 ### Search ignored files too

```
rg --no-ignore "password"
```

 ### Search exact words

```
rg -w "cat"
```

 ### Invert matching

```
rg -v "debug"
```

 ### Only output matching text

```
rg -o "https?://[^ ]+"
```

 ### Count matches

```
rg -c "TODO"
```

 ### Show only filenames

```
rg -l "TODO"
```

 ### Use regular expressions

```
rg "error|warning|critical"
```

---

 # 5\. `find` \+ `grep`

 These commands become particularly powerful when combined.

 Suppose you want to search for `"TODO"` only inside `.py` files.

```
find . -name "*.py" -exec grep -n "TODO" {} \;
```

 Breakdown:

```
find .                 → search current directory
-name "*.py"           → only Python files
-exec                  → execute another command
grep -n "TODO"         → search TODO and show line numbers
{}                     → current file
\;                     → end command
```

 Another approach:

```
find . -type f -name "*.py" -print0 | xargs -0 grep -n "TODO"
```

 `-print0` and `xargs -0` make this safer for filenames containing spaces/newlines.

 But for this particular task, **`rg` is much simpler**:

```
rg -n "TODO" -t py
```

---

 # 6\. `fd` \+ `rg`

 This is an extremely useful combination.

 Suppose:

 > Find Python files and search them for `database`.

```
fd -e py -x rg "database" {}
```

 Or you can often just use:

```
rg "database" -t py
```

 ### Search JavaScript files

```
fd -e js -x rg "fetch\(" {}
```

 ### Search only files matching a filename pattern

```
fd "config" -t f -x rg "database" {}
```

---

 # 7\. `find` vs `fd`

 | Task | `find` | `fd` |
| --- | --- | --- |
| Find files | ✅ | ✅ |
| Find directories | ✅ | ✅ |
| Find by name | ✅ | ✅ |
| Find by size | ✅ | Limited |
| Find by modification time | ✅ | Limited |
| Complex filesystem conditions | ✅ Excellent | ⚠️ |
| Simple everyday search | ⚠️ | ✅ Excellent |
| Easy syntax | ⚠️ | ✅ |
| Execute commands | ✅ | ✅ |

### Example

 Traditional:

```
find . -type f -name "*.jpg"
```

 Modern:

```
fd -t f -e jpg
```

 Use **`find`** when you need sophisticated filesystem conditions. Use **`fd`** for quick file/directory discovery.

---

 # 8\. `grep` vs `rg`

 | Task | `grep` | `rg` |
| --- | --- | --- |
| Search text | ✅ | ✅ |
| Recursive search | `grep -r` | Default |
| Regex | ✅ | ✅ |
| Fast large codebases | Good | ⭐ Excellent |
| Git-aware | ❌ | ✅ |
| File-type filtering | Less convenient | ⭐ Excellent |
| Simple command | ✅ | ✅ |

Example:

```
grep -rn "TODO" .
```

 Modern equivalent:

```
rg -n "TODO"
```

 Python only:

```
rg -n "TODO" -t py
```

---

 # 9\. The most useful combinations

 ### Find a file

```
fd filename
```

 ### Find all Python files

```
fd -e py
```

 ### Search for text everywhere

```
rg "hello"
```

 ### Search text in Python files

```
rg "hello" -t py
```

 ### Search case-insensitively

```
rg -i "hello"
```

 ### Search with line numbers

```
rg -n "hello"
```

 ### Search exact word

```
rg -w "error"
```

 ### Search hidden files

```
rg --hidden "TODO"
```

 ### Find files and then search them

```
fd -e py -x rg "TODO" {}
```

 ### Traditional Unix approach

```
find . -type f -name "*.py" -exec grep -n "TODO" {} \;
```

---

 # 10\. A practical mental model

 Think of the commands like this:

```
                 What are you looking for?
                         │
             ┌───────────┴───────────┐
             │                       │
          FILES                    TEXT
             │                       │
        ┌────┴────┐             ┌────┴────┐
        │         │             │         │
      find       fd            grep       rg
     powerful   simple        classic    modern
```

 So if you ask:

 > **"Where is this file?"**

 Use:

```
fd filename
```

 or:

```
find . -name "filename"
```

 If you ask:

 > **"Where is this text?"**

 Use:

```
rg "text"
```

 or:

```
grep -rn "text" .
```

 If you need **complex file conditions**, reach for `find`.

 If you're working in a **codebase and searching source code**, `rg` is usually the best first choice.

 ### A good cheat sheet

```
# FIND
find . -name "*.txt"
find . -type f
find . -type d
find . -size +100M
find . -mtime -7

# FD
fd file
fd -e py
fd -t f
fd -t d
fd -H pattern
fd pattern -x command {}

# GREP
grep "text" file
grep -i "text" file
grep -n "text" file
grep -r "text" .
grep -rn "text" .
grep -w "word" file
grep -v "text" file
grep -E "foo|bar" file

# RIPGREP
rg "text"
rg -i "text"
rg -n "text"
rg -w "word"
rg -l "text"
rg -t py "text"
rg -g "*.js" "text"
rg --hidden "text"
rg --no-ignore "text"
```

 The key distinction to remember is: **`find`/`fd` locate files; `grep`/`rg` locate text inside files.**
