# 📘 Linux Users and Groups - Complete Beginner to Pro Guide

## 👤 Types of Users in Linux

Linux supports multiple types of users. Understanding user types is foundational for managing permissions and system access:

1. **Root User**:

   * Username: `root`
   * UID: `0`
   * Superuser with complete control over the system.

2. **System Users**:

   * UID: `1` to `999`
   * Created by the system or during software installations (e.g., `daemon`, `nobody`)
   * Used for running services, not for interactive logins.

3. **Regular Users**:

   * UID: `1000` and above
   * Created by system admins or during OS installation
   * Used for general interactive login with limited privileges

> 📁 All user information is stored in `/etc/passwd`

### 🔍 View All Users

```bash
cat /etc/passwd | cut -d: -f1
```

---

## ➕ How to Add a User

### ✅ Method 1 (Recommended):

```bash
sudo adduser devuser
```

* Prompts for password and user information
* Automatically creates home directory

### ⚙️ Method 2 (Manual):

```bash
sudo useradd devuser1         # No home directory
sudo useradd -m devuser2      # Create home directory
sudo passwd devuser2          # Set password manually
```

> 📁 User home directories are created under `/home/username`

---

## ❌ How to Delete a User

```bash
sudo deluser username                     # Keeps home directory
sudo deluser --remove-home username       # Deletes home directory too
```

---

## 👥 Understanding Groups in Linux

### 🔹 What is a Group?

* A group is a collection of users.
* Helps manage permissions collectively.

### 📌 Each user:

1. Has one **Primary Group** (assigned at user creation)
2. Can belong to multiple **Secondary Groups**

### 📁 All group info is stored in `/etc/group`

### 🔍 View All Groups

```bash
cut -d: -f1 /etc/group
```

### 🔍 Check Group Info

```bash
getent group groupname
```

---

## ➕ How to Create a Group

```bash
sudo addgroup devgroup
```

## ❌ How to Delete a Group

```bash
sudo delgroup groupname
```

---

## 👤➕ How to Add a User to a Group

```bash
sudo usermod -aG groupname username
```

### 🔍 Verify User's Groups

```bash
groups username
```

## ➖ Remove a User from a Group

```bash
sudo gpasswd -d username groupname
```

---

## 🔐 Managing User Passwords

### 🔄 Change User Password

```bash
sudo passwd username
```

### ⌛ Expire Password Immediately

```bash
sudo chage -d 0 username
```

### 📆 View Account Aging Info

```bash
sudo chage -l username
```

---

## 🔄 Switch Users

```bash
su username
```

---

## 🐚 Shells & User Info

### 🔍 Check Current Shell

```bash
grep username /etc/passwd
```

### ✍️ Change Login Shell

```bash
sudo chsh -s /bin/bash username
```

### 📝 Edit User Info (Full Name, etc.)

```bash
sudo usermod -c "Full Name Info" username
```

---

## ✅ Summary Table

| Command            | Purpose                       |
| ------------------ | ----------------------------- |
| `adduser`          | Add user with home and prompt |
| `useradd`          | Add user (manual setup)       |
| `passwd`           | Set/change password           |
| `deluser`          | Remove user                   |
| `addgroup`         | Create new group              |
| `delgroup`         | Delete group                  |
| `usermod -aG`      | Add user to group             |
| `gpasswd -d`       | Remove user from group        |
| `chage`            | Manage account aging          |
| `chsh`             | Change login shell            |
| `grep /etc/passwd` | View user info                |

---

This guide is all you need to become a pro in Linux user and group management 💪. Continue experimenting with commands on your system for hands-on mastery!
