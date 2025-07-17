# 🐧 Linux Commands Reference

This README.md summarizes **essential Linux commands** for file and directory management, based on my class notes while revising Linux and DevOps fundamentals.

These commands are primarily stored in `/bin` or `/usr/bin` in the **Linux Filesystem Hierarchy Standard (FHS)**, making them core tools for system interaction.

> 🎯 Perfect for beginners or anyone brushing up on Linux skills!

---

## 📄 File Creation Commands

### `cat`
- `cat > filename` – Creates or overwrites a file with input.
- `cat >> filename` – Appends data to a file.
- `cat sourcefile >> destfile` – Appends sourcefile content to destfile.

### `touch`
- `touch filename` – Creates an empty file or updates its timestamp.

### `vi` – Text Editor
- `vi filename` – Opens/creates a file in vi editor.

**Basic Workflow:**
- `i` – Insert mode
- `Esc :w` – Save
- `:q` – Quit
- `:x` – Save and quit

**Navigation:**
- `gg` – Go to first line
- `G` – Go to last line
- `w/b` – Move forward/backward by word
- `nw/nb` – Move n words forward/backward
- `u` – Undo changes
- `yy/nyy` – Copy line(s)
- `p/P` – Paste below/above cursor
- `dw` – Delete word
- `dd/ndd` – Delete line(s)
- `/word` – Search and highlight word

**Insert Modes:**
- `i` – Insert at cursor
- `I` – Insert at line start
- `a` – Insert after cursor
- `A` – Insert at line end
- `o/O` – Insert new line below/above

**Extended Options:**
- `:set nu` – Show line numbers
- `:set nonu` – Hide line numbers

---

## 📁 File and Directory Operations

### `cp` – Copy Files/Directories
- `cp source dest` – Copies a file to dest.
- `cp file dirname` – Copies a file into a directory.
- `cp -r dir1 dir2` – Copies a directory and its contents.
- `cp -rvfp dir1 dir2` – Verbose, preserve permissions, force copy.
- `cp -b source dest` – Backup of dest before copy.

### `mv` – Move or Rename
- `mv file dirname` – Moves a file to a directory.
- `mv source dest` – Moves files/directories to another path.
- `mv oldname newname` – Renames a file or directory.

### `mkdir` – Create Directories
- `mkdir dirname` – Creates a single directory.
- `mkdir dir1 dir2` – Creates multiple directories.
- `mkdir dir{1..n}` – Creates a range of directories.
- `mkdir -p parent/child` – Creates nested directories.

### `rm`, `rmdir` – Remove Files/Directories
- `rm filename` – Deletes a file.
- `rmdir dirname` – Deletes an empty directory.
- `rm -rf dirname` – Deletes directory and contents recursively (⚠️ caution).

---

## 🔍 Filtering Commands

- `head -n filename` – Shows first n lines of a file.
- `tail -n filename` – Shows last n lines of a file.
- `sed 's/old/new/g' filename` – Replace all 'old' with 'new' in a file.
- `cut -b pos filename` – Extracts characters at position pos.

---

## 💻 Usage Examples

Run these in a Linux terminal (e.g., Ubuntu, WSL):

```bash
touch test.txt                  # Create a file
vi test.txt                     # Edit with vi (i to insert, Esc :x to save)
cp test.txt test_copy.txt       # Copy
mv test_copy.txt mydir/         # Move
head -2 test.txt                # View top lines
```

---

## 📚 About

I’m revising **Linux and DevOps core topics** to master system administration and automation.

These commands are foundational for navigating and managing Linux systems. Try them out and explore the `/bin` directory:

```bash
ls /bin
```

Happy learning! 🚀
