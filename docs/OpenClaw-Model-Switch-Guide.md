# OpenClaw 模型切換與新增指南

## 📋 目錄
1. [檢查當前模型](#1-檢查當前模型)
2. [切換預設模型](#2-切換預設模型)
3. [新增模型到可用名單](#3-新增模型到可用名單)
4. [TUI 顯示的模型切換](#4-tui-顯示的模型切換)
5. [常用命令速查](#5-常用命令速查)

---

## 1. 檢查當前模型

### 查看當前預設模型
```bash
openclaw models status
```

**輸出範例：**
```
Default: ollama/gemma4
Configured models (2): ollama/qwen3.5:35b, ollama/gemma4
```

### 查看所有已配置的模型
```bash
openclaw models list
```

**輸出範例：**
```
Model                                      Input      Ctx         Local Auth  Tags
ollama/gemma4                              text       125k        no    yes   default
ollama/qwen3.5:35b                         text+image 256k        no    yes   configured
```

### 查看 agent 可用的模型清單
```bash
cat ~/.openclaw/agents/main/agent/models.json | jq '.providers.ollama.models[].id'
```

---

## 2. 切換預設模型

### 切換到特定模型
```bash
openclaw models set <模型 ID>
```

**範例：**
```bash
openclaw models set ollama/gemma4
```

**成功輸出：**
```
Config overwrite: /home/shulab/.openclaw/openclaw.json
Updated ~/.openclaw/openclaw.json
Default model: ollama/gemma4
```

### 確認切換成功
```bash
openclaw models status | grep "Default"
```

---

## 3. 新增模型到可用名單

### 方法一：使用 Ollama CLI（推薦）

1. **確認 Ollama 模型已拉取**
   ```bash
   ollama list
   ollama pull <模型名稱>
   ```

2. **在 OpenClaw 中掃描可用模型**
   ```bash
   openclaw models scan
   ```

3. **驗證模型已加入**
   ```bash
   openclaw models list
   ```

### 方法二：手動編輯 models.json

1. **編輯模型配置檔案**
   ```bash
   nano ~/.openclaw/agents/main/agent/models.json
   ```

2. **在對應 provider 的 models 陣列中加入新模型**

   **Ollama 範例：**
   ```json
   {
     "id": "gemma4:e4b",
     "name": "gemma4:e4b",
     "reasoning": false,
     "input": ["text"],
     "cost": {
       "input": 0,
       "output": 0,
       "cacheRead": 0,
       "cacheWrite": 0
     },
     "contextWindow": 128000,
     "maxTokens": 8192,
     "compat": {
       "supportsUsageInStreaming": true
     },
     "api": "ollama"
   }
   ```

3. **重新載入配置**
   ```bash
   openclaw gateway restart
   ```

### 方法三：使用 models set-alias 建立別名

```bash
openclaw models aliases set <別名> <模型 ID>
```

**範例：**
```bash
openclaw models aliases set fast ollama/gemma4
```

---

## 4. TUI 顯示的模型切換

### 問題說明
TUI（Terminal UI）顯示的模型資訊來自於**當前 session 的啟動配置**，而不是預設模型設定。即使修改了預設模型，已開啟的 TUI session 仍會顯示初始啟動時的模型。

### 解決方法

#### 方法一：在 TUI 中執行命令切換
```bash
# 在 TUI 視窗中執行
openclaw agent --model ollama/gemma4 --message "切換模型測試" --deliver
```

#### 方法二：重新啟動 TUI session
1. **關閉當前 TUI**
   - 按 `Ctrl+C` 或在 TUI 中執行 exit/quit 命令

2. **重新開啟 TUI**
   ```bash
   openclaw tui --local
   ```

3. **確認模型已切換**
   ```bash
   openclaw models status
   ```

#### 方法三：使用特定模型開啟 agent
```bash
openclaw agent --model ollama/gemma4 --message "你好" --deliver
```

### 快速切換腳本
```bash
#!/bin/bash
# switch-model.sh - 快速切換並重啟 TUI

MODEL=$1
if [ -z "$MODEL" ]; then
    echo "用法：./switch-model.sh <模型 ID>"
    echo "範例：./switch-model.sh ollama/gemma4"
    exit 1
fi

# 1. 設為預設模型
openclaw models set "$MODEL"

# 2. 確認
openclaw models status | grep "Default"

echo "✅ 預設模型已切換為：$MODEL"
echo "請重新啟動 TUI 以套用變更"
```

---

## 5. 常用命令速查

### 模型管理命令
```bash
# 查看模型狀態
openclaw models status

# 列出所有模型
openclaw models list

# 掃描可用模型
openclaw models scan

# 設定預設模型
openclaw models set <模型 ID>

# 建立別名
openclaw models aliases set <別名> <模型 ID>

# 查看別名列表
openclaw models aliases list
```

### TUI 相關命令
```bash
# 開啟 TUI
openclaw tui --local

# 使用特定模型開啟 agent
openclaw agent --model <模型 ID> --message "訊息" --deliver

# 查看 session 狀態
session_status
```

### Gateway 管理
```bash
# 查看 Gateway 狀態
openclaw gateway status

# 重啟 Gateway
openclaw gateway restart

# 檢查 Gateway 服務
systemctl --user status openclaw-gateway
```

---

## 📊 常見問題

### Q: TUI 顯示的模型沒有更新？
**A:** 這是正常現象。TUI 顯示的是 session 啟動時的模型設定。需要重新啟動 TUI session 才能看到最新的預設模型。

### Q: 如何知道模型是否支援某些功能？
**A:** 使用 `openclaw models list` 查看各模型的 `Input` 欄位：
- `text` - 純文字模型
- `text+image` - 支援圖片輸入
- `text+image+video` - 支援多媒體輸入

### Q: 如何檢查模型是否已載入 Ollama？
**A:**
```bash
ollama list
ollama run <模型名稱>
```

---

## 🔗 相關文件
- [OpenClaw 官方文檔](https://docs.openclaw.ai/cli/models)
- [Ollama 文檔](https://ollama.com/docs)

---

*最後更新：2026-05-22*
