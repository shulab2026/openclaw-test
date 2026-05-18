# 🔧 GitHub Skill 完整安裝與設定手冊

## 📋 概述

本文檔涵蓋如何完整設定 GitHub CLI (`gh`) 與 SSH 金鑰，以支援 OpenClaw 的 GitHub Skill 功能。

---

## 🎯 前置條件

1. OpenClaw 環境已安裝
2. GitHub 帳號已建立
3. 終端機可訪問（bash）

---

## 📦 第一部分：GitHub Skill 安裝

### 步驟 1：確認 GitHub Skill 狀態

```bash
# 列出已安裝的技能
clawhub list | grep github

# 如果看到 github，表示已安裝
```

### 步驟 2：重新安裝 GitHub Skill（可選）

```bash
# 使用 clawhub 重新安裝
clawhub install github

# 或指定倉庫安裝
clawhub install shulab2026/openclaw-skill-github
```

### 步驟 3：驗證安裝

```bash
# 確認 skill 已安裝
clawhub list | grep github

# 測試基本功能
gh --version
```

---

## 🔑 第二部分：SSH 金鑰產生與設定

### 步驟 1：檢查現有金鑰

```bash
# 檢查 ~/.ssh 目錄
ls -la ~/.ssh/

# 查看現有金鑰
ls -la ~/.ssh/id_*

# 如果看到 id_ed25519、id_rsa 等，表示已有金鑰
```

### 步驟 2：產生新金鑰

```bash
# 使用 ed25519（最安全）
ssh-keygen -t ed25519 -C "shulab@github.com" -f ~/.ssh/id_ed25519_github

# 或直接使用現有金鑰
ssh-keygen -t rsa -b 4096 -C "shulab@github.com" -f ~/.ssh/id_rsa_github
```

**提示**：
- `-C`：添加註解，方便識別
- `-f`：指定文件名
- **Enter**：使用預設路徑
- **密碼**：可選（建議輸入以保護金鑰）

### 步驟 3：啟動 SSH Agent 並加載金鑰

```bash
# 啟動 SSH Agent
eval "$(ssh-agent -s)"
# 輸出：Agent pid 12345

# 將金鑰加入 Agent
ssh-add ~/.ssh/id_ed25519_github

# 確認已加載
ssh-add -l
# 輸出：123456789012 SHA256:xxx shulab@github.com (ED25519)
```

### 步驟 4：設定 SSH 配置文件

```bash
# 創建 ~/.ssh/config 文件
cat > ~/.ssh/config << 'EOF'
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_github
  IdentitiesOnly yes
  StrictHostKeyChecking no
EOF

# 設定權限（重要！）
chmod 600 ~/.ssh/config
```

### 步驟 5：取得公鑰內容

```bash
# 顯示公鑰
cat ~/.ssh/id_ed25519_github.pub
```

**輸出範例**：
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILkbqdYzSuPSqrWvGDkWod3ytiNYZn2oZntqGOA/YHq5 shulab@github.com
```

### 步驟 6：將公鑰加入 GitHub

1. **複製公鑰**：
   ```bash
   # Linux: 使用 xclip
   xclip -selection clipboard ~/.ssh/id_ed25519_github.pub
   
   # 或手動複製上述公鑰內容
   ```

2. **開啟 GitHub 設定**：
   - 網址：https://github.com/settings/keys
   - 點擊 **"New SSH key"** 或 **"Add SSH key"**

3. **貼上金鑰**：
   - **Title**: `shulab-github`（或其他易記名稱）
   - **Type**: `Authentication key`
   - **Key**: 貼上複製的完整公鑰內容
   - 點擊 **"Add SSH key"**

4. **確認**：
   - 輸入 GitHub 密碼
   - 看到成功訊息："Your SSH key has been added"

### 步驟 7：測試 SSH 連線

```bash
# 測試 SSH 連線
ssh -T git@github.com
```

**成功回應**：
```
Hi shulab2026! You've successfully authenticated, but GitHub does not provide shell access.
```

**失敗處理**：
```bash
# 如果出現 Host key verification failed
ssh -vT git@github.com  # 查看詳細錯誤
```

---

## 🔑 第三部分：GitHub Token 設定（用於 gh CLI）

### 步驟 1：取得個人存取權杖（PAT）

1. 開啟 https://github.com/settings/tokens
2. 點擊 **"Tokens (classic)"** → **"Generate new token"**
3. 選擇 **Note**：輸入描述（如 `shulab-openclaw`）
4. **Expiration**：設定有效期限（建議 30-90 天）
5. **Select scopes**：勾選所需權限
   - `repo`（完全控制私人倉庫）
   - `read:org`（讀取組織資訊，可選）
6. 點擊 **"Generate token"**
7. **立即複製 token**（格式：`ghp_xxxxx` 或 `github_pat_xxxxx`）

### 步驟 2：設定環境變數

```bash
# 臨時設定（當前終端機有效）
export GH_TOKEN=***

