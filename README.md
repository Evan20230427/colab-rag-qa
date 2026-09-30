# 🤖 LLM + RAG 本地知識問答系統（Google Colab 版）

改寫自 YouTube 教學，將原本需要在本地安裝的 RAG 知識問答系統，封裝為可在 **Google Colab** 上一鍵執行的操作腳本。

## 📖 技術架構

| 元件 | 工具 | 說明 |
|------|------|------|
| **嵌入模型** | `mxbai-embed-large` | 將文字轉為向量，用於語義搜尋 |
| **對話 LLM** | `ycchen/breeze-7b-instruct-v1_0` | 聯發科繁體中文優化語言模型 |
| **推論引擎** | Ollama | 本地 LLM 執行環境 |
| **向量資料庫** | ChromaDB | 儲存並查詢知識庫嵌入向量 |
| **Web UI** | Streamlit | 互動式問答介面 |
| **對外隧道** | pyngrok | 將 Colab 內部埠號公開為 HTTPS URL |

## 🚀 快速開始

1. 開啟 [RAG_QA_Colab.ipynb](./RAG_QA_Colab.ipynb) 並上傳至 Google Colab
2. 設定 Runtime 為 **T4 GPU**（Runtime → Change runtime type → T4 GPU）
3. 在 **Cell 2** 填入您的 Ngrok authtoken（[免費申請](https://ngrok.com)）
4. **依序執行所有 Cell**（Cell 1 → Cell 9）

## 📦 Notebook 結構

| Cell | 功能 | 說明 |
|------|------|------|
| Cell 1 | 📖 說明文件 | Markdown 說明（架構、前置需求、預計等待時間） |
| Cell 2 | ⚙️ 使用者設定 | Ngrok token、Drive 整合、模型選擇（唯一需修改的 Cell） |
| Cell 3 | 📦 系統套件 | 安裝 pciutils、curl |
| Cell 4 | 🦙 Ollama 安裝 | 安裝 Ollama 並以背景執行緒啟動服務，附就緒健康檢查 |
| Cell 5 | 🔽 模型下載 | 下載 mxbai-embed-large + Breeze-7B（約 5~15 分鐘） |
| Cell 6 | 📦 Python 套件 | 安裝 streamlit / pyngrok / chromadb / ollama |
| Cell 7 | 📊 知識庫準備 | 掛載 Drive 或自動產生 10 筆範例 QA 知識庫 |
| Cell 8 | ✍️ 產生 app.py | 寫出完整的 Streamlit 應用程式 |
| Cell 9 | 🚀 啟動服務 | 啟動 Streamlit 背景程序 + 建立 Ngrok HTTPS 隧道 |

## ⚙️ 改版說明（本地 vs Colab）

| 項目 | 原始版本（本地）| Colab 版本 |
|------|---------------|------------|
| Ollama 啟動 | 手動安裝並執行 | 自動安裝 + `subprocess.Popen` 背景執行 |
| UI 存取 | `localhost:8501` | pyngrok 隧道 → HTTPS 公開 URL |
| 資料庫持久化 | 本地磁碟 | Google Drive（選配）|
| QA 檔案 | 手動放置 | Drive 讀取或自動產生範例 |
| 進度顯示 | 無 | `st.progress()` 進度條 |
| 參考來源 | 無 | 可展開查看 Top-3 知識片段 |

## 📋 使用前需準備

- Google 帳號（用於 Colab 和 Drive）
- [Ngrok 免費帳號](https://ngrok.com)（取得 authtoken）

## ⚠️ 注意事項

- Google Colab 免費版 Session 最長約 12 小時
- 斷線後需重新執行 Cell 4 ~ Cell 9（若有 Drive 持久化，可跳過知識庫重建）
- 模型總大小約 5GB，首次下載需要較長時間
