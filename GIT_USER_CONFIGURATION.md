# Git User Configuration Guide

## Overview

When managing multiple GitHub accounts, it's critical to set the correct `user.name` and `user.email` for each repository to avoid committing under the wrong identity.

This guide shows you how to:

- Remove global Git configuration
- Set local username and email per repository
- Verify your settings before committing

---

## Step 1: Remove Global Configuration

### Remove Global User Name

```bash
git config --global --unset user.name
```

**Expected output:** None (silent success)

### Remove Global User Email

```bash
git config --global --unset user.email
```

**Expected output:** None (silent success)

### Verify Globals Are Removed

```bash
git config --global --list
```

**Expected output:** Should NOT show `user.name=` or `user.email=`

Example output:

```
core.editor=vim
core.autocrlf=true
...
(no user.name or user.email)
```

---

## Step 2: Set Local Configuration Per Repository

### Navigate to Your Repository

```bash
cd path/to/your/repository
```

### Set Local User Name

```bash
git config user.name "Your Full Name"
```

**Replace** `"Your Full Name"` with your actual name.

Examples:

- `git config user.name "John Doe"`
- `git config user.name "Sudhanshu Sabale"`

### Set Local User Email

```bash
git config user.email "your-email@example.com"
```

**Replace** `"your-email@example.com"` with your actual email.

**For personal repos:**

```bash
git config user.email "personal@gmail.com"
```

**For work repos:**

```bash
git config user.email "work@company.com"
```

---

## Step 3: Verify Your Settings

### Verify Local Configuration in Current Repo

```bash
git config --local --list
```

**Expected output:**

```
user.name=Your Full Name
user.email=your-email@example.com
core.repositoryformatversion=0
core.filemode=true
core.bare=false
...
```

### Check Specific Values

Check user name:

```bash
git config user.name
```

Expected output:

```
Your Full Name
```

Check user email:

```bash
git config user.email
```

Expected output:

```
your-email@example.com
```

### Verify Where Config Is Stored

```bash
git config --local --list | grep user
```

**Expected output:**

```
user.name=Your Full Name
user.email=your-email@example.com
```

This confirms settings are **local** (not global).

---

## Step 4: Test Before Committing

### Make a Test Commit (Optional)

If you want to verify the config works before making actual commits:

```bash
# Create a test file
echo "test" > test.txt

# Stage it
git add test.txt

# Make a test commit (don't push)
git commit -m "Test commit to verify user config"

# Check the commit details
git log -1 --pretty=format:"%an <%ae>"
```

**Expected output:**

```
Your Full Name <your-email@example.com>
```

### Verify Commit Author

To see the author of the last commit:

```bash
git log -1
```

**Expected output:**

```
commit abc123def456...
Author: Your Full Name <your-email@example.com>
Date:   Wed Jun 4 10:30:00 2026 +0000

    Test commit to verify user config
```

---

## Complete Workflow Example

### For Personal Repository

```bash
# 1. Navigate to repo
cd ~/Documents/my-personal-project

# 2. Set local config
git config user.name "Sudhanshu Sabale"
git config user.email "sudhanshu@gmail.com"

# 3. Verify
git config user.name
git config user.email

# 4. Make a commit
git add .
git commit -m "Update feature"

# 5. Check author
git log -1 --pretty=format:"%an <%ae>"
# Output: Sudhanshu Sabale <sudhanshu@gmail.com>
```

### For Work Repository

```bash
# 1. Navigate to repo
cd ~/Documents/company-project

# 2. Set local config
git config user.name "Sudhanshu Sabale"
git config user.email "sudhanshu@company.com"

# 3. Verify
git config user.name
git config user.email

# 4. Make a commit
git add .
git commit -m "Fix critical bug"

# 5. Check author
git log -1 --pretty=format:"%an <%ae>"
# Output: Sudhanshu Sabale <sudhanshu@company.com>
```

---

## Verification Checklist

Before pushing any commits, verify:

- [ ] Global `user.name` is unset

  ```bash
  git config --global user.name
  # Should return nothing or error
  ```

