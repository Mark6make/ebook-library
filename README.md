# Ebook Library — GitHub Transfer Hub

> **Purpose:** Use GitHub as a central transfer/library hub for ebooks across Machine A (Mac mini), Machine B (MacBook), and Machine C.
>
> **Important:** The three computers do **not** need to keep identical local `eBooks` contents. Any machine may selectively upload books to the GitHub repo, or selectively download books from it. The remote repository is the shared source/transfer point; each local machine is a partial working copy chosen by need and disk space.

## Repository

- Remote: https://github.com/Mark6make/ebook-library
- Machine A current local path: `~/ebooks`
- Machine B local path: external drive path ending in `eBooks`, e.g. `/Volumes/<DriveName>/eBooks`
- Machine C local path: flexible; default can be `~/ebooks`

## Directory Structure

```
_books/
  <lang>/           # Language: en, zh, ja, etc.
    <author-slug>/  # Author in lowercase-kebab-case
      <book-slug>/  # One directory per book
        <files>     # Actual ebook files (EPUB preferred, PDF fallback)
      00-complete-<name>/  # For multi-volume complete collections
_meta/
  sync-log.md       # Transfer history log
  machine-B-C-prompt.md
README.md
```

## Core Operating Model

### 1) Selective download is allowed and expected
A machine may download:
- the whole repo, or
- only metadata and selected book directories, or
- a single file directly from GitHub.

This is especially important for Machine B (MacBook), whose internal disk is small.

### 2) Selective upload is allowed from any machine
Any machine can add books:
1. Pull or fetch the latest remote state.
2. Create an `upload/<date>-<description>` branch.
3. Add only the new/changed book files.
4. Commit and push.
5. Merge to `main` via PR or direct push if appropriate.

### 3) Do not assume local = remote
Before saying a book is “missing,” check whether it is missing locally or missing in the remote repository. Local folders may be incomplete by design.

## Recommended Download Methods

### Option A — Full clone (simple, if disk space is fine)

```bash
git clone https://github.com/Mark6make/ebook-library.git <LOCAL_EBOOKS_PATH>
cd <LOCAL_EBOOKS_PATH>
git pull origin main
```

### Option B — Sparse checkout (recommended for selective download)

Use this when you only want part of the repository.

```bash
# Choose local path first, e.g. ~/ebooks or /Volumes/<DriveName>/eBooks
LOCAL_EBOOKS_PATH="<LOCAL_EBOOKS_PATH>"

git clone --filter=blob:none --no-checkout https://github.com/Mark6make/ebook-library.git "$LOCAL_EBOOKS_PATH"
cd "$LOCAL_EBOOKS_PATH"
git sparse-checkout init --cone

# Always include metadata, plus only the book directories you need
git sparse-checkout set README.md _meta _books/README.md _books/en/peck-m-scott/01-the-road-less-traveled

git checkout main
```

To add another directory later:

```bash
cd "$LOCAL_EBOOKS_PATH"
git sparse-checkout add _books/zh/peck-m-scott/00-complete-1-8
git pull origin main
```

### Option C — Download one file directly via GitHub URL

Use when you only need one ebook and do not want a git working copy.

```bash
curl -L -o "<OUTPUT_FILE>" "https://raw.githubusercontent.com/Mark6make/ebook-library/main/<PATH_IN_REPO>"
```

Example:

```bash
curl -L -o "EN_Book1_The_Road_Less_Traveled_Peck_2012.epub" \
"https://raw.githubusercontent.com/Mark6make/ebook-library/main/_books/en/peck-m-scott/01-the-road-less-traveled/EN_Book1_The_Road_Less_Traveled_Peck_2012.epub"
```

## Upload Workflow from Any Machine

```bash
cd <LOCAL_EBOOKS_PATH>
git fetch origin
git checkout main
git pull origin main

git checkout -b upload/$(date +%Y%m%d)-<short-description>
mkdir -p _books/<lang>/<author-slug>/<book-slug>
cp <downloaded-book-file> _books/<lang>/<author-slug>/<book-slug>/

# Update _books/README.md and _meta/sync-log.md if needed
git add _books _meta README.md
git commit -m "feat: add <book-title> by <author>"
git push -u origin HEAD
```

Then merge via GitHub web UI or:

```bash
gh pr create --fill --base main
gh pr merge --squash --delete-branch
```

## Format Priority

1. **EPUB** — preferred (readable, small)
2. **PDF** — fallback when no EPUB exists
3. **AZW3 / MOBI** — only if both EPUB and PDF are unavailable

## Naming Convention

- Filename: `<Lang>_Book<N>_<ShortTitle>_<Author>_<Year>.<ext>`
- Example: `EN_Book1_The_Road_Less_Traveled_Peck_2012.epub`
- Example: `ZH_Book5_不一样的鼓声_Peck_2020.epub`
- Chinese filenames may use Chinese characters.

## Current Library Contents

See `_books/README.md` for book-level metadata.

## Contact

- Repo: https://github.com/Mark6make/ebook-library
- Managed by: Hermes Agent
