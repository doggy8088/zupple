# Zupple

`Zupple` 是一個以瀏覽器執行的滑塊拼圖 (Sliding Puzzle) 小遊戲。  
使用者可以上傳自己的圖片，系統會自動裁切成正方形並切成拼圖，支援桌機與手機操作，搭配 WebGL 星空背景特效。

## 線上體驗

- 正式網站：<https://zupple.gh.miniasp.com>

## 功能特色

- 自訂圖片上傳：可從裝置選擇圖片作為拼圖來源。
- 自動正方形裁切：以圖片中心裁切成 1:1，避免變形。
- 三種難度：`3x3`、`5x5`、`7x7`。
- 保證可解：洗牌採「合法移動」隨機打亂，不會出現無解盤面。
- 看原圖提示：按住 `👀 按住看原圖` 可暫時顯示完整參考圖。
- 響應式設計：支援滑鼠與觸控操作。
- 星空背景特效：以 Three.js + Shader 製作具景深與閃爍效果的星空。

## 遊戲規則

1. 先在主畫面選擇難度，或上傳自訂圖片。
2. 點擊 `開始遊戲` 進入拼圖畫面。
3. 只能移動與空格相鄰的拼圖塊。
4. 當所有拼圖塊回到正確位置即完成。
5. 可按住 `👀 按住看原圖` 進行短暫提示。

## 本機執行

### 需求

- 任一現代瀏覽器（Chrome / Edge / Firefox / Safari）。
- 可連網（載入 Three.js CDN）。

### 方法 1：直接開啟

1. 進入專案根目錄。
2. 直接以瀏覽器開啟 `index.html`。

### 方法 2：使用本機靜態伺服器（建議）

```bash
python -m http.server 8080
```

然後在瀏覽器開啟：<http://localhost:8080>

## 操作說明

- `更換圖片`：上傳新的拼圖圖片。
- `3x3 / 5x5 / 7x7`：切換拼圖難度。
- `開始遊戲`：切換到遊戲畫面並開始新局。
- `← 退出`：回主選單。
- `👀 按住看原圖`：按住顯示原圖，放開恢復拼圖。
- `再玩一次`：以同一張圖片重開新局。

## 專案結構

```text
.
├─ index.html                           # 主程式（HTML/CSS/JS 全在同檔）
├─ CNAME                                # GitHub Pages 自訂網域
├─ .nojekyll                            # GitHub Pages 設定
└─ .github/
   └─ workflows/
      └─ copilot-setup-steps.yml        # Copilot setup workflow
```

## 技術架構

- 前端：`HTML + CSS + Vanilla JavaScript`
- 3D 背景：`Three.js r128`（透過 cdnjs 載入）
- 圖片處理：`Canvas 2D API`（裁切、預設圖片生成）
- 拼圖資料模型：以陣列儲存每個 tile 的目標與目前位置

## 核心流程摘要

1. 圖片輸入：
   - 使用 `FileReader` 讀取圖片。
   - 透過 `cropImageToSquare()` 進行中心裁切。
2. 建立盤面：
   - `createBoard()` 依難度建立 tile 與背景定位。
3. 洗牌：
   - `shuffle()` 透過多次合法移動打亂盤面，維持可解性。
4. 遊戲互動：
   - `move()` 驗證是否可移動並更新步數。
   - `checkWin()` 驗證是否完成並顯示勝利畫面。
5. 背景特效：
   - `createStarLayer()` 建立多層星空粒子。
   - 自訂 Shader 畫出圓形光暈與閃爍效果。

## 可調整項目（快速客製）

可在 `index.html` 內直接調整以下參數：

- 難度按鈕：主選單 `setDiff(3/5/7)`。
- 洗牌次數：`shuffle()` 內 `moves` 設定。
- 星空密度：`nearStars` / `farStars` 的 `count`。
- 星空大小與亮度：`sizeMin`、`sizeMax`、`brightness`。
- 勝利動態強度：`boom()` 內的 `speed`。

## 部署

此專案為純靜態網站，可部署到任何 Static Hosting：

- GitHub Pages
- Cloudflare Pages
- Netlify
- Vercel（Static）

若使用 GitHub Pages 並設定自訂網域，可保留 `CNAME` 檔案。

## 瀏覽器與裝置相容性

- 支援桌機滑鼠操作。
- 支援手機觸控操作（包含按住看原圖）。
- 需瀏覽器支援 WebGL 與 ES6。

## 已知限制

- 目前所有程式碼集中在單一 `index.html`，不利大型維護。
- 沒有儲存進度功能，重新整理頁面會重置狀態。
- 未提供自動化測試與程式碼格式化流程。

## 建議後續優化

1. 拆分為 `index.html` + `styles.css` + `app.js`。
2. 增加最佳成績（步數/時間）與本地儲存（LocalStorage）。
3. 新增鍵盤操作與無障礙（a11y）支援。
4. 加入離線資源（例如自架 three.js）避免外部 CDN 失效。

## 授權

本專案採用 `MIT License` 授權，詳細內容請參考根目錄的 [LICENSE](LICENSE) 檔案。