- [ ] Global `user.email` is unset

  ```bash
  git config --global user.email
  # Should return nothing or error
  ```

- [ ] Local `user.name` is set correctly

  ```bash
  git config user.name
  # Should show your name
  ```

- [ ] Local `user.email` is set correctly

  ```bash
  git config user.email
  # Should show correct email for this repo
  ```

- [ ] Commit author matches expected identity
  ```bash
  git log -1 --pretty=format:"%an <%ae>"
  # Should match your configured user.name and user.email
  ```

---

## Common Issues & Solutions

### Issue: "Please Tell Me Who You Are"

**Error message:**

```
*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.email "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.
```

**Solution:**
This means local config is not set. Run:

```bash
git config user.name "Your Name"
git config user.email "you@example.com"
```

(Note: Do NOT use `--global` flag)

---

### Issue: Wrong Author on Commit

**Symptom:** Commit shows wrong email or name

**Solution:**

1. Check what's set globally:

   ```bash
   git config --global --list | grep user
   ```

2. If global config exists, remove it:

   ```bash
   git config --global --unset user.name
   git config --global --unset user.email
   ```

3. Set correct local config:

   ```bash
   git config user.name "Correct Name"
   git config user.email "correct@email.com"
   ```

4. Verify:
   ```bash
   git config user.name
   git config user.email
   ```

---

### Issue: Config Not Persisting

**Symptom:** Settings work in one repo but not in another

**Cause:** Each repository needs its own local configuration

**Solution:**
Repeat the configuration steps for each repository:

```bash
cd repo1
git config user.name "Name"
git config user.email "email1@example.com"

cd ../repo2
git config user.name "Name"
git config user.email "email2@example.com"
```

---

## Quick Reference Commands

| Task                    | Command                                   |
| ----------------------- | ----------------------------------------- |
| Remove global name      | `git config --global --unset user.name`   |
| Remove global email     | `git config --global --unset user.email`  |
| View all global config  | `git config --global --list`              |
| Set local name          | `git config user.name "Your Name"`        |
| Set local email         | `git config user.email "you@example.com"` |
| View local name         | `git config user.name`                    |
| View local email        | `git config user.email`                   |
| View all local config   | `git config --local --list`               |
| View last commit author | `git log -1 --pretty=format:"%an <%ae>"`  |
| View commit details     | `git log -1`                              |

---

## File Locations

| Config Level     | Location                | Command to View              |
| ---------------- | ----------------------- | ---------------------------- |
| Global           | `~/.gitconfig`          | `git config --global --list` |
| Local (per repo) | `your-repo/.git/config` | `git config --local --list`  |

### Manual Editing (Advanced)

You can also edit files directly:

**Global config:**

```bash
notepad ~/.gitconfig
```

**Local config:**

```bash
notepad your-repo/.git/config
```

Look for sections like:

```
[user]
    name = Your Name
    email = you@example.com
```

---

## Tips for Multiple Accounts

✅ **Always set local config** before making your first commit in a new repo

✅ **Use `git config --local --list`** to verify before committing

✅ **Test with `git log`** to confirm commit author before pushing

✅ **Keep a checklist** when setting up new repositories

✅ **Use descriptive comments** in your commits if committing for both accounts in same day

Example commit message:

```bash
git commit -m "[personal] Updated project documentation"
git commit -m "[work] Fixed production bug in auth module"
```

---

## Troubleshooting Workflow

1. **You're in a repo and ready to commit**

   ```bash
   # First, verify your config
   git config user.name
   git config user.email
   ```

2. **If output is empty or wrong:**

   ```bash
   # Set the correct values
   git config user.name "Your Name"
   git config user.email "correct@email.com"
   ```

3. **Verify again:**

   ```bash
   git config user.name
   git config user.email
   ```

4. **Make your commit:**

   ```bash
   git add .
   git commit -m "Your message"
   ```

5. **Confirm the author:**

   ```bash
   git log -1 --pretty=format:"%an <%ae>"
   ```

6. **If correct, push:**

   ```bash
   git push origin branch-name
   ```

---

**Last Updated:** June 4, 2026  
**Status:** ✅ Ready to use
