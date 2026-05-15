# 📰 開源 AI 專案日誌 - 2026-05-15

> 來源：GitHub Blog 最新分析 (April 30, 2025 / Updated May 1, 2025)  
> 主題：**從 MCP 到多代理協作 - 10 大開源 AI 專案**

---

## 🎯 三大核心趨勢

### 1️⃣ MCP (Model Context Protocol) 成為 AI 整合新標準

**關鍵洞察**：MCP 正在成為 AI 與外部工具整合的「通用協議」

| 項目 | 描述 |
|------|------|
| **Open WebUI MCP** | 將 MCP 工具轉換為 OpenAPI HTTP 服務器，簡化 AI 應用集成 |
| **F/mcptools** | Fatih Kadir Akin (GitHub Star) 開發的 MCP CLI 工具 |
| **Blender-MCP** | 透過 MCP 連接 Blender 與 Claude，自然語言控制 3D 創作 |

**學習重點**：
- MCP 如何作為「通用工具端口」連接 LLM 與桌面應用
- 理解 MCP 協議的標準化趨勢 (Abigail Cabunoc 指出整合痛點)
- 實踐：從 Open WebUI MCP 源碼學習代理架構

---

### 2️⃣ 多代理協作 (Multi-Agent Collaboration)

**關鍵洞察**：單一 AI 代理不足，多代理協同是未來方向

| 項目 | 亮點 |
|------|------|
| **OWL** | CAMEL-AI 框架，GAIA 基準測試 58.18 分 (開源頂尖) |
| **架構** | 多代理透過瀏覽器、終端、函數調用協同工作 |

**學習重點**：
- 代理角色分工設計 (Kevin Crosby: 「人對人、代理對代理」)
- 理解代理協作模式而非單一代理優化
- 實踐：運行 OWL 示例，設計自己的代理協作場景

---

### 3️⃣ AI 代理可移植性標準

**關鍵洞察**：跨框架遷移需求增加，Letta 提供解決方案

| 項目 | 功能 |
|------|------|
| **Letta** | 開源 `.af` 文件格式，版本控制 AI 代理及記憶狀態 |
| **優勢** | 跨框架 (MemGPT/Letta, LangGraph, CrewAI) 無縫遷移 |
| **背景** | 源自 MemGPT 的 serialization 層 |

**學習重點**：
- `.af` 文件格式結構
- 代理狀態保存與恢復機制
- 實踐：測試 Letta 與不同框架的互操作性

---

## 🛠️ 其他重要項目 (6-10)

### 6️⃣ VoiceStar
- **語言**: Python
- **功能**: 精確時長控制的 TTS
- **場景**: 定時音頻 (播客、配音)、固定長度提示
- **介面**: CLI + Gradio, 含預訓練模型

### 7️⃣ Second-Me
- **語言**: Python
- **功能**: AI 數字分身
- **應用**: 學習您的思維/溝通風格、管理 LinkedIn/Airbnb 帳號
- **風險**: 需謹慎使用個人數據

### 8️⃣ CSM (Conversational Speech Model)
- **語言**: Python
- **架構**: Llama 架構 + Residual Vector Quantization
- **優勢**: 開源 TTS 替代方案，語音自然度高
- **注意**: 模型使用有濫用限制 (Apache 2.0 with restrictions)

### 9️⃣ self.so
- **語言**: TypeScript
- **功能**: AI 生成個人網站 (上傳 CV/LinkedIn → 自動生成)
- **技術棧**: Together.ai, Vercel AI SDK, Next.js, Helicone
- **場景**: 快速建立專業個人頁面

### 🔟 Unbody
- **語言**: TypeScript
- **定位**: 「AI 的 Supabase」
- **四層架構**:
  1. **Perception**: 數據接入、解析、向量化
  2. **Memory**: 向量數據庫 + 持久化存儲
  3. **Reasoning**: 內容生成、函數調用、任務規劃
  4. **Action**: API/SDK 暴露知識、觸發器
- **價值**: 快速搭建 AI 原生應用後端

---

## 💡 對學習的建議

### 立即行動 (本周)
1. **研究 MCP 協議**
   - 閱讀 Open WebUI MCP 源碼
   - 嘗試 F/mcptools CLI
   - 設計自己的 MCP 工具 (連接本地 API)

2. **體驗多代理**
   - 克隆 OWL 倉庫並運行示例
   - 理解 CAMEL-AI 框架的代理設計模式
   - 設計簡單的 2-3 代理協作場景

### 深入探索 (本月)
1. **Letta 可移植性**
   - 了解 `.af` 文件格式
   - 測試跨框架遷移 (LangGraph ↔ Letta)
   - 設計自己的代理序列化格式

2. **Unbody 後端框架**
   - 實踐四層架構 (Perception/Memory/Reasoning/Action)
   - 搭建簡單的 AI 應用 Demo
   - 比較與其他後端方案 (FastAPI + LangChain)

### 長期追蹤
- **MCP 生態**: 關注更多 MCP 工具 (Blender、Unity、Unreal 等)
- **多代理研究**: 跟踪 GAIA 基準測試開源模型排名
- **語音技術**: VoiceStar、CSM 的應用場景探索

---

## 📚 參考資源

| 類型 | 連結 |
|------|------|
| 原始文章 | [GitHub Blog: From MCP to multi-agents](https://github.blog/open-source/maintainers/from-mcp-to-multi-agents-the-top-10-open-source-ai-projects-on-github-right-now-and-why-they-matter/) |
| OWL | https://github.com/camel-ai/owl |
| Letta | https://github.com/letta-ai |
| Open WebUI MCP | https://github.com/open-webui |
| F/mcptools | https://github.com/f |

---

## 🔖 標籤

`MCP` `多代理` `AI agents` `開源` `語音合成` `後端框架` `可移植性` `2025-05-15`

---

*最後更新: 2026-05-15*  
*下一條日誌: 追蹤今日項目後續動態*
