# Machine B / C — Ebook Library Sync Prompt

> Copy the section below for **Machine B** and **Machine C** respectively.
> Replace `[MACHINE_ID]` with `macbook-B` or `desktop-C` (or whatever you named them).
> Paste the full section into a new Hermes conversation on that machine.

---

## 以下是给 Machine B（以及 Machine C）的 Prompt

---

```
请帮我完成电子书库同步工作（ebook-library）。

## 背景

我的三台电脑（A/Mac mini、B、C）共用一个 GitHub 仓库来同步电子书：
- **仓库地址：** https://github.com/Mark6make/ebook-library
- **本地同步目录：** ~/ebooks

## 你的任务

### 第一次设置（仅需做一次）

请帮我完成以下操作：

1. **克隆仓库到 ~/ebooks**
   ```bash
   git clone https://github.com/Mark6make/ebook-library.git ~/ebooks
   cd ~/ebooks
   ```

2. **确认仓库结构正常**
   ```bash
   find ~/ebooks/_books -name "*.epub" | sort
   ls ~/ebooks/_books/en/peck-m-scott/
   ```

3. **检查 GitHub 认证状态**
   ```bash
   gh auth status
   ```
   如果未登录，请告诉我，我需要给你一个 GitHub Personal Access Token。

---

### 日常同步流程（每次有新书要上传时）

1. **拉取最新版本**
   ```bash
   cd ~/ebooks
   git checkout main && git pull origin main
   ```

2. **创建上传分支**
   ```bash
   git checkout -b upload/YYYYMMDD-书籍简介
   ```
   例如：`git checkout -b upload/20260628-peck-series`

3. **将下载好的电子书复制到对应目录**
   - 英文书放到：`_books/en/<作者名>/<书名-slug>/`
   - 中文书放到：`_books/zh/<作者名>/<书名-slug>/`

4. **提交并推送**
   ```bash
   git add _books/
   git commit -m "feat: add <书名> by <作者>"
   git push -u origin HEAD
   ```

5. **合并到 main（通过 GitHub 网页或 gh CLI）**
   - 如果用 GitHub 网页：打开仓库 → Pull Requests → New Pull Request → 选择你的分支 → 合并到 main
   - 如果用 gh：
     ```bash
     gh pr create --fill --base main
     gh pr merge --squash --delete-branch
     ```

---

### 重要说明

- **文件名规范：**
  - 英文：`EN_Book<N>_<书名简称>_<作者>_<年份>.<扩展名>`
  - 中文：`ZH_Book<N>_<书名>_<作者>_<年份>.<扩展名>`
  - 示例：`EN_Book1_The_Road_Less_Traveled_Peck_2012.epub`

- **格式优先级：** EPUB > PDF > AZW3/MOBI
  - 有 EPUB 就用 EPUB
  - 没有 EPUB 才用 PDF

- **同步日志：** 每次推送后，更新 `_meta/sync-log.md`，在末尾追加一行：
  ```
  | 日期 | 电脑编号 | 书籍 | 状态 |
  |------|---------|------|------|
  | YYYY-MM-DD | B 或 C | <书籍信息> | pushed to main |
  ```

- **获取已有书籍：** 想把书下载到本地时，直接从 ~/ebooks 目录中复制即可。

---

### 格式规范说明

请严格按照以下目录结构存放书籍：
```
_books/
  <语言代码>/
    <作者名（英文小写横线分隔）>/
      <书名-slug（英文小写横线分隔）>/
        书籍文件.epub（或其他格式）
```

例如：
```
_books/en/peck-m-scott/01-the-road-less-traveled/EN_Book1_The_Road_Less_Traveled_Peck_2012.epub
_books/zh/peck-m-scott/00-complete-1-8/少有人走的路_全集1-8_斯科特派克_2020.epub
```

---

请先执行第一次设置的步骤，确认仓库克隆成功，然后告诉我结果。
```
