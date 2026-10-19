# AI 智慧筆記摘要器（AI Note Summarizer）

Python 課程作業：輸入多行文章，由本機 Ollama 的 `qwen2.5:3b` 模型產生繁體中文三個重點與一句話總結。使用 requests 呼叫 API，逐行解析 JSON Lines 串流並即時顯示文字。

## 環境與安裝

需要 Python 3.9 以上、已安裝的 uv 與 Ollama。所有指令都請在此專案目錄執行（Windows PowerShell）：

```powershell
cd C:\Users\kid40\ai-note-summarizer
uv --version
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
- `uv.lock`：已產生的依賴鎖定檔，保留以重現依賴環境。
- `site/index.html`：繁體中文作品展示網站，含介紹、執行步驟、成果與 AI 使用說明。
- `site/style.css`：網站樣式與手機、電腦版面。

文章會傳給本機 Ollama，程式不會儲存文章或摘要。

## 執行狀態與驗證紀錄

已通過 Python 語法檢查，並使用現有 Python 與 requests 2.32.3 實際呼叫本機 `qwen2.5:3b`，成功解析串流與顯示摘要。

**最新狀態：作者已確認成功透過 `uv run python main.py` 執行摘要程式**，搭配 Python、uv 與本機 Ollama `qwen2.5:3b`。專案目前已有 `.venv` 與 `uv.lock`。先前「uv 無法使用、尚未產生環境與鎖定檔」的紀錄是舊環境狀態，已不代表目前專案狀態。

本次網站製作未重新執行模型測試。專案未保留完整的實測輸入與輸出，因此網站成果區明確標示待補，沒有捏造摘要。補齊時請將同一次測試的完整原文、AI 產生的三個重點、一句話總結、測試日期、終端紀錄或截圖，以及核對結果填入 `site/index.html`，並更新待補狀態。上方操作方式中的文章只是輸入範例，不是已保存的測試結果。

## 展示網站與預覽

以瀏覽器直接開啟 `site/index.html` 即可預覽；也可以在專案根目錄執行：

```powershell
python -m http.server 8000 --directory site --bind 127.0.0.1
```

接著開啟 <http://127.0.0.1:8000>，按 `Ctrl+C` 停止伺服器。網站僅使用靜態 HTML/CSS，不需要安裝前端套件、啟動 Ollama 或連線本機摘要 API。

此目錄可作為 GitHub Pages 的靜態內容。本階段尚未設定部署，也未新增 GitHub Actions。注意 GitHub Pages 的分支發布來源僅支援根目錄或 `/docs`，不能直接選 `/site`；未來若採分支發布，可另行將 `site/` 內容放到發布分支根目錄，或依需求規劃部署方式。本次先保留指定的 `site/` 結構。

## AI 使用說明

- Codex：協助撰寫程式、錯誤處理、展示網站與文件。
- Ollama：在本機執行 `qwen2.5:3b`，負責文章摘要。
- 使用者：自行安裝、執行與測試，已確認 uv 執行成功；仍須核對摘要是否忠於原文，並補上完整成果紀錄。
