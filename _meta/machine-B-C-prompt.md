# Ebook Library Transfer Hub — Machine B & C Prompts

This repository is a **transfer hub / central library**, not a mandatory full mirror.
Each machine may selectively upload or selectively download books. Local `eBooks` folders on A/B/C may be different.

---

## Machine B (MacBook) — External Drive + Selective Download Edition

> Machine B is a MacBook with limited internal disk. Store ebooks on an external drive in a folder named `eBooks`.
> Do not assume Machine B should clone/download every book.

```
请帮我完成电子书库中转仓库（ebook-library）的设置和按需使用。

## 背景

我的三台电脑（A/Mac mini、B/MacBook、C）共用一个 GitHub 仓库来传递/保存电子书：
- 仓库地址：https://github.com/Mark6make/ebook-library
- 注意：这不是要求三台电脑的本地文件完全一致。GitHub 是中心库/中转库。
- 任意电脑都可以选择性上传书籍，也可以选择性从远程下载某些书。
- Machine B 是 MacBook，内置硬盘小，电子书应放在外部移动硬盘的 `eBooks` 文件夹里。

## 先做路径确认

请先探测外部硬盘挂载点：
```bash
ls /Volumes/
```

然后确定本机电子书目录：
```bash
# 示例，实际路径请根据 /Volumes/ 输出调整
LOCAL_EBOOKS_PATH="/Volumes/<外部硬盘名称>/eBooks"
```

如果无法判断外部硬盘路径，请先停止并问我：`Machine B 的外部硬盘路径是什么？`

---

## 你可以根据我要做的事情选择流程

### A. 我只想下载某几本书（推荐：sparse checkout）

请不要默认全量克隆所有书。优先使用 sparse checkout：

```bash
LOCAL_EBOOKS_PATH="/Volumes/<外部硬盘名称>/eBooks"

git clone --filter=blob:none --no-checkout https://github.com/Mark6make/ebook-library.git "$LOCAL_EBOOKS_PATH"
cd "$LOCAL_EBOOKS_PATH"
git sparse-checkout init --cone

# 先只拉元数据，不下载全部大文件
git sparse-checkout set README.md _meta _books/README.md
git checkout main
```

如果我指定要下载某本书，再追加对应目录，例如：
```bash
cd "$LOCAL_EBOOKS_PATH"
git sparse-checkout add _books/en/peck-m-scott/01-the-road-less-traveled
git pull origin main
```

如果我只要单个文件，也可以直接用 raw.githubusercontent.com 下载，不一定需要 git clone。

---

### B. 我要从 Machine B 上传新书

```bash
cd "$LOCAL_EBOOKS_PATH"
git fetch origin
git checkout main
git pull origin main

git checkout -b upload/$(date +%Y%m%d)-<书籍简介>
mkdir -p _books/<语言>/<作者-slug>/<书名-slug>/
cp "<新下载的电子书文件>" _books/<语言>/<作者-slug>/<书名-slug>/

# 如有必要，更新 _books/README.md 和 _meta/sync-log.md
git add _books _meta README.md
git commit -m "feat: add <书名> by <作者>"
git push -u origin HEAD
```

然后通过 GitHub 网页合并 PR，或者用：
```bash
gh pr create --fill --base main
gh pr merge --squash --delete-branch
```

---

### C. 我想全量下载（仅当我明确要求）

只有当我明确说“全量克隆/下载全部电子书”时，才执行：
```bash
LOCAL_EBOOKS_PATH="/Volumes/<外部硬盘名称>/eBooks"
git clone https://github.com/Mark6make/ebook-library.git "$LOCAL_EBOOKS_PATH"
```

---

## 重要规则

- 不要假设本地 eBooks 应该和远程完全一样。
- 判断“有没有某本书”时，要区分：本地没有，还是远程也没有。
- 格式优先级：EPUB > PDF > AZW3/MOBI。
- 有 EPUB 就用 EPUB；没有 EPUB 才接受 PDF。
- 目录结构：`_books/<语言>/<作者-slug>/<书名-slug>/`
- 文件名规范：`EN_Book<N>_<书名>_<作者>_<年份>.<ext>` 或 `ZH_Book<N>_<书名>_<作者>_<年份>.<ext>`。

请先完成：探测外部硬盘路径，并告诉我建议的 `LOCAL_EBOOKS_PATH`。
```

---

## Machine C — Flexible Local Path + Selective Download Edition

> Machine C may use `~/ebooks` by default, but it also does not need to mirror everything.

```
请帮我完成电子书库中转仓库（ebook-library）的设置和按需使用。

## 背景

我的三台电脑（A/Mac mini、B/MacBook、C）共用一个 GitHub 仓库来传递/保存电子书：
- 仓库地址：https://github.com/Mark6make/ebook-library
- 注意：这不是要求三台电脑的本地文件完全一致。GitHub 是中心库/中转库。
- 任意电脑都可以选择性上传书籍，也可以选择性从远程下载某些书。
- Machine C 默认可以使用 `~/ebooks`，除非我指定其他目录。

请先设置：
```bash
LOCAL_EBOOKS_PATH="$HOME/ebooks"
```

---

## 你可以根据我要做的事情选择流程

### A. 我只想下载某几本书（推荐：sparse checkout）

不要默认全量克隆所有书。优先使用 sparse checkout：

```bash
LOCAL_EBOOKS_PATH="$HOME/ebooks"

git clone --filter=blob:none --no-checkout https://github.com/Mark6make/ebook-library.git "$LOCAL_EBOOKS_PATH"
cd "$LOCAL_EBOOKS_PATH"
git sparse-checkout init --cone

# 先只拉元数据
git sparse-checkout set README.md _meta _books/README.md
git checkout main
```

如果我指定要下载某本书，再追加对应目录，例如：
```bash
cd "$LOCAL_EBOOKS_PATH"
git sparse-checkout add _books/zh/peck-m-scott/00-complete-1-8
git pull origin main
```

如果我只要单个文件，也可以直接用 raw.githubusercontent.com 下载。

---

### B. 我要从 Machine C 上传新书

```bash
cd "$LOCAL_EBOOKS_PATH"
git fetch origin
git checkout main
git pull origin main

git checkout -b upload/$(date +%Y%m%d)-<书籍简介>
mkdir -p _books/<语言>/<作者-slug>/<书名-slug>/
cp "<新下载的电子书文件>" _books/<语言>/<作者-slug>/<书名-slug>/

git add _books _meta README.md
git commit -m "feat: add <书名> by <作者>"
git push -u origin HEAD
```

然后通过 GitHub 网页合并 PR，或者用：
```bash
gh pr create --fill --base main
gh pr merge --squash --delete-branch
```

---

### C. 我想全量下载（仅当我明确要求）

只有当我明确说“全量克隆/下载全部电子书”时，才执行：
```bash
git clone https://github.com/Mark6make/ebook-library.git "$HOME/ebooks"
```

---

## 重要规则

- 不要假设本地 eBooks 应该和远程完全一样。
- 判断“有没有某本书”时，要区分：本地没有，还是远程也没有。
- 格式优先级：EPUB > PDF > AZW3/MOBI。
- 有 EPUB 就用 EPUB；没有 EPUB 才接受 PDF。
- 目录结构：`_books/<语言>/<作者-slug>/<书名-slug>/`
- 文件名规范：`EN_Book<N>_<书名>_<作者>_<年份>.<ext>` 或 `ZH_Book<N>_<书名>_<作者>_<年份>.<ext>`。

请先告诉我你准备采用哪种模式：只拉元数据、按需下载某本书，还是全量克隆。
```
