# 無限進化 展示頁

公開的美術預覽網站（GitHub Pages）：<https://wincafex2-lu.github.io/evolution-wilds-demo/>

這個倉庫**只放建置好的靜態檔案**（HTML、JS、CSS、GLB 模型），沒有原始碼。原始碼在私人倉庫 `evolution-wilds`。

| 路徑 | 內容 | 來源 |
| --- | --- | --- |
| `index.html` | 首頁，依階段排列（新生物做好後加一張卡片，連到 `creatures/?creature=<ID>`） | 手寫 |
| `art/` | 第 6 階段 芽尾獸 × 草原（含攻擊、受擊、轉身、進食） | 美術樣板 + hit-turn-eat-clips 補丁 |
| `game/` | 遊戲本體 M1 測試版（泓 2026-11-14 同意公開） | 私人倉庫 `evolution-wilds` 的 `release/m1`（32347bc），`apps/web` 用 `npx vite build --sourcemap false` 建置（不要附 .map） |
| `creatures/` | 進化線生物實驗室（目前：1 螞蟻、2 蚱蜢、3 螳螂、4 蟑螂、5 蚯蚓、7 史前鱷魚、8 小蛇、9 蟒蛇、10 泰坦巨蟒、神獸線 11 小蛟龍） | 生物實驗室；蚯蚓的 `annelid*`、蛇類的 `scale*` 材質與它們的 GLB 來自 evolution-wilds 分支 `claude/project-thread-jtd15f` |

`.nojekyll` 讓 GitHub Pages 原樣提供所有檔案。

## 展示版專屬修改

- 兩個頁面：觸控裝置（iPad）**關掉即時陰影**（泓的決定，iPad 和 iPad mini 都會閃黑影），改用 `src/showcase/blobShadow.ts` 在生物腳下畫柔和的橢圓影子；載入與切換畫質都套用。`?shadows=1` 在觸控裝置打開陰影，`?shadows=0` 在電腦上關掉。
- 兩個頁面：觸控裝置（iPad）預設關掉 Bloom 與 MSAA（`src/showcase/app.ts` 裡 `coarse` 判斷）。iPad 中畫質移動時出現閃爍黑影，低畫質沒有；網址加 `?bloom=1&msaa=4` 可以恢復原本設定做比較。iPad mini 中畫質仍有黑影，所以觸控裝置的中／高畫質陰影改成 1 層、只涵蓋 45 公尺（`?cascades=2&shadowfar=150` 恢復）；`?shadows=0` 完全關掉陰影，用來排查。
- 兩個頁面與首頁：禁止 iPad 頁面縮放（viewport `user-scalable=no`、擋 gesture／dblclick／連點 touchend，`touch-action`）。
- 兩個頁面：左下面板最上方有「▾ 收合」按鈕，可把面板、標題和數據收起來（每台裝置會記住）。
- `creatures/`：小蛟龍的飛行動作（Hover／TakeOff／Land）在展示台多一列「飛行」按鈕，只對 `creatureFiles.ts` 的 `FLYING_CREATURES` 顯示；`dragonMaterial.ts` 來自分支 `claude/project-thread-hx3b3h`。
- `creatures/`：遊戲鏡頭在觸控裝置上加了右下角圓形「攻擊」按鈕（跟美術樣板一樣），讓一隻手指轉視角時另一隻手指可以攻擊。原始實驗室沒有這個按鈕；這個修改只在展示版。

## 怎麼更新（有新生物通過審核時）

1. 把來源資料夾複製到一個工作目錄（不要直接在共享資料夾裡建置），例如：
   - 美術樣板：`evolution-wilds-art-sample/`，再套用最新的動作補丁（`patch -p1 < .../from-sample.patch`，並複製補丁裡的 `reference-showcase/src/showcase/*` 和 `assets/creatures/*`）。
   - 生物實驗室：`evolution-wilds-creatures/`（新生物的 `<ID>.glb`、`_LOD1`、`_LOD2` 放進 `assets/creatures/` 就會自動出現在選單）。
2. 在工作目錄執行 `npx -y npm@11.21.2 ci`、`npx tsc -p tsconfig.json`、`npx vite build`（`vite.config.ts` 已設 `base: './'`，路徑都是相對的）。
3. 若共享實驗室還沒有蚯蚓或蛇，把 `annelid*.ts`、`scale*.ts` 和 `creatureLooks.ts`／`creatureView.ts` 的 annelid 分支合進去。若生物實驗室的 `src/showcase/panel.ts`、`styles.css` 沒有 `evo-strike` 攻擊按鈕，照上面「展示版專屬修改」再加回去（觸控畫質設定也一樣）。
4. 用新的 `dist/` 整個取代這裡的 `art/` 或 `creatures/`，必要時更新首頁文字，推上 `main`。Pages 約一兩分鐘後更新。
5. 不要把原始碼、`.map`、截圖審查頁或任何個人資料放進來。
