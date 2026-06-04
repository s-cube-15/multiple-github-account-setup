# Comprehensive Git SSH Setup Guide for Multiple GitHub Accounts

**Last Updated:** June 4, 2026  
**Purpose:** Complete reference guide for managing two GitHub accounts (personal & work) using SSH authentication

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [SSH Key Setup](#ssh-key-setup)
3. [SSH Config File Setup](#ssh-config-file-setup)
4. [Repository Configuration](#repository-configuration)
5. [Troubleshooting](#troubleshooting)
6. [Verification Steps](#verification-steps)
7. [Quick Reference](#quick-reference)

---

## Prerequisites

### What You Need

- Windows 10+ (with Git Bash or PowerShell)
- Two GitHub accounts (personal & work)
- Admin access to generate SSH keys
- SSH keys already created (or steps to create them below)

### Check If Git Is Installed

```bash
git --version
```

Should show version 2.x or higher.

---

## SSH Key Setup

### Step 1: Generate SSH Keys (If Not Already Done)

Generate key for personal account:

```bash
ssh-keygen -t ed25519 -C "your-personal-email@example.com" -f ~/.ssh/id_personal
```

When prompted:

- **Enter passphrase:** (leave empty or set a passphrase)
- Creates two files:
  - `~/.ssh/id_personal` (private key)
  - `~/.ssh/id_personal.pub` (public key)

Generate key for work account:

```bash
ssh-keygen -t ed25519 -C "your-work-email@example.com" -f ~/.ssh/id_work
```

When prompted:

- **Enter passphrase:** (leave empty or set a passphrase)
- Creates two files:
  - `~/.ssh/id_work` (private key)
  - `~/.ssh/id_work.pub` (public key)

### Step 2: Add Public Keys to GitHub

**For Personal Account:**

1. Display the public key:
   ```bash
   cat ~/.ssh/id_personal.pub
   ```
2. Copy the entire output
3. Go to GitHub → Settings → SSH and GPG keys → New SSH key
4. Paste the key and give it a name (e.g., "Personal Windows")
5. Click "Add SSH key"

**For Work Account:**

1. Display the public key:
   ```bash
   cat ~/.ssh/id_work.pub
   ```
2. Copy the entire output
3. Go to GitHub (work account) → Settings → SSH and GPG keys → New SSH key
4. Paste the key and give it a name (e.g., "Work Windows")
5. Click "Add SSH key"

---

## SSH Config File Setup

### Step 1: Create/Edit SSH Config File

Navigate to SSH directory:

```bash
cd ~/.ssh
```

Create or edit `config` file (no extension):

```bash
# Windows PowerShell
notepad config

# Or use nano/vim
nano config
```

### Step 2: Add Host Configurations

Paste this content into the `config` file:

```
# Personal GitHub
Host github-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_personal
    AddKeysToAgent yes
    IdentitiesOnly yes

# Work GitHub
Host github-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_work
    AddKeysToAgent yes
    IdentitiesOnly yes
```

### Step 3: Set Correct Permissions

```bash
# Windows PowerShell
icacls "$env:USERPROFILE\.ssh\config" /inheritance:r /grant:r "$env:USERNAME`:F"
```

---

## Repository Configuration

### Method 1: For Existing Repositories (RECOMMENDED)

#### For Personal Repository

Navigate to repository:

```bash
cd path/to/your/personal/repo
```

Check current remote:

```bash
git remote -v
```

Change remote URL to SSH:

```bash
git remote set-url origin git@github-personal:your-personal-username/repo-name.git
```

Verify the change:

```bash
git remote -v
```

Should show:

```
origin  git@github-personal:your-personal-username/repo-name.git (fetch)
origin  git@github-personal:your-personal-username/repo-name.git (push)
```

Set local git config for this repo:

```bash
git config user.name "Your Personal Name"
git config user.email "your-personal-email@example.com"
```

Verify:

```bash
git config user.name
git config user.email
```

#### For Work Repository

Navigate to repository:

```bash
cd path/to/your/work/repo
```

Change remote URL to SSH:

```bash
git remote set-url origin git@github-work:your-work-username/repo-name.git
```

Verify:

```bash
git remote -v
```

Should show:

```
origin  git@github-work:your-work-username/repo-name.git (fetch)
origin  git@github-work:your-work-username/repo-name.git (push)
```

Set local git config:

```bash
git config user.name "Your Work Name"
git config user.email "your-work-email@example.com"
```

Verify:

```bash
git config user.name
git config user.email
```

### Method 2: For New Repositories (Using Clone)

#### Clone Personal Repository

```bash
git clone git@github-personal:your-personal-username/repo-name.git
cd repo-name
git config user.name "Your Personal Name"
git config user.email "your-personal-email@example.com"
```

#### Clone Work Repository

```bash
git clone git@github-work:your-work-username/repo-name.git
cd repo-name
git config user.name "Your Work Name"
git config user.email "your-work-email@example.com"
```

---

## Verification Steps

### Test SSH Connection

Test personal account:

```bash
ssh -T git@github-personal
```

Expected output:

```
Hi your-personal-username! You've successfully authenticated, but GitHub does not provide shell access.
```

Test work account:

```bash
ssh -T git@github-work
```

Expected output:

```
Hi your-work-username! You've successfully authenticated, but GitHub does not provide shell access.
```

### Verify Git Configuration

Check global git config:

```bash
git config --global --list
```

Check local repo config:

```bash
git config --local --list
```

### Test Git Operations

Test push/pull in personal repo:

```bash
cd path/to/personal/repo
git status
git pull origin main
```

Test push/pull in work repo:

```bash
cd path/to/work/repo
git status
git pull origin main
```

---

## Troubleshooting

### Issue 1: "Permission Denied (publickey)" Error

**Cause:** SSH key not loaded or SSH config misconfigured

**Solution:**

```bash
# Start SSH agent
eval "$(ssh-agent -s)"

# Add keys manually
ssh-add ~/.ssh/id_personal
ssh-add ~/.ssh/id_work

# Verify keys are loaded
ssh-add -l
```

### Issue 2: "Please Sign In" Dialog Appears

**Cause:** Repository is using HTTPS instead of SSH

**Solution:**

```bash
# Check current remote
git remote -v

# If shows https://github.com/..., change it:
git remote set-url origin git@github-personal:username/repo.git
# or
git remote set-url origin git@github-work:username/repo.git

# Verify
git remote -v
```

### Issue 3: "Permission to username/repo.git denied"

**Cause:** Using wrong SSH key for account

**Solution:**

1. Verify SSH key is added to correct GitHub account
2. Verify SSH config points to correct IdentityFile
3. Test connection: `ssh -T git@github-personal`

### Issue 4: Multiple SSH Keys Causing Conflicts

**Solution:**
Add to SSH config:

```
IdentitiesOnly yes
```

This forces SSH to only use the specified key in IdentityFile.

---

## Quick Reference

### Directory & File Locations

| Component           | Location                               |
| ------------------- | -------------------------------------- |
| SSH Keys            | `~/.ssh/id_personal`, `~/.ssh/id_work` |
| SSH Config          | `~/.ssh/config`                        |
| Git Config (Global) | `~/.gitconfig`                         |
| Git Config (Local)  | `.git/config` in repo root             |

### Common Commands

| Task                | Command                                     |
| ------------------- | ------------------------------------------- |
| Check SSH config    | `cat ~/.ssh/config`                         |
| Test SSH connection | `ssh -T git@github-personal`                |
| Check git config    | `git config --list`                         |
| Change remote URL   | `git remote set-url origin [new-url]`       |
| View current remote | `git remote -v`                             |
| Set local username  | `git config user.name "Name"`               |
| Set local email     | `git config user.email "email@example.com"` |
| Start SSH agent     | `eval "$(ssh-agent -s)"`                    |
| Add SSH key         | `ssh-add ~/.ssh/id_personal`                |

### SSH URL Patterns

| Account  | Pattern                                 |
| -------- | --------------------------------------- |
| Personal | `git@github-personal:username/repo.git` |
| Work     | `git@github-work:username/repo.git`     |

---

## Workflow Summary

### When Setting Up a New Repository

1. **Clone using correct host:**

   ```bash
   git clone git@github-personal:username/repo.git  # or github-work
   ```

2. **Enter repo and configure user:**

   ```bash
   cd repo
   git config user.name "Name"
   git config user.email "email@example.com"
   ```

3. **Verify before first push:**

   ```bash
   git remote -v
   git config user.name
   ```

4. **Push/pull normally:**
   ```bash
   git push origin main
   git pull origin main
   ```

### When Converting Existing Repository

1. **Check current remote:**

   ```bash
   git remote -v
   ```

2. **Update to SSH:**

   ```bash
   git remote set-url origin git@github-personal:username/repo.git
   ```

3. **Set user config:**

   ```bash
   git config user.name "Name"
   git config user.email "email@example.com"
   ```

4. **Test:**
   ```bash
   git pull origin main
   ```

---

## Important Notes

⚠️ **Never commit SSH private keys** (id_personal, id_work)

✅ **Only share public keys** (id_personal.pub, id_work.pub)

✅ **Keep SSH config file** (~/.ssh/config) for future reference

✅ **Use IdentitiesOnly yes** to prevent key conflicts

✅ **Set user.name and user.email locally** in each repository

✅ **Test SSH connection** before pushing

---

## Additional Resources

- [GitHub SSH Documentation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
- [SSH Key Best Practices](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure)
- [Git Configuration Guide](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage)

---

**Need help?** Refer to the Troubleshooting section or test SSH connection with `ssh -T git@github-personal`
