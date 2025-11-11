# Module 2: File Compression and Archiving

This module covers the essential Linux commands for compressing and archiving files. While often used together, it's important to understand the difference:

*   **Archiving** is the process of combining multiple files and directories into a single file (an "archive"). This makes it easier to manage and transfer a large collection of files.
*   **Compression** is the process of reducing the size of a file to save disk space or reduce transfer time.

We will primarily focus on the two most common tools: `tar` for archiving and `gzip`/`zip` for compression.

### Table of Contents
1.  [The `tar` Command](#the-tar-command)
2.  [The `gzip` Command](#the-gzip-command)
3.  [The `zip` and `unzip` Commands](#the-zip-and-unzip-commands)
4.  [Summary of Key Concepts](#summary-of-key-concepts)

---

### 1. The `tar` Command

`tar` (short for **T**ape **AR**chive) is the standard tool used to **create, extract, and list contents** of archive files (commonly `.tar` files) on Linux. It bundles multiple files into a single `.tar` file without compressing them.

#### Creating an Archive (`tar -cvf`)

To create a new archive, We use:
- `-c` – Create a new archive
- `-v` – Verbose output (show progress)
- `-f` – Specify filename

**Syntax:**
```bash
tar -cvf [archive_name.tar] [files_or_directories_to_archive]
```

**Example:**
```bash
tar -cvf myarchive.tar folder1/
```
This command creates an archive named `myarchive.tar` containing `folder1` and all of its contents.

#### Extracting an Archive (`tar -xvf`)

To extract files from an archive, We use:
- `-x` – Extract files
- `-v` – Verbose output
- `-f` – Specify filename

**Syntax:**
```bash
tar -xvf [archive_name.tar]
```

**Example:**```bash
tar -xvf myarchive.tar
```
This command extracts all files from `myarchive.tar` into the current directory.

#### Listing Archive Contents (`tar -tvf`)

To view the contents of an archive without extracting it, you use the `t` (list) option.

**Syntax:**
```bash
tar -tvf [archive_name.tar]
```
- `-t` – List contents of archive
- `-v` – Verbose output
- `-f` – Specify filename

**Example:**
```bash
tar -tvf myarchive.tar
```
This command lists all the files and directories contained within `myarchive.tar`.

---

### 2. gzip / gunzip – Compress / Decompress .tar.gz Files

`gzip` is a popular compression tool used to reduce the size of a single file. It is often used to compress a `.tar` archive, resulting in a `.tar.gz` file (also known as a "tarball").

#### Compressing with `gzip`

`gzip` compresses a file and replaces the original with a new, smaller file with a `.gz` extension.

**Syntax:**
```bash
gzip [filename]
```

**Example:**
```bash
gzip myarchive.tar
```
This command compresses `myarchive.tar` and renames it to `myarchive.tar.gz`. The original `myarchive.tar` file is removed.

#### Decompressing with `gunzip`

`gunzip` is used to decompress files created with `gzip`.

**Syntax:**
```bash
gunzip [filename.gz]
```

**Example:**
```bash
gunzip myarchive.tar.gz
```
This command decompresses `myarchive.tar.gz` back to `myarchive.tar`. The compressed file is removed.
> 🔁 **Tip:** You can combine `tar` and `gzip` in one step:
```bash
tar -czvf myarchive.tar.gz folder/
```
This creates a compressed archive directly.
---

### 3.zip / unzip – Create and Extract ZIP Archives

`zip` is a convenient tool for creating archives and compressing them in a single step, often used for cross-platform compatibility (especially with Windows).

#### Creating a ZIP Archive

To create a `.zip` archive, use the `zip` command. For directories, you must use the `-r` (recursive) option.

**Syntax:**
```bash
zip -r [archive_name.zip] [files_or_directories]
```
- `-r` – Recursively include all files and subdirectories
> ⚠️ Without `-r`, only the folder structure (without contents) is added.

**Example:**
```bash
zip -r myfolder.zip folder1/```
This command creates a compressed archive named `myfolder.zip` containing `folder1` and all its sub-folders and files.

#### Extracting a ZIP Archive

To extract the contents of a `.zip` archive, use the `unzip` command.

**Syntax:**
```bash
unzip [archive_name.zip]
```

**Example:**
```bash
unzip myfolder.zip
```
This command extracts the contents of `myfolder.zip` into the current directory.

---

### 4. Summary of Key Concepts

| Command         | Purpose                                     | Options / Flags                                | Example                                  |
| :-------------- | :------------------------------------------ | :--------------------------------------------- | :--------------------------------------- |
| `tar -cvf`      | Create an archive                           | `-c` (create), `-v` (verbose), `-f` (filename) | `tar -cvf archive.tar folder/`           |
| `tar -xvf`      | Extract an archive                          | `-x` (extract)                                 | `tar -xvf archive.tar`                   |
| `tar -tvf`      | List contents of an archive                 | `-t` (list)                                    | `tar -tvf archive.tar`                   |
| `gzip`          | Compress a single file                      |                                                | `gzip file.tar`                          |
| `gunzip`        | Decompress a `.gz` file                     |                                                | `gunzip file.tar.gz`                     |
| `zip -r`        | Create a compressed `.zip` archive          | `-r` (recursive, for folders)                  | `zip -r archive.zip folder/`             |
| `unzip`         | Extract a `.zip` archive                    |                                                | `unzip archive.zip`                      |

By mastering these commands, you can efficiently manage your files by archiving them for organization or compressing them to save disk space
