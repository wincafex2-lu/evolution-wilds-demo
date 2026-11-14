# 無限進化 展示頁

公開的美術預覽網站（GitHub Pages）：<https://wincafex2-lu.github.io/evolution-wilds-demo/>

這個倉庫**只放建置好的靜態檔案**（HTML、JS、CSS、GLB 模型），沒有原始碼。原始碼在私人倉庫 `evolution-wilds`。

| 路徑 | 內容 | 來源 |
| --- | --- | --- |
| `index.html` | 首頁，連到下面兩個預覽 | 手寫 |
| `art/` | 芽尾獸 S1 × 草原（含攻擊、受擊、轉身、進食） | 美術樣板 + hit-turn-eat-clips 補丁 |
| `creatures/` | 進化線生物實驗室（目前：螞蟻、蚱蜢） | 生物實驗室 |

`.nojekyll` 讓 GitHub Pages 原樣提供所有檔案。

## 展示版專屬修改

- `creatures/`：遊戲鏡頭在觸控裝置上加了右下角圓形「攻擊」按鈕（跟美術樣板一樣），讓一隻手指轉視角時另一隻手指可以攻擊。原始實驗室沒有這個按鈕；這個修改只在展示版。

## 怎麼更新（有新生物通過審核時）

1. 把來源資料夾複製到一個工作目錄（不要直接在共享資料夾裡建置），例如：
   - 美術樣板：`evolution-wilds-art-sample/`，再套用最新的動作補丁（`patch -p1 < .../from-sample.patch`，並複製補丁裡的 `reference-showcase/src/showcase/*` 和 `assets/creatures/*`）。
   - 生物實驗室：`evolution-wilds-creatures/`（新生物的 `<ID>.glb`、`_LOD1`、`_LOD2` 放進 `assets/creatures/` 就會自動出現在選單）。
2. 在工作目錄執行 `npx -y npm@11.21.2 ci`、`npx tsc -p tsconfig.json`、`npx vite build`（`vite.config.ts` 已設 `base: './'`，路徑都是相對的）。
3. 若生物實驗室的 `src/showcase/panel.ts`、`styles.css` 沒有 `evo-strike` 攻擊按鈕，照上面「展示版專屬修改」再加回去。
4. 用新的 `dist/` 整個取代這裡的 `art/` 或 `creatures/`，必要時更新首頁文字，推上 `main`。Pages 約一兩分鐘後更新。
5. 不要把原始碼、`.map`、截圖審查頁或任何個人資料放進來。
