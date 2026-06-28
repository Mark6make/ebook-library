# Ebook Library — Personal Cross-Machine Sync Repository

> **Purpose:** Store all personal ebooks in one Git repository, synced across Machine A (Mac mini), Machine B, and Machine C via branch-based push + main merge workflow.

## Directory Structure

```
_books/
  <lang>/           # Language: en, zh, ja, etc.
    <author-slug>/  # Author in lowercase-kebab-case
      <book-slug>/   # One directory per book
        <files>      # Actual ebook files (EPUB preferred, PDF fallback)
      00-complete-<name>/  # For multi-volume complete collections
_meta/
  sync-log.md       # Sync history log
.github/
  workflows/
    sync-notify.yml  # CI: logs push metadata to sync-log.md
README.md           # This file
```

## Sync Mechanism

### Machine A (Mac mini) — Initial Uploader
1. Download books locally
2. Place files under `_books/<lang>/<author>/<book-slug>/`
3. Push to branch: `upload/<yyyymmdd>-<description>`
4. Open PR → merge to `main`

### Machine B / C — Pullers
```bash
# One-time clone (on each machine)
git clone https://github.com/Mark6make/ebook-library.git ~/ebooks

# Pull latest
cd ~/ebooks && git checkout main && git pull origin main
```

### Machine B / C — Uploaders (when they add new books)
```bash
cd ~/ebooks
git checkout -b upload/<yyyymmdd>-<description>
# ... add files ...
git add . && git commit -m "feat: add ..."
git push -u origin HEAD
# Open PR manually or use: gh pr create --fill
```

## Format Priority
1. **EPUB** — preferred (readable, small)
2. **PDF** — fallback when no EPUB available
3. **AZW3 / MOBI** — only if both EPUB and PDF unavailable

## Naming Convention
- Filename: `<Lang>_Book<N>_<ShortTitle>_<Author>_<Year>.<ext>`
- Example: `EN_Book1_The_Road_Less_Traveled_Peck_2012.epub`
- Example: `ZH_Book5_不一样的鼓声_Peck_2020.epub`
- Chinese files may use Chinese characters in filename

## Adding a New Author / Language
1. Create directory: `_books/<lang>/<author-slug>/`
2. Add metadata to `_books/README.md`
3. Commit and push via standard workflow

## CI Workflow
The `sync-notify.yml` workflow runs on every push to `sync/*` or `upload/*` branches:
- Extracts machine ID, timestamp, and file list from commit message
- Updates `_meta/sync-log.md`
- Merges to main automatically (for branches matching `sync/*`)

## Contact
- Repo: https://github.com/Mark6make/ebook-library
- Managed by: Hermes Agent (macOS)
