# Multiple GitHub Account Setup with SSH

Manage two GitHub accounts (personal & work) on the same machine using SSH authentication without sign-in dialogs.

---

## 📋 Quick Overview

This project provides guides and configurations for setting up **secure SSH-based authentication** for multiple GitHub accounts on Windows. Using SSH eliminates annoying sign-in popups and allows seamless switching between accounts.

### Key Features

✅ **No more sign-in dialogs** — SSH keys handle authentication automatically  
✅ **Separate SSH keys** — One key per GitHub account for security  
✅ **Easy account switching** — Use host aliases (`github-personal`, `github-work`)  
✅ **Per-repository configuration** — Set different user names/emails for each account  
✅ **Comprehensive guides** — Step-by-step setup and troubleshooting docs

---

## 🚀 Quick Start (5 Minutes)

### 1. Generate SSH Keys

```bash
# Personal account
ssh-keygen -t ed25519 -C "personal@example.com" -f ~/.ssh/id_personal

# Work account
ssh-keygen -t ed25519 -C "work@example.com" -f ~/.ssh/id_work
```

### 2. Add Keys to GitHub

- Go to GitHub → Settings → SSH and GPG keys
- Add `~/.ssh/id_personal.pub` and `~/.ssh/id_work.pub` to respective accounts

### 3. Create SSH Config File

Create `~/.ssh/config` (no extension):

```
Host github-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_personal
    IdentitiesOnly yes

Host github-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_work
    IdentitiesOnly yes
```

### 4. Update Repository URLs

```bash
# For personal repos
git remote set-url origin git@github-personal:username/repo.git

# For work repos
git remote set-url origin git@github-work:username/repo.git
```

### 5. Test Connection

```bash
ssh -T git@github-personal  # Should show your personal username
ssh -T git@github-work      # Should show your work username
```

✅ Done! No more sign-in popups when pushing/pulling.

---

## 📚 Documentation

### 1. **[COMPREHENSIVE_GIT_SSH_GUIDE.md](./COMPREHENSIVE_GIT_SSH_GUIDE.md)** — The Complete Reference

**Use this if:**

- You want detailed setup instructions with explanations
- You're troubleshooting issues
- You need to understand the full configuration

**Covers:**

- SSH key generation (Ed25519 keys recommended)
- SSH config file setup with detailed explanations
- Repository configuration for existing and new repos
- Verification steps to test everything works
- 8 common troubleshooting scenarios
- Complete command reference

**Time commitment:** 20–30 minutes for complete setup

---

### 2. **[GIT_SSH_MIGRATION.md](./GIT_SSH_MIGRATION.md)** — Convert HTTPS to SSH

**Use this if:**

- You already have repositories cloned with HTTPS
- You're getting "Please sign in" dialogs
- You want to quickly switch a single repo to SSH

**Covers:**

- Why HTTPS causes sign-in popups
- How to convert a repository from HTTPS → SSH
- Setting local git identity per repository
- Before/after examples

**Time commitment:** 5 minutes per repository

---

### 3. **[GIT_USER_CONFIGURATION.md](./GIT_USER_CONFIGURATION.md)** — Set Username & Email Per Repo

**Use this if:**
- You want to remove your global git configuration
- You need to set different usernames/emails for different accounts
- You want to verify your git identity before committing
- You're getting commits under the wrong email

**Covers:**
- Removing global user.name and user.email
- Setting local configuration per repository
- Verification commands and checklist
- Complete workflow examples
- Troubleshooting wrong author issues
- Quick reference commands

**Time commitment:** 10 minutes for setup, 1 minute per repo verification

---

## 🔧 Common Tasks

| Task                | Command                                                       |
| ------------------- | ------------------------------------------------------------- |
| Check SSH config    | `cat ~/.ssh/config`                                           |
| Test personal SSH   | `ssh -T git@github-personal`                                  |
| Test work SSH       | `ssh -T git@github-work`                                      |
| View repo remote    | `git remote -v`                                               |
| Change to SSH       | `git remote set-url origin git@github-personal:user/repo.git` |
| Set local git name  | `git config user.name "Your Name"`                            |
| Set local git email | `git config user.email "you@example.com"`                     |
| Verify local config | `git config --local --list`                                   |

---

## ⚠️ Important Security Notes

- **Never commit SSH private keys** (`id_personal`, `id_work`) to version control
- **Only share public keys** (`id_personal.pub`, `id_work.pub`)
- **Keep SSH keys protected** — they grant access to your GitHub accounts
- **Use strong passphrases** when generating keys (or leave empty for automation)
- **Regenerate keys annually** or if compromised

---

## 🆘 Troubleshooting

### Issue: "Permission Denied (publickey)"

```bash
# Start SSH agent and add keys
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_personal
ssh-add ~/.ssh/id_work

# Verify keys are loaded
ssh-add -l
```

### Issue: Still Getting Sign-In Dialog

```bash
# Check if repo is still using HTTPS
git remote -v

# If it shows https://, switch to SSH:
git remote set-url origin git@github-personal:username/repo.git
```

### Issue: "Permission to username/repo.git denied"

This usually means the SSH key isn't added to your GitHub account. Go to:

- GitHub → Settings → SSH and GPG keys → Check if the key is listed

For more troubleshooting, see [COMPREHENSIVE_GIT_SSH_GUIDE.md](./COMPREHENSIVE_GIT_SSH_GUIDE.md#troubleshooting).

---

## 📖 Workflow Examples

### Setting Up a New Repository

```bash
# Clone with correct host alias
git clone git@github-personal:username/repo.git
cd repo

# Configure user for this repo
git config user.name "Your Name"
git config user.email "personal@example.com"

# Verify and push
git remote -v
git push origin main
```

### Converting an Existing HTTPS Repository

```bash
cd your-existing-repo

# Check current URL
git remote -v

# Switch to SSH
git remote set-url origin git@github-work:username/repo.git

# Set user config
git config user.name "Your Name"
git config user.email "work@example.com"

# Test
git pull origin main
```

---

## 📍 File Locations (Quick Reference)

| Item              | Location                |
| ----------------- | ----------------------- |
| Personal SSH key  | `~/.ssh/id_personal`    |
| Work SSH key      | `~/.ssh/id_work`        |
| SSH config file   | `~/.ssh/config`         |
| Global git config | `~/.gitconfig`          |
| Local repo config | `your-repo/.git/config` |

---

## 🔗 External Resources

- [GitHub SSH Documentation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
- [SSH Key Best Practices](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure)
- [Git Configuration Guide](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage)

---

## 💡 Tips

- **Use descriptive SSH key names** in GitHub (e.g., "Personal Windows", "Work Laptop")
- **Set IdentitiesOnly yes** in SSH config to prevent key conflicts
- **Always verify** `git config --local --list` in a repo before committing
- **Test SSH connection** with `ssh -T git@github-personal` before troubleshooting
- **Keep SSH config backed up** — it's your key reference for setup

---

## 📝 Next Steps

1. **New to this?** → Start with the [Quick Start](#-quick-start-5-minutes) section above
2. **Need details?** → Read [COMPREHENSIVE_GIT_SSH_GUIDE.md](./COMPREHENSIVE_GIT_SSH_GUIDE.md)
3. **Already using HTTPS?** → Follow [GIT_SSH_MIGRATION.md](./GIT_SSH_MIGRATION.md)
4. **Stuck?** → Check the [Troubleshooting](#-troubleshooting) section

---

**Last Updated:** June 4, 2026  
**Status:** ✅ Ready to use
