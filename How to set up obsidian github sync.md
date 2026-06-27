## Android Obsidian Git Sync — Complete Setup Guide

### Prerequisites
- Obsidian installed on Android
- GitHub account with a repository created

---

### 1. Install Termux
Install from Google Play Store.

---

### 2. Install packages
```bash
pkg update && pkg upgrade
pkg install git openssh
```

---

### 3. Grant storage permission
```bash
termux-setup-storage
# Tap Allow when dialog appears
```

---

### 4. Generate SSH key
```bash
ssh-keygen -t ed25519 -C "android-zettelkasten"
# Press Enter for all prompts
```

---

### 5. Add SSH key to GitHub
```bash
cat ~/.ssh/id_ed25519.pub
# Copy the entire output
```
**GitHub → Settings → SSH and GPG keys → New SSH key** → paste and save.

---

### 6. Test SSH auth
```bash
ssh -T git@github.com
# Expected: Hi drsarutobi8! You've successfully authenticated...
```

---

### 7. Set git identity
```bash
git config --global user.email "your@email.com"
git config --global user.name "Your Name"
```

---

### 8. Clone repo into vault location
```bash
# Clone to temp location first
git clone https://github.com/yourusername/zettelkasten.git ~/ztk-tmp

# Move into Documents
cd ~/storage/shared/Documents/zettelkasten
rm -rf .git
mv ~/ztk-tmp/.git .
rm -rf ~/ztk-tmp

# Fix ownership warning
git config --global --add safe.directory /storage/emulated/0/Documents/zettelkasten

# Sync to repo state
git reset --hard HEAD
git checkout README.md
```

---

### 9. Keep remote as HTTPS (required for plugin)
```bash
git remote -v
# Should show https://github.com/... — leave it as is
# If it shows SSH, switch back:
git remote set-url origin https://github.com/yourusername/zettelkasten.git
```

---

### 10. Create `.gitignore`
```bash
cat > .gitignore << 'EOF'
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.obsidian/cache
.trash/
.obsidian/plugins/obsidian-git/main.js
.obsidian/plugins/obsidian-git/styles.css
.obsidian/plugins/obsidian-git/data.json
EOF
```

---

### 11. Commit and push via Termux (first time)
```bash
git add -A
git commit -m "init: gitignore setup"
git push origin main
# Enter GitHub username and password/PAT when prompted
```

---

### 12. Generate GitHub Personal Access Token (PAT)
The plugin uses HTTPS, so it needs a PAT — not SSH.

**GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token**
- Scope: tick **`repo`** only
- Copy the token immediately (you won't see it again)

---

### 13. Open vault in Obsidian

- Tap vault icon → **Manage vaults** → **Open folder as vault**
- Navigate to `Documents/zettelkasten`

---

### 14. Install Obsidian Git plugin
**Settings → Community plugins → Browse** → search **Obsidian Git** → Install → Enable

---

### 15. Configure Obsidian Git plugin

**Authentication:**
| Field | Value |
|---|---|
| Username | your GitHub username |
| Password/PAT | paste token from Step 12 |
| Author name | Your Name |
| Author email | your@email.com |

**Automatic sync:**
| Setting | Value |
|---|---|
| Auto commit interval | `10` min |
| Auto push interval | `10` min |
| Pull on startup | ✅ ON |
| Auto commit after stopping file edits | ✅ ON |
| Push on commit-and-sync | ✅ ON |
| Pull on commit-and-sync | ✅ ON |

---

### 16. Test the plugin
In Obsidian → Source Control panel → tap the **push button** (upload icon).

Should succeed silently. If it errors, recheck the PAT in settings.

---

