# Git SSH Setup for Two GitHub Accounts

## What Was Changed

Changed the remote URL from **HTTPS** to **SSH** for the personal GitHub account.

### Before:

```
origin  https://github.com/s-cube-15/Python_LangChain.git (fetch)
origin  https://github.com/s-cube-15/Python_LangChain.git (push)
```

### After:

```
origin  git@github-personal:s-cube-15/Python_LangChain.git (fetch)
origin  git@github-personal:s-cube-15/Python_LangChain.git (push)
```

## Why This Fixes The Sign-In Dialog

- **HTTPS** requires interactive authentication → causes sign-in popup
- **SSH** uses your SSH keys automatically → no popup needed

---

## How To Set Up For Your Work Account

### Step 1: Navigate to Your Work Repository

```bash
cd path/to/your/work/repo
```

### Step 2: Check Current Remote URL

```bash
git remote -v
```

It should show HTTPS (if it has the popup issue):

```
origin  https://github.com/your-work-username/repo-name.git
```

### Step 3: Change to SSH (Work Account)

```bash
git remote set-url origin git@github-work:your-work-username/repo-name.git
```

**Replace:**

- `your-work-username` → your actual GitHub work username
- `repo-name` → actual repository name

### Step 4: Verify It Changed

```bash
git remote -v
```

Should now show:

```
origin  git@github-work:your-work-username/repo-name.git (fetch)
origin  git@github-work:your-work-username/repo-name.git (push)
```

### Step 5: Configure Git Identity for Work Repo

```bash
git config user.name "Your Work Name"
git config user.email "work@email.com"
```

---

## Summary

| Account      | Host Alias        | SSH Command                             |
| ------------ | ----------------- | --------------------------------------- |
| **Personal** | `github-personal` | `git@github-personal:username/repo.git` |
| **Work**     | `github-work`     | `git@github-work:username/repo.git`     |

Both use the SSH config you set up earlier, so no more sign-in dialogs! ✅
