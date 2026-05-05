# Git Rebase 團隊協作實戰手冊

歡迎加入「開發進度看板」專案！本練習旨在模擬真實團隊開發中，當多人同時修改同一個檔案（`index.html`）時，如何透過 `git rebase` 保持專案歷史紀錄的整潔，並優雅地解決衝突。

---

## 🚀 練習情境
我們正在開發一個網頁看板。專案結構已經由 Lead 建立完成：
- **全域資訊區**：由 Lead 維護。
- **個人任務區**：Alice, Bob, Charlie 分別負責 `#alice`, `#bob`, `#charlie` 三個區塊。

---

## 🛠 操作流程

### 1. 準備環境
請將專案 Clone 到本地並建立自己的開發分支：
```bash
git clone <專案 URL>
cd rebase-practice
git checkout -b feature-<你的名字>

# 🔍 檢查點：確認目前分支標記為 feature-<你的名字>
git branch
```

### 2. 進行開發
請用編輯器打開 `index.html`，找到屬於你的區塊（例如 `id="alice"`），將內容修改後提交：
```bash
git add index.html
git commit -m "Feat: 更新 <你的名字> 的任務內容"

# 🔍 檢查點：確認 Commit 已成功建立，且目前領先於 main
git log --oneline --graph --all
```

### 3. 同步 Lead 的更新 (開始 Rebase)
當 Lead 更新了 `main` 分支後，請執行以下步驟：
```bash
# 1. 取得遠端最新狀態
git fetch origin

# 2. 🔍 重要檢查點：觀察「分岔」！
# 你應該會看到兩條路徑從某個點分開，一條是你的進度，一條是 Lead 的進度
git log --oneline --graph --all

# 3. 開始 Rebase (將你的進度重新基於 origin/main)
git rebase origin/main
```

### 4. 處理衝突 (Conflict Resolution)
如果在 Rebase 過程中遇到衝突：
1. **打開檔案**：找到 `<<<<<<< HEAD` 與 `>>>>>>>` 標記。
2. **手動修復**：決定保留內容（建議兩者並存，但確保 HTML 標籤完整）。
3. **標記解決並繼續**：
   ```bash
   git add index.html
   git rebase --continue
   ```

### 5. 驗證與推送
1. **預覽**：在瀏覽器打開 `index.html`，確認你的修改與 Lead 的修改都正確呈現。
2. **🔍 重要檢查點：觀察「直線」！**
   # 你應該會看到分岔消失，你的 Commit 現在排在 origin/main 的正上方
   git log --oneline --graph --all
3. **推送**：
   ```bash
   git push origin feature-<你的名字> --force-with-lease
   ```

---

## ⚠️ Rebase 準則
**「絕不要對已經推送 (push) 到公共分支 (如 main) 的 Commit 進行 Rebase。」**
本練習中，我們只對「尚未合併到 main 的個人 feature 分支」進行 Rebase，這是安全且被鼓勵的做法。

---
祝開發順利！如果有任何衝突無法解決，請呼叫你的 Lead。