# 永久設定（加入 ~/.bashrc 或 ~/.bash_profile）
echo 'export GH_TOKEN=***' >> ~/.bashrc
source ~/.bashrc
```

### 步驟 3：測試 gh CLI

```bash
# 確認登入狀態
gh auth status

# 成功回應：
# github.com
#   ✓ Logged in to github.com account shulab2026 (GH_TOKEN)
```

---

## 🧪 第四部分：GitHub Skill 功能測試

### 測試清單

```bash
# 1. 基本認證測試
gh auth status

# 2. 倉庫資訊查詢
gh repo view shulab2026/openclaw-test --json name,owner,updatedAt

# 3. 列出倉庫
gh repo list shulab2026

# 4. Git 操作測試
cd /home/shulab/.openclaw/workspace/shulab2026/openclaw-test
git remote -v
git status

# 5. Issue 查詢（如果倉庫有 issue）
gh issue list shulab2026/openclaw-test

# 6. PR 查詢（如果倉庫有 PR）
gh pr list shulab2026/openclaw-test
```

---

## 📁 第五部分：環境持久化設定

### 設定 ~/.bashrc 終始化腳本

```bash
cat >> ~/.bashrc << 'EOF'

# GitHub Skill 環境設定
# SSH Agent 自動啟動
if [ -z "$SSH_AUTH_SOCK" ]; then
    eval "$(ssh-agent -s)"
    ssh-add ~/.ssh/id_ed25519_github 2>/dev/null
fi

# GitHub Token（請替換為您的 token）
# export GH_TOKEN=***

# Git 全局設定
git config --global user.email "shulab@github.com"
git config --global user.name "shulab2026"
EOF

# 重新載入
source ~/.bashrc
```

---

## ⚠️ 常見問題與解決方案

### Q1: SSH 連線失敗 - Host key verification failed

```bash
# 方案 1：從 known_hosts 移除並重新加入
ssh-keygen -R github.com
ssh -T git@github.com

# 方案 2：設定 StrictHostKeyChecking
echo "Host github.com" >> ~/.ssh/config
echo "  StrictHostKeyChecking accept-new" >> ~/.ssh/config
```

### Q2: gh 命令提示未登入

```bash
# 檢查環境變數
echo $GH_TOKEN

# 重新設定
export GH_TOKEN=***
gh auth status
```

### Q3: SSH Agent 未運行

```bash
# 啟動 Agent
eval "$(ssh-agent -s)"

# 加載金鑰
ssh-add ~/.ssh/id_ed25519_github
```

### Q4: 金鑰權限錯誤

```bash
# 設定正確權限
chmod 700 ~/.ssh
chmod 600 ~/.ssh/config
chmod 600 ~/.ssh/id_ed25519_github
chmod 644 ~/.ssh/id_ed25519_github.pub
```

### Q5: GitHub CLI 未安裝

```bash
# 使用 clawhub 安裝
clawhub install github

# 或手動安裝
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update && sudo apt install gh
```

---

## 📋 快速檢查清單

- [ ] SSH 金鑰已產生並加載
- [ ] ~/.ssh/config 已設定
- [ ] 公鑰已加入 GitHub Settings
- [ ] SSH 連線測試成功（`ssh -T git@github.com`）
- [ ] GitHub Token 已產生並設定
- [ ] `gh auth status` 顯示已登入
- [ ] `gh repo view` 能讀取倉庫
- [ ] Git 操作（push/pull）能正常運行
- [ ] ~/.bashrc 已包含環境持久化設定

---

## 🎯 結語

完成以上步驟後，您的 GitHub Skill 已完全設定並可正常運作。您可以：

- 使用 `gh` 命令查詢倉庫、issue、PR 等資訊
- 使用 `git` 命令進行版本控制操作
- 在 OpenClaw 環境中完整使用 GitHub 相關功能

---

## 📚 相關資源

- GitHub CLI 官方文件：https://cli.github.com/manual/
- SSH 金鑰設定：https://docs.github.com/en/authentication/connecting-to-github-with-ssh
- 個人存取權杖設定：https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens
- OpenClaw GitHub Skill 文件：https://docs.openclaw.ai/skills/github

---

**版本**: 1.0  
**最後更新**: 2026-05-18  
**作者**: Shulab Claw
