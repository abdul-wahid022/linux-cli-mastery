# Module 2: Compression & Archiving

**Objective:** Bundle files into archives, compress them, extract them again and verify the result.

| Section | Topic |
| ------- | ----- |
| [1](#1-archiving-vs-compression) | Archiving vs compression |
| [2](#2-tar) | `tar` |
| [3](#3-gzip-bzip2-and-xz) | `gzip` `bzip2` `xz` |
| [4](#4-zip-and-unzip) | `zip` `unzip` |
| [5](#5-choosing-a-format) | Choosing a format |
| [6](#6-verifying-with-checksums) | Verifying with checksums |
| [7](#practice) | Practice, common mistakes |

---

## 1. Archiving vs compression

| Term | What it does | Tool |
| ---- | ------------ | ---- |
| **Archiving** | Combines many files and directories into one file. No size reduction. | `tar` |
| **Compression** | Makes a file smaller. | `gzip`, `bzip2`, `xz` |
| **Both in one step** | Archive and compress together. | `tar -z`, `tar -j`, `tar -J`, `zip` |

A `.tar.gz` (or `.tgz`) file is a tar archive compressed with gzip.

---

## 2. tar

`tar` (tape archive) creates, lists and extracts archives.

| Flag | Meaning |
| ---- | ------- |
| `-c` | Create an archive |
| `-x` | Extract an archive |
| `-t` | List the contents |
| `-f` | Archive file name (must come right before the name) |
| `-v` | Verbose, print files as they are processed |
| `-z` | Use gzip compression (`.tar.gz`) |
| `-j` | Use bzip2 compression (`.tar.bz2`) |
| `-J` | Use xz compression (`.tar.xz`) |
| `-C <dir>` | Change to a directory before extracting |
| `--exclude=<pattern>` | Skip matching files |

| Task | Command |
| ---- | ------- |
| Create an archive | `tar -cvf myarchive.tar folder1/` |
| Create a gzip archive | `tar -czvf myarchive.tar.gz folder1/` |
| Create a bzip2 archive | `tar -cjvf myarchive.tar.bz2 folder1/` |
| Create an xz archive | `tar -cJvf myarchive.tar.xz folder1/` |
| List contents | `tar -tvf myarchive.tar` |
| Extract into the current directory | `tar -xvf myarchive.tar` |
| Extract a gzip archive | `tar -xzvf myarchive.tar.gz` |
| Extract into another directory | `tar -xzvf myarchive.tar.gz -C /tmp/restore` |
| Extract a single file | `tar -xzvf myarchive.tar.gz folder1/notes.txt` |
| Exclude files | `tar -czvf backup.tar.gz --exclude='*.log' folder1/` |

Modern `tar` detects the compression type when extracting, so `tar -xvf file.tar.gz` also works.

---

## 3. gzip, bzip2 and xz

These tools compress **one file at a time**. By default they replace the original with the compressed file.

| Command | What it does |
| ------- | ------------ |
| `gzip file` | Compress, creates `file.gz` and removes `file` |
| `gzip -k file` | Compress and keep the original |
| `gzip -9 file` | Maximum compression (slower) |
| `gzip -d file.gz` or `gunzip file.gz` | Decompress |
| `zcat file.gz` | Print a compressed file without extracting it |
| `bzip2 file` / `bunzip2 file.bz2` | Compress / decompress with bzip2 |
| `xz file` / `unxz file.xz` | Compress / decompress with xz |

```bash
gzip -k myarchive.tar
ls -lh myarchive.tar*
```

---

## 4. zip and unzip

`zip` archives and compresses in one step and is readable on Windows and macOS.

| Command | What it does |
| ------- | ------------ |
| `zip -r myfolder.zip folder1/` | Create a zip of a directory |
| `zip archive.zip a.txt b.txt` | Zip individual files |
| `unzip myfolder.zip` | Extract into the current directory |
| `unzip myfolder.zip -d /tmp/out` | Extract into another directory |
| `unzip -l myfolder.zip` | List contents without extracting |

> Without `-r`, `zip` stores the directory entry only and leaves out everything inside it.

If `zip` or `unzip` is missing: `sudo apt install zip unzip`.

---

## 5. Choosing a format

| Format | Typical extension | Speed | Compression | Good for |
| ------ | ----------------- | ----- | ----------- | -------- |
| gzip | `.tar.gz`, `.tgz` | Fast | Good | Everyday backups, the default choice |
| bzip2 | `.tar.bz2` | Slower | Better than gzip | Larger archives where size matters |
| xz | `.tar.xz` | Slowest | Best | Distribution of software, long-term storage |
| zip | `.zip` | Fast | Good | Sharing with Windows and macOS users |

---

## 6. Verifying with checksums

A checksum proves that a file was not damaged or changed.

```bash
sha256sum myarchive.tar.gz > myarchive.sha256
sha256sum -c myarchive.sha256
```
```
myarchive.tar.gz: OK
```

`md5sum` works the same way but SHA-256 is preferred.

---

## Practice

1. Create a directory `data` with three text files inside.
2. Archive it as `data.tar`, then list the archive without extracting it.
3. Create `data.tar.gz`, `data.tar.bz2` and `data.tar.xz`, and compare their sizes with `ls -lh`.
4. Extract `data.tar.gz` into `/tmp/restore` using `-C`.
5. Create `data.zip` and list it with `unzip -l`.
6. Write a SHA-256 checksum for `data.tar.gz` and verify it.

## Common mistakes

| Mistake | What happens | Fix |
| ------- | ------------ | --- |
| `tar -cf` with a typo in the order (`tar -fc name`) | The next word is read as the file name | Put `f` last: `-cvf name` |
| Running `gzip file` and expecting the original to remain | Original is removed | Use `gzip -k` |
| `zip folder.zip folder/` without `-r` | Empty directory in the zip | Use `zip -r` |
| Extracting an untrusted archive in the current directory | Files can be scattered around | Extract into an empty directory with `-C` |

---

[Previous: Files & Directories](../01-files-and-directories/README.md) · [Back to index](../../README.md) · [Next: File Viewing](../03-file-viewing/README.md)
