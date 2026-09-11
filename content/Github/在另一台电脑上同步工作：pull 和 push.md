# 在另一台电脑上同步工作：pull 和 push

> 适用场景：你已经把项目上传到自己的 GitHub fork，现在换一台电脑继续工作。
> 核心规则：**开工前先 pull，收工前先 push**，两台机器才不会各跑各的。
---
## 零、新电脑一次性准备（每台新电脑只做一次）

### 0.1 SSH over 443（如有代理拦截 SSH）

```bash
cat >> ~/.ssh/config << 'EOF'

Host github.com
  Hostname ssh.github.com
  Port 443
  User git
EOF

# 测试连通
ssh -T git@github.com
# 看到 "Hi <用户名>! You've successfully authenticated..." 即成功
```

> 如果这台新电脑还没有 SSH key，需先生成并添加到 GitHub：
> ```bash
> ssh-keygen -t ed25519 -C "你的邮箱"      # 一路回车
> cat ~/.ssh/id_ed25519.pub                 # 复制输出
> ```
> 然后 GitHub → Settings → SSH and GPG keys → New SSH key → 粘贴。

### 0.2 配置 git 身份（如果没配过）

```bash
git config --global user.name  "你的名字"
git config --global user.email "你的邮箱"
```

---

## 一、首次：把项目克隆到新电脑

```bash
# 克隆你自己的 fork
git clone git@github.com:<你的用户名>/PyElastica.git
cd PyElastica

# 接上 upstream（如果以后要在这台机器上也拉原作者更新）
git remote add upstream git@github.com:GazzolaLab/PyElastica.git

# 确认
git remote -v
```

SophT 同理：

```bash
git clone git@github.com:<你的用户名>/SophT.git
cd SophT
git remote add upstream git@github.com:SophT-Team/SophT.git
git remote -v
```

> 大文件 output 不会被克隆下来（因为 `.gitignore` 已经把它们挡在仓库外）。
> 这些 output 是各台机器**本地各自生成**的，本来就不该在机器间同步。

---

## 二、日常循环：每次开工与收工

### 开工前 —— 先拉取最新（必做）

```bash
git pull origin main
```

这会把你在**另一台电脑**上推送的最新改动同步下来。
养成习惯：**坐下来第一件事就是 pull**，否则容易在旧版本上改，导致后面冲突。

### 干活 —— 正常修改文件

### 收工前 —— 提交并推送（必做）

```bash
# 1. 看看改了什么
git status

# 2.（可选但推荐）验证没有大文件混入
git add .
git diff --cached --name-only | xargs -I{} find {} -size +1M 2>/dev/null
# 理想输出为空白

# 3. 提交
git commit -m "今天做了什么的简要说明"
# 若被 pre-commit 钩子卡住：git commit --no-verify -m "..."

# 4. 推送
git push origin main
```

---

## 三、常见情况与处理

### 情况 A：push 报 `rejected` / `non-fast-forward`

原因：另一台电脑推送过新东西，远程比你本地新。
**不要用 `--force`**，先拉取合并：

```bash
git pull origin main      # 拉取并合并远程的新提交
# 若有冲突，解决后：
git add <冲突文件>
git commit
git push origin main
```

### 情况 B：pull 时出现 merge conflict（冲突）

两台电脑改了同一个文件的同一处时会发生。文件里会出现：

```
<<<<<<< HEAD
这台电脑的内容
=======
远程（另一台电脑）的内容
>>>>>>> origin/main
```

手动编辑保留想要的部分，删掉 `<<<<<<<`、`=======`、`>>>>>>>` 标记，然后：

```bash
git add <冲突文件>
git commit
git push origin main
```

### 情况 C：本地有还没提交的改动，但想先 pull

如果 pull 时报 "Your local changes would be overwritten"，先把当前改动临时存起来：

```bash
git stash          # 暂存当前改动
git pull origin main
git stash pop      # 恢复改动（可能需解决冲突）
```

---

## 四、黄金法则

1. **开工先 pull，收工先 push** —— 雷打不动。
2. **一台电脑收工前一定 push**，否则另一台 pull 不到，等于白做。
3. **大文件 output 不进 git**，靠本地各自重新生成。
4. 冲突不可怕，按 `<<<<<<<` 标记手动选择即可；拿不准就把冲突内容贴出来问。

---

## 五、最小命令速查

| 时机 | 命令 |
|------|------|
| 开工 | `git pull origin main` |
| 收工 | `git add .` → `git commit -m "..."` → `git push origin main` |
| 被钩子卡住 | `git commit --no-verify -m "..."` |
| push 被拒 | `git pull origin main`（解决冲突）→ `git push origin main` |
| 先存改动再 pull | `git stash` → `git pull` → `git stash pop` |

