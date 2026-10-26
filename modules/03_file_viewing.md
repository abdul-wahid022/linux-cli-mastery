# Module 3: File Viewing & Text Tools

**Objective:** Read, search, count and compare file contents, and combine commands with pipes and redirection.

| Section | Topic |
| ------- | ----- |
| [1](#1-viewing-files) | Viewing files: `cat` `less` `more` `head` `tail` |
| [2](#2-live-monitoring) | Live monitoring: `tail -f` |
| [3](#3-searching-with-grep) | Searching: `grep` |
| [4](#4-counting-with-wc) | Counting: `wc` |
| [5](#5-comparing-files) | Comparing: `diff` `cmp` |
| [6](#6-pipes-and-redirection) | Pipes and redirection |
| [7](#7-text-processing-tools) | `sort` `uniq` `cut` `tr` |
| [8](#practice) | Practice, common mistakes |

These commands only read files, so they are safe to practise on. Some system files need `sudo` to read.

---

## 1. Viewing files

| Command | What it does | Example |
| ------- | ------------ | ------- |
| `cat <file>` | Print the whole file | `cat file.txt` |
| `tac <file>` | Print the file with the last line first | `tac file.txt` |
| `nl <file>` | Print with line numbers | `nl file.txt` |
| `less <file>` | Scrollable viewer, best for large files | `less file.txt` |
| `more <file>` | Simple page-by-page viewer | `more file.txt` |
| `head <file>` | First 10 lines | `head file.txt` |
| `head -n 20 <file>` | First 20 lines | `head -n 20 file.txt` |
| `tail <file>` | Last 10 lines | `tail file.txt` |
| `tail -n 50 <file>` | Last 50 lines | `tail -n 50 file.txt` |

**Keys inside `less`**

| Key | Action |
| --- | ------ |
| `Space` / `b` | Next / previous page |
| `g` / `G` | Start / end of file |
| `/word` | Search forward (`n` for next match) |
| `?word` | Search backward |
| `q` | Quit |

Use `cat` for short files and `less` for long ones.

---

## 2. Live monitoring

`tail -f` prints new lines as they are added to a file. This is how you watch a log in real time.

```bash
echo "Line 1" > test.log
tail -f test.log
```

In a second terminal:

```bash
echo "Line 2" >> test.log
```

`Line 2` appears immediately in the first terminal. Stop with `Ctrl+C`.

| Command | What it does |
| ------- | ------------ |
| `tail -f file` | Follow a file |
| `tail -F file` | Follow, and keep following if the file is rotated or recreated |
| `tail -n 100 -f file` | Show the last 100 lines, then follow |

---

## 3. Searching with grep

`grep` prints the lines that match a pattern.

| Command | What it does | Example |
| ------- | ------------ | ------- |
| `grep <pattern> <file>` | Lines containing the pattern | `grep hello file.txt` |
| `grep -i` | Ignore case | `grep -i error log.txt` |
| `grep -n` | Show line numbers | `grep -n error log.txt` |
| `grep -c` | Count matching **lines** | `grep -c error log.txt` |
| `grep -v` | Lines that do **not** match | `grep -v success log.txt` |
| `grep -w` | Match whole words only | `grep -w cat file.txt` |
| `grep -r` | Search directories recursively | `grep -r error /var/log/` |
| `grep -E` | Extended regex, for example `a\|b` | `grep -E "error\|warning" log.txt` |
| `grep -A 3` / `-B 3` / `-C 3` | Show 3 lines After / Before / around each match | `grep -C 3 error log.txt` |

```bash
grep "cat" *.txt
```
```
animals.txt:cat
file.txt:this is a cat
file.txt:cat likes milk
```

> `grep -c` counts the **lines** that match, not the number of occurrences. A line with `error` twice counts once.

---

## 4. Counting with wc

| Command | What it does |
| ------- | ------------ |
| `wc file` | Lines, words and bytes |
| `wc -l file` | Number of lines |
| `wc -w file` | Number of words |
| `wc -c file` | Number of bytes |
| `wc -m file` | Number of characters |

```bash
wc myfile.txt
```
```
  15  120  856 myfile.txt
```

That is 15 lines, 120 words and 856 bytes.

---

## 5. Comparing files

| Command | What it does |
| ------- | ------------ |
| `diff a b` | Show line differences |
| `diff -u a b` | Unified format, easier to read |
| `diff -y a b` | Side by side |
| `diff -i a b` | Ignore case |
| `cmp a b` | Compare byte by byte, report the first difference |

```bash
echo -e "Line 1\nLine 2\nLine 3" > file1.txt
echo -e "Line 1\nLine 2 Modified\nLine 3" > file2.txt
diff file1.txt file2.txt
```
```
2c2
< Line 2
---
> Line 2 Modified
```

`2c2` means line 2 changed. `<` is from the first file, `>` is from the second. Use `cmp` to check quickly whether two files are identical.

---

## 6. Pipes and redirection

Every command has three streams: standard input (0), standard output (1) and standard error (2).

| Symbol | What it does | Example |
| ------ | ------------ | ------- |
| `\|` | Send the output of one command into the next | `ps aux \| grep ssh` |
| `>` | Write output to a file (overwrite) | `ls > list.txt` |
| `>>` | Append output to a file | `date >> log.txt` |
| `<` | Read input from a file | `wc -l < file.txt` |
| `2>` | Write errors to a file | `find / -name x 2> errors.txt` |
| `2>/dev/null` | Throw errors away | `find / -name x 2>/dev/null` |
| `&>` | Write output and errors to a file | `make &> build.log` |
| `tee` | Write to a file **and** show on screen | `ls \| tee list.txt` |

Examples:

```bash
grep "error" log.txt | wc -l          # count error lines
grep -i "error" log.txt | sort        # sort the matches
```

Pass the file to the command directly (`grep error file.txt`) instead of `cat file.txt | grep error`. It is shorter and faster.

---

## 7. Text processing tools

| Command | What it does | Example |
| ------- | ------------ | ------- |
| `sort` | Sort lines | `sort names.txt` |
| `sort -n` / `-r` | Numeric / reverse order | `sort -nr numbers.txt` |
| `uniq` | Remove **adjacent** duplicate lines (sort first) | `sort data.txt \| uniq` |
| `uniq -c` | Count each unique line | `sort data.txt \| uniq -c` |
| `cut -d',' -f1` | Take a column from delimited text | `cut -d',' -f1 data.csv` |
| `tr 'a-z' 'A-Z'` | Translate characters | `echo hi \| tr 'a-z' 'A-Z'` |

A very common pattern: count how often each line appears and show the most frequent first.

```bash
sort data.txt | uniq -c | sort -nr | head
```

---

## Practice

1. Create `animals.txt` with five lines, one animal per line, including a duplicate.
2. Show the first 2 lines, the last 2 lines and the line count.
3. Find the lines containing `cat`, ignoring case, with line numbers.
4. Sort the file and count how many times each animal appears.
5. Create a copy with one word changed and compare the two with `diff -u`.
6. Run `tail -f` on a file in one terminal and append lines from another.
7. Save the result of step 4 into `report.txt` with `tee`.

## Common mistakes

| Mistake | What happens | Fix |
| ------- | ------------ | --- |
| `cat` on a huge or growing log | Floods the terminal | Use `less`, `tail` or `tail -f` |
| `>` instead of `>>` | Overwrites the file | Use `>>` to append |
| `uniq` without `sort` | Only adjacent duplicates are removed | `sort \| uniq` |
| Reading `grep -c` as total matches | It counts lines | Use `grep -o word file \| wc -l` for occurrences |
| `grep` on `/var/log/auth.log` that does not exist | `No such file` | Newer Debian and Kali use the journal, see [Module 6](../06-system-logs/README.md) |

---

[Previous: Compression & Archiving](../02-compression-and-archiving/README.md) · [Back to index](../../README.md) · [Next: System & Processes](../04-system-and-processes/README.md)
