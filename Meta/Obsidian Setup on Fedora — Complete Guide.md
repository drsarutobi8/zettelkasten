
### 1. Install Obsidian

Download AppImage from [obsidian.md](https://obsidian.md/) or via Flatpak:

```bash
flatpak install flathub md.obsidian.Obsidian
```

### 2. Set up SSH key for GitHub

```bash
ssh-keygen -t ed25519 -C "fedora-zettelkasten"
# Press Enter for all prompts

cat ~/.ssh/id_ed25519.pub
# Copy output
```

**GitHub → Settings → SSH and GPG keys → New SSH key** → paste and save.

Test:

```bash
ssh -T git@github.com
# Expected: Hi drsarutobi8! You've successfully authenticated...
```

### 3. Clone the vault

```bash
git clone git@github.com:drsarutobi8/zettelkasten.git ~/projects/zettelkasten
```

If already cloned via HTTPS, switch to SSH:

```bash
cd ~/projects/zettelkasten
git remote set-url origin git@github.com:drsarutobi8/zettelkasten.git
```

### 4. Open vault in Obsidian

- Open Obsidian → **Open folder as vault**
- Select `~/projects/zettelkasten`

### 5. Install Obsidian Git plugin

**Settings → Community plugins → Browse** → search **Obsidian Git** → Install → Enable

### 6. Configure Obsidian Git plugin

|Setting|Value|
|---|---|
|Auto commit interval|`10` min|
|Auto push interval|`10` min|
|Pull on startup|✅ ON|
|Auto commit after stopping file edits|✅ ON|
|Push on commit-and-sync|✅ ON|
|Pull on commit-and-sync|✅ ON|

**No PAT needed** — plugin uses system git which uses your SSH key automatically.

### 7. Install Zotero Integration plugin

**Settings → Community plugins → Browse** → search **Zotero Integration** → Install → Enable

Requirements:

- Zotero desktop running with **Better BibTeX** plugin installed
- In plugin settings, set literature note path to `Literature/`

### 8. Test sync

Make a change → Source Control panel → push button, or wait for auto-commit timer.

---

**Key difference from Android:** Desktop uses system git + SSH. No PAT required. See also: [[How to set up obsidian github sync]]