# AI 智慧筆記摘要器（AI Note Summarizer）

Python 課程作業：輸入多行文章，由本機 Ollama 的 `qwen2.5:3b` 模型產生繁體中文三個重點與一句話總結。使用 requests 呼叫 API，逐行解析 JSON Lines 串流並即時顯示文字。

## 環境與安裝

需要 Python 3.9 以上、已安裝的 uv 與 Ollama。所有指令都請在此專案目錄執行（Windows PowerShell）：

```powershell
cd C:\Users\kid40\ai-note-summarizer
uv --version
uv add requests
uv sync
```

本專案已提供 `pyproject.toml`，不需要再次執行 `uv init`。全新空目錄才需要先執行 `uv init --bare --name ai-note-summarizer`。

確認 Ollama 已啟動，模型未安裝時執行：

```powershell
ollama pull qwen2.5:3b
```

若 Ollama 尚未啟動，開啟 Ollama 應用程式或在另一個終端執行 `ollama serve`。已啟動時不必重複啟動。

## 操作方式

```powershell
uv run python main.py
```

輸入或貼上多行文章，最後單獨輸入 `/end` 並按 Enter：

```text
Python 是一種程式語言，可用於資料分析和自動化。
本課程介紹變數、迴圈與函式，並以實作專案練習。
/end
```

程式會逐段顯示摘要，完成後結束。按 `Ctrl+C` 可取消。首次載入模型可能較慢；模型產生的摘要仍應自行核對原文。

## 常見問題

- 找不到 `uv`：確認已安裝的 uv 所在資料夾已加入 PATH，重新開啟終端，或使用 uv.exe 的完整路徑；不需要重複安裝。
- 無法連線：確認 Ollama 正在執行，且 `http://localhost:11434` 可連線。
- 找不到模型：執行 `ollama pull qwen2.5:3b`，可用 `ollama list` 檢查。
- 逾時：確認記憶體與模型載入狀態，再重試。連線逾時為 10 秒，等待串流資料的逾時為 180 秒。
- JSON 格式錯誤、HTTP 錯誤、串流中斷或空白輸入：程式會顯示原因，失敗時結束碼為 1。

## 檔案

- `main.py`：輸入、API 呼叫、串流解析與錯誤處理。
- `pyproject.toml`：專案資訊與 requests 依賴。
- `.gitignore`：排除虛擬環境、私密設定與快取。
- `uv.lock`：成功執行 uv 後產生，建議保留以重現依賴環境。

文章會傳給本機 Ollama，程式不會儲存文章或摘要。

## 本次驗證紀錄

已通過 Python 語法檢查，並使用現有 Python 與 requests 2.32.3 實際呼叫本機 `qwen2.5:3b`，成功解析串流與顯示摘要。

本次執行環境找不到 `uv` 指令，嘗試 `uv init`、`uv add requests`、`uv sync` 與 `uv run python main.py` 均無法執行。因此 `pyproject.toml` 為手動建立，尚未產生 `.venv` 與 `uv.lock`，也未重新安裝 uv。請在能執行已安裝 uv 的終端依上方步驟完成環境同步。
