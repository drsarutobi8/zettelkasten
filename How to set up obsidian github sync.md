## Android Obsidian Git Sync — Complete Setup

### 1. Install Termux
Install from F-Droid (not Play Store — Play Store version is outdated).

### 2. Install packages
```bash
pkg update && pkg upgrade
pkg install git openssh
```

### 3. Grant storage permission
```bash
termux-setup-storage
# Tap Allow when dialog appears
```

### 4. Generate SSH key
```bash
ssh-keygen -t ed25519 -C "android-zettelkasten"
# Press Enter for all prompts
```

### 5. Add key to GitHub
```bash
cat ~/.ssh/id_ed25519.pub
# Copy the output
```
Go to **GitHub → Settings → SSH and GPG keys → New SSH key**, paste and save.

### 6. Test SSH auth
```bash
ssh -T git@github.com
# Should say: Hi drsarutobi8! You've successfully authenticated...
```

### 7. Set git identity
```bash
git config --global user.email "drsarutobi@gmail.com"
git config --global user.name "Apichat Banyatsupasil"
```

### 8. Clone repo into vault location
```bash
git clone https://github.com/drsarutobi8/zettelkasten.git ~/ztk-tmp
cd ~/storage/shared/Documents/zettelkasten
rm -rf .git
mv ~/ztk-tmp/.git .
rm -rf ~/ztk-tmp
git config --global --add safe.directory /storage/emulated/0/Documents/zettelkasten
git reset --hard HEAD
git checkout README.md
```

### 9. Switch remote to SSH
```bash
git remote set-url origin git@github.com:drsarutobi8/zettelkasten.git
```

### 10. Create `.gitignore`
```bash
cat > .gitignore << 'EOF'
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.obsidian/cache
.trash/
.obsidian/plugins/obsidian-git/main.js
.obsidian/plugins/obsidian-git/styles.css
EOF
```

### 11. Commit and push
```bash
git add -A
git commit -m "init: gitignore and first note"
git push origin main
```

### 12. Open vault in Obsidian
- Vault icon → **Manage vaults** → **Open folder as vault**
- Navigate to `Documents/zettelkasten`

### 13. Install Obsidian Git plugin
Settings → Community plugins → Browse → search **Obsidian Git** → Install → Enable

### 14. Configure Obsidian Git plugin
| Setting | Value |
|---|---|
| Auto commit interval | `10` min |
| Auto push interval | `10` min |
| Pull on startup | ✅ ON |
| Auto commit after stopping file edits | ✅ ON |
| Push on commit-and-sync | ✅ ON |
| Pull on commit-and-sync | ✅ ON |

---
Done. Obsidian will now auto-commit and push every 10 minutes, and pull from GitHub on every startup.