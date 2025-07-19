# 📦 Compress and Extract Commands for DevOps Engineers

This README provides an in-depth guide to the most commonly used compression and extraction tools in Linux. These tools are essential for DevOps engineers for managing backups, packaging artifacts, optimizing storage, and automating CI/CD workflows.

---

## 🔐 **Zip**

`zip` is used to compress one or more files into a single `.zip` archive file. It reduces file size and is commonly used for bundling logs, config files, or deployment packages.

### ✅ Basic Syntax

```bash
zip [options] zipfile_name.zip file1 file2 file3
```

### 🔧 Commonly Used Options

| Option       | Description                                |
| ------------ | ------------------------------------------ |
| `-r`         | Recursively zip directories and contents   |
| `-v`         | Verbose output (shows compression info)    |
| `-q`         | Quiet mode (no output)                     |
| `-m`         | Move files into zip and delete originals   |
| `-d`         | Delete specific files from an existing zip |
| `-u`         | Update changed files in zip                |
| `-x`         | Exclude specific files                     |
| `-e`         | Encrypt zip with password                  |
| `-T`         | Test the integrity of the archive          |
| `-0` to `-9` | Compression level (0 = none, 9 = max)      |

### 📌 DevOps Use Cases

* Compressing build artifacts for storage or upload to S3
* Zipping configuration files before transferring
* Archiving old logs with `-m` to save space

### 📂 Example

```bash
zip -r project_backup.zip /var/log/myapp -x "*.log"
```

---

## 🗂️ **Unzip**

`unzip` is used to extract `.zip` archives.

### ✅ Basic Syntax

```bash
unzip [options] archive.zip
```

### 🔧 Commonly Used Options

| Option | Description                      |
| ------ | -------------------------------- |
| `-l`   | List contents without extracting |
| `-v`   | Verbose file info                |
| `-d`   | Extract to a specific directory  |
| `-n`   | Never overwrite existing files   |
| `-o`   | Overwrite without prompting      |
| `-x`   | Exclude certain files            |
| `-q`   | Quiet mode                       |
| `-t`   | Test archive integrity           |

### 📌 DevOps Use Cases

* Extracting deployment bundles
* Deploying zipped infrastructure scripts
* Verifying archive contents before extracting

### 📂 Example

```bash
unzip app_build.zip -d /opt/releases/
```

---

## 🪵 **Tar**

`tar` (tape archive) is used to **bundle** multiple files or directories. It is usually combined with compression tools like `gzip` or `bzip2`.

### ✅ Basic Syntax

```bash
tar [options] archive.tar file1 file2 dir1
```

### 🔧 Commonly Used Options

| Option      | Description                                        |
| ----------- | -------------------------------------------------- |
| `-c`        | Create new archive                                 |
| `-x`        | Extract archive                                    |
| `-t`        | List archive contents                              |
| `-v`        | Verbose (shows progress)                           |
| `-f`        | Specifies archive name (must come before filename) |
| `-z`        | Compress using gzip (`.tar.gz`)                    |
| `-j`        | Compress using bzip2 (`.tar.bz2`)                  |
| `--exclude` | Skip specific files or directories                 |

### 📌 DevOps Use Cases

* Creating full backups of config directories
* Packaging Helm charts, Terraform modules, or Ansible roles
* Shipping `.tar.gz` artifacts in CI/CD pipelines

### 📂 Example

```bash
tar --exclude="*.log" -czvf config_backup.tar.gz /etc/myapp/
```

---

## 🗜️ **Gzip**

`gzip` is a simple compression tool for **individual files**. For folders, it is combined with `tar`.

### ✅ Common Syntax

```bash
gzip [options] file
```

### 🔧 Commonly Used Options

| Option | Description                               |
| ------ | ----------------------------------------- |
| `-k`   | Keep original file                        |
| `-v`   | Verbose (shows compression ratio)         |
| `-d`   | Decompress (`gunzip` does the same)       |
| `-r`   | Compress files recursively in directories |

### 📌 DevOps Use Cases

* Compressing large log files
* Archiving single configuration files
* Used in automation scripts to reduce disk usage

### 📂 Example

```bash
gzip -k -v server.log
```

To decompress:

```bash
gunzip server.log.gz
```

---

## 🧪 Daily DevOps Tasks with These Commands

| Task                         | Tool       | Command                                              |
| ---------------------------- | ---------- | ---------------------------------------------------- |
| Backup logs before rotation  | tar + gzip | `tar -czvf logs_backup.tar.gz /var/log/nginx/`       |
| Archive old builds           | zip        | `zip -r old_builds.zip /builds/2023/`                |
| Extract deployment packages  | unzip      | `unzip release.zip -d /opt/releases/`                |
| Compress and move reports    | gzip       | `gzip -k report.txt && mv report.txt.gz /backups/`   |
| Exclude secrets from archive | tar        | `tar --exclude='secrets.env' -czvf app.tar.gz ./app` |

---

## ✅ Summary

| Tool     | Compress              | Extract          | Folder Support | Notes                             |
| -------- | --------------------- | ---------------- | -------------- | --------------------------------- |
| `zip`    | ✅                     | ✅                | ✅ (with `-r`)  | Cross-platform, password support  |
| `unzip`  | ❌                     | ✅                | ✅              | Easy to extract and list contents |
| `tar`    | ✅ (with `-z` or `-j`) | ✅                | ✅              | Widely used in Linux environments |
| `gzip`   | ✅ (single file)       | ❌ (use `gunzip`) | ❌              | Best for logs and flat files      |
| `gunzip` | ❌                     | ✅                | ❌              | Use with `.gz` files only         |

This guide is essential for mastering file compression workflows as a DevOps engineer. Practice each command, and integrate them into scripts and automation tasks for maximum productivity.
