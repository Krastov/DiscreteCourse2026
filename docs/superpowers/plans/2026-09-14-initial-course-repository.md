# Initial Course Repository Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Publish an initial repository for **离散数学与结构（I）** with an unchanged Loom snapshot under `main_notes/` and a clearly separated personal-note area under `contribution/`.

**Architecture:** Import every tracked file from the pinned Loom source commit as ordinary files under `main_notes/`, without nested Git metadata. Add short Chinese documentation at the repository root and in `contribution/`, then verify the copy, repository boundaries, documentation rules, and pushed branch.

**Tech Stack:** Git, Markdown, POSIX shell utilities

---

### Task 1: Import the pinned Loom snapshot

**Files:**
- Create: `main_notes/**` from the tracked tree at source commit `290fa4042d33a7dd11f290eb39fba7f5d5b71369`

- [ ] **Step 1: Confirm the source commit and clean target path**

Run:

```bash
git -C ../loom-inspect rev-parse HEAD
test ! -e main_notes
```

Expected: the first command prints `290fa4042d33a7dd11f290eb39fba7f5d5b71369`; the second exits successfully with no output.

- [ ] **Step 2: Export the tracked source tree into `main_notes/`**

Run:

```bash
git -C ../loom-inspect archive --format=tar --prefix=main_notes/ 290fa4042d33a7dd11f290eb39fba7f5d5b71369 | tar -xf - -C .
```

Expected: `main_notes/README.md`, `main_notes/loom.cls`, `main_notes/template/`, `main_notes/examples/`, `main_notes/overleaf/`, and the remaining tracked source files are created. No `main_notes/.git` path exists.

- [ ] **Step 3: Verify the imported file list and contents**

Run:

```bash
plan_tmp=$(mktemp -d)
git -C ../loom-inspect ls-tree -r --name-only 290fa4042d33a7dd11f290eb39fba7f5d5b71369 | LC_ALL=C sort > "$plan_tmp/source-tree.txt"
find main_notes \( -type f -o -type l \) | sed 's#^main_notes/##' | LC_ALL=C sort > "$plan_tmp/target-tree.txt"
diff -u "$plan_tmp/source-tree.txt" "$plan_tmp/target-tree.txt"
diff -qr -x .git ../loom-inspect main_notes
test ! -e main_notes/.git
```

Expected: the file-list and complete bytewise comparisons produce no diff; the nested-Git check exits successfully.

- [ ] **Step 4: Stage and commit the source snapshot**

Run:

```bash
git add main_notes
git commit -m "feat: import Loom notes foundation"
```

Expected: one commit containing the complete `main_notes/` snapshot.

### Task 2: Add course and contribution documentation

**Files:**
- Create: `README.md`
- Create: `contribution/README.md`

- [ ] **Step 1: Create the root README**

Create `README.md` with exactly:

```markdown
# 离散数学与结构（I）

本仓库用于整理与协作维护「离散数学与结构（I）」课程笔记。

## 目录说明

- [`main_notes/`](main_notes/)：课程主笔记，初始模板与资料来自 [Loom](https://github.com/Polaris-Aeterna/loom-notes)。个人笔记请勿放入此目录。
- [`contribution/`](contribution/)：同学们各自维护个人笔记的区域。提交前请阅读其中的 [README](contribution/README.md)。

请将个人笔记、图片和相关附件统一放入自己在 `contribution/` 下创建的子文件夹中。
```

- [ ] **Step 2: Create the contribution README**

Create `contribution/README.md` with exactly:

```markdown
# 个人笔记贡献区

这里用于收集「离散数学与结构（I）」课程的个人笔记。

## 提交方式

1. 每位同学请在 `contribution/` 下创建一个属于自己的子文件夹，建议使用姓名或 GitHub 用户名命名，例如 `contribution/PhotonYan/`。
2. 将自己的笔记、图片及相关附件全部放在该子文件夹内。
3. 不要把个人文件直接堆放在 `contribution/` 根目录。
4. **不要把个人笔记放入 `main_notes/`。** `main_notes/` 用于课程主笔记。

请只维护自己的子文件夹；如需修改其他同学的内容，请先与对方沟通。
```

- [ ] **Step 3: Verify the documentation rules**

Run:

```bash
rg -n "离散数学与结构（I）|main_notes|contribution" README.md contribution/README.md
rg -n '不要把个人笔记放入 `main_notes/`' contribution/README.md
```

Expected: both files contain the course name and directory guidance; the explicit prohibition appears in `contribution/README.md`.

- [ ] **Step 4: Commit the documentation**

Run:

```bash
git add README.md contribution/README.md
git commit -m "docs: add course contribution guidance"
```

Expected: one commit containing the root and contribution documentation.

### Task 3: Verify and publish the repository

**Files:**
- Verify: all tracked repository files

- [ ] **Step 1: Check repository integrity and scope**

Run:

```bash
git status --short
git diff --check HEAD~2..HEAD
test "$(git ls-files main_notes | wc -l | tr -d ' ')" = "$(git -C ../loom-inspect ls-files | wc -l | tr -d ' ')"
test ! -e main_notes/.git
test -f README.md
test -f contribution/README.md
```

Expected: the worktree is clean, whitespace validation passes, tracked source and target file counts match, and required repository boundaries and README files exist.

- [ ] **Step 2: Push the `main` branch**

Run:

```bash
git push -u origin main
```

Expected: Git reports the new `main` branch was created on `github.com:PhotonYan/DiscreteCourse2026.git`.

- [ ] **Step 3: Verify the remote branch and content**

Run:

```bash
git ls-remote origin refs/heads/main
git rev-parse HEAD
```

Expected: both commands print the same commit hash, proving the verified local state is the published `main` branch.
