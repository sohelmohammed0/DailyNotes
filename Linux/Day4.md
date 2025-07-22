
# 🔧 Linux Commands: `grep`, `chmod`, `chown`

This guide explains three powerful Linux commands used frequently in DevOps and system administration.

---

## 📘 1. Grep Command

### 🔹 What is `grep`?
`grep` stands for **Global Regular Expression Print**.  
It is used to **search for specific patterns** in text files.  
By default, `grep` is **case-sensitive**.

### 📌 Common `grep` Syntax

| Command                          | Description |
|----------------------------------|-------------|
| `grep pattern filename`          | Shows all lines that match the pattern |
| `grep -o pattern filename`       | Prints only the matched word, not the whole line |
| `grep -n pattern filename`       | Displays line numbers of matching lines |
| `grep -i pattern filename`       | Ignores case sensitivity |
| `grep -v pattern filename`       | Excludes lines containing the pattern |
| `grep -v pattern file1 > file2`  | Saves the filtered result into another file |

### 🧪 Example File: `fruits.txt`
```
Apple
apple pie
Banana
Mango
apple juice
Grapes
```

### 🔍 Example Usage
```bash
grep apple fruits.txt
grep -o apple fruits.txt
grep -n apple fruits.txt
grep -i apple fruits.txt
grep -v apple fruits.txt
grep -v apple fruits.txt > no_apples.txt
```

---

## 🔐 2. Chmod Command

### 🔹 What is `chmod`?
`chmod` stands for **Change Mode**.  
It is used to **change file or directory permissions** (read, write, execute).

### 👥 Permission Categories

| Symbol | Refers to     |
|--------|---------------|
| `u`    | User (owner)  |
| `g`    | Group         |
| `o`    | Others        |
| `a`    | All (u+g+o)   |

### 🔢 Permission Values

| Permission | Symbol | Value |
|------------|--------|-------|
| Read       | `r`    | 4     |
| Write      | `w`    | 2     |
| Execute    | `x`    | 1     |

**Combine permissions**:  
- `rwx` = 4 + 2 + 1 = 7  
- `rw-` = 4 + 2 = 6  
- `r--` = 4  

### 🔧 Example Usage
```bash
chmod 755 script.sh        # rwxr-xr-x
chmod 777 data.txt         # rwxrwxrwx
chmod 644 notes.txt        # rw-r--r--
chmod u+x run.sh
chmod g-w file.txt
chmod a+rx public.sh
```

---

## 👤 3. Chown Command

### 🔹 What is `chown`?
`chown` stands for **Change Owner**.  
It changes the **owner or group** of a file or directory.

### 📌 Basic Syntax
```bash
chown new_user filename
chown user:group filename
```

### 🔧 Example Usage
```bash
sudo chown sohel myscript.sh
sudo chown sohel:developers project1/
ls -l file.txt    # Check owner and group
```

---

## ✅ Summary

| Command | Purpose                                 |
|---------|-----------------------------------------|
| `grep`  | Search for text patterns inside files   |
| `chmod` | Change file or folder permissions       |
| `chown` | Change file/folder ownership            |

> 🧠 **Pro Tip**: Always double-check permissions and ownership using `ls -l` after making changes!
