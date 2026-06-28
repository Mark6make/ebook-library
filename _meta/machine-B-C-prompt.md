# Ebook Library Sync — Machine B & C Prompts

This folder holds the sync prompts for Machine B (MacBook) and Machine C.
The actual prompts are tailored per machine based on storage constraints.

---

## Machine B (MacBook) — External Drive Edition

> MacBook 机身硬盘空间小，ebooks 保存在外部移动硬盘上。
> 目录名称统一为 `eBooks`，但挂载点需要自动探测或由用户指定。

---

```
请帮我完成电子书库同步工作（ebook-library）。

## 背景

我的三台电脑（A/Mac mini、B/MacBook、C）共用一个 GitHub 仓库来同步电子书：
- 仓库地址：https://github.com/Mark6make/ebook-library
- 本地同步目录：`<外部硬盘挂载点>/eBooks`

## 特别说明（Machine B）

你的硬盘空间有限，所以 ebooks 保存在外部移动硬盘上。
请先探测外部硬盘的挂载路径：
```bash
# 方法1：查找 /Volumes/ 下除了 Macintosh HD 以外的目录（通常是移动硬盘）
ls /Volumes/
# 通常移动硬盘会显示为：My Passport、Elements、External、Backup 等
# 找到后， ebooks 目录就是 <挂载点>/eBooks

# 方法2：如果你知道挂载点，直接告诉我路径，例如：
# /Volumes/My\ Passport/eBooks
```

**如果找不到外部硬盘**，请先告诉我，我会手动告诉你挂载路径。

---

## 你的任务

### 第一次设置（仅需做一次）

1. **确认 eBooks 目录位置**
   ```bash
   # 查看所有外部卷标
   ls /Volumes/
   # 假设移动硬盘叫 "My Passport"，则ebooks路径为：
   # /Volumes/My\ Passport/eBooks
   ```

2. **克隆仓库到 eBooks 目录**
   ```bash
   mkdir -p "<你的eBooks路径>"
   git clone https://github.com/Mark6make/ebook-library.git "<你的eBooks路径>"
   cd "<你的eBooks路径>"
   ```
   把 `<你的eBooks路径>` 替换成实际路径，例如：
   `/Volumes/My Passport/eBooks`

3. **确认仓库结构正常**
   ```bash
   find "<你的eBooks路径>/_books" -name "*.epub" | sort
   ```

4. **检查 GitHub 认证状态**
   ```bash
   gh auth status
   ```
   如果未登录，请告诉我，我需要给你一个 GitHub Personal Access Token。

---

### 日常同步流程（每次上传新书时）

1. **拉取最新版本**
   ```bash
   cd "<你的eBooks路径>"
   git checkout main && git pull origin main
   ```

2. **创建上传分支**
   ```bash
   cd "<你的eBooks路径>"
   git checkout -b upload/YYYYMMDD-书籍简介
   ```
   例如：`git checkout -b upload/20260628-new-book`

3. **将电子书复制到对应目录后提交推送**
   - 英文书：`_books/en/<作者-slug>/<书名-slug>/`
   - 中文书：`_books/zh/<作者-slug>/<书名-slug>/`
   ```bash
   git add _books/
   git commit -m "feat: add <书名> by <作者>"
   git push -u origin HEAD
   ```

4. **合并到 main**（GitHub 网页或 gh CLI）

---

### 重要说明

- **格式优先级：** EPUB > PDF > AZW3/MOBI
- **文件名规范：**
  - 英文：`EN_Book<N>_<书名>_<作者>_<年份>.<扩展名>`
  - 中文：`ZH_Book<N>_<书名>_<作者>_<年份>.<扩展名>`
- **目录结构：** `_books/<语言>/<作者-slug>/<书名-slug>/`
- **同步日志：** 每次推送后，更新 `_meta/sync-log.md`

请先执行第一步（探测外部硬盘挂载点），确认 eBooks 目录位置。
```

---

## Machine C — Internal Drive Edition

> 和 Machine A 一样，使用本地内部存储。
> 目录统一为 `~/ebooks`。

---

```
请帮我完成电子书库同步工作（ebook-library）。

## 背景

我的三台电脑（A/Mac mini、B/MacBook、C）共用一个 GitHub 仓库来同步电子书：
- 仓库地址：https://github.com/Mark6make/ebook-library
- 本地同步目录：~/ebooks（C 电脑有充足本地空间，直接放用户目录）

---

## 你的任务

### 第一次设置（仅需做一次）

1. **克隆仓库到 ~/ebooks**
   ```bash
   git clone https://github.com/Mark6make/ebook-library.git ~/ebooks
   cd ~/ebooks
   ```

2. **确认仓库结构正常**
   ```bash
   find ~/ebooks/_books -name "*.epub" | sort
   ```

3. **检查 GitHub 认证状态**
   ```bash
   gh auth status
   ```
   如果未登录，请告诉我，我需要给你一个 GitHub Personal Access Token。

---

### 日常同步流程（每次上传新书时）

1. **拉取最新版本**
   ```bash
   cd ~/ebooks && git checkout main && git pull origin main
   ```

2. **创建上传分支**
   ```bash
   cd ~/ebooks
   git checkout -b upload/YYYYMMDD-书籍简介
   ```

3. **将电子书复制到对应目录后提交推送**
   - 英文书：`_books/en/<作者-slug>/<书名-slug>/`
   - 中文书：`_books/zh/<作者-slug>/<书名-slug>/`
   ```bash
   git add _books/
   git commit -m "feat: add <书名> by <作者>"
   git push -u origin HEAD
   ```

4. **合并到 main**（GitHub 网页或 gh CLI）

---

### 重要说明

- **格式优先级：** EPUB > PDF > AZW3/MOBI
- **文件名规范：**
  - 英文：`EN_Book<N>_<书名>_<作者>_<年份>.<扩展名>`
  - 中文：`ZH_Book<N>_<书名>_<作者>_<年份>.<扩展名>`
- **目录结构：** `_books/<语言>/<作者-slug>/<书名-slug>/`
- **同步日志：** 每次推送后，更新 `_meta/sync-log.md`

请先执行第一步，确认仓库克隆成功。
```
