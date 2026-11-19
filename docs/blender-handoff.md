# 跨電腦 Blender 渲染交接

更新日期：2026-11-19（Asia/Taipei）。本次只準備 Git 與交接資料；尚未建立 Blender 場景、匯出器或渲染腳本。

## 另一台電腦從這裡開始

```sh
git clone https://github.com/n8886919/Shore_Jetson_Nano_Case.git
cd Shore_Jetson_Nano_Case
git status --short --branch
```

若目的地已有 checkout，先檢查它的修改並保留；確認可以快轉更新時才執行 `git pull --ff-only`。不要覆蓋另一台電腦的既有工作。

開啟專案後，可直接把 [接續工作 prompt](start-promo-prompt.txt) 貼給另一台電腦的 agent。

使用 **GPT-6.1 Sol / High** 建立首次匯入與場景，流程穩定後的一般材質／燈光調整可用 Medium。這是本案的工作建議，不是兩個模型在本案的實測比較。

先讀 repo 根目錄的 [AGENTS.md](../AGENTS.md)、[README.md](../README.md)、[60 秒宣傳片需求](promo-brief.md) 與 [免費素材候選](promo-assets.md)，再處理以下工作。各主機的安裝路徑不相同，請實際尋找 FreeCAD、Blender 和 Python；不要沿用原主機的絕對路徑。

## 目前的 Git 與模型基準

| 項目 | 已確認狀態 |
|---|---|
| Repository | `https://github.com/n8886919/Shore_Jetson_Nano_Case` |
| 交接分支 | `main` |
| 最新 CAD 裝配提交 | `ab68dad5f44dbd8a4daf9d4355e3f660ac6e2edb` |
| 使用者更新的 STL 提交 | `b936dbb`；只保存原有 STL 修改，沒有重新匯出 |
| CAD 主檔 | `Nano_Case_v05_DRAFT.FCStd`，12,168,684 bytes |
| FCStd SHA-256 | `e278736467a4bf4baa7b2373814f2324a7128c24c5db7d0ce4634c66dce04fa4` |
| 主檔記錄的 FreeCAD 版本 | `26.3R20261018 (Git shallow)` |
| 模型 | 392 個物件；原有 `Reference` 裝配，雲台幾何已內嵌 |
| STL | `nano_case_with_screen.stl`，1,487,384 bytes；不作為 Blender 組裝來源 |
| STL SHA-256 | `b33e0f85ed7b45baa85a323db3127473a1d91335fde30166f8c03f0ccfb22f15` |

CAD 原圖保持唯一來源。渲染前後在 repo 根目錄執行以下 Python；Windows 可依環境改用 `py -3`，Linux/macOS 可用 `python3`，在 Python 中貼上程式碼：

```python
from pathlib import Path
from hashlib import sha256

expected = {
    "Nano_Case_v05_DRAFT.FCStd": "e278736467a4bf4baa7b2373814f2324a7128c24c5db7d0ce4634c66dce04fa4",
    "nano_case_with_screen.stl": "b33e0f85ed7b45baa85a323db3127473a1d91335fde30166f8c03f0ccfb22f15",
}
for name, fingerprint in expected.items():
    actual = sha256(Path(name).read_bytes()).hexdigest()
    if actual != fingerprint:
        raise SystemExit(f"STOP: {name} differs from the handoff baseline: {actual}")
    print(f"OK: {name}: {actual}")
```

若雜湊不同，先用 Git 確認是否有使用者較新的合法提交；不要把新版本還原成這裡的舊基準。不要為了修復匯入錯誤而另存覆蓋原 FCStd。

## 交付與實作方式

**最新目標是 60 秒的宣傳動畫**，完整劇本與優化空間見 [60 秒宣傳片需求](promo-brief.md)。這取代先前「只做靜態樣張／不做新動畫」的階段目標；原有歷史 MP4 仍不可當成本版定位基準。

1. 開頭是精美的工程組裝動畫，**螢幕在上、Jetson Nano 在下**。
2. 環境轉入海邊，產品與攝影腳架、露營電源接合，走入使用場域。
3. 出現模擬衝浪者與追蹤子母畫面，辨識畫面必須沿用我們的 App。
4. agent 可優化分鏡、節奏、轉場及免費素材搭配，總片長維持 60 秒。

實作原則：

- FreeCAD 負責工程幾何；Blender 負責材質、燈光、鏡頭、動畫與合成。使用者已確認有 Google AI Plus，可把其生圖／生影片工具當成輔助資源，詳見劇本文件。
- 建立可重複執行的 Python 匯入／場景／動畫腳本，以及必要的小型設定檔；先滿足這個作品，不建立額外框架、服務或 AI 控制外掛。
- 建議腳本與設定放在 `render/`，場景放在 `render/scenes/`，樣張放在 `render/previews/`，最終影片放在 `render/output/`。除 App 參考截圖外，以上實作目前尚未建立；可依目的地主機調整並記錄。
- `.blend` 是呈現用衍生檔；原 FCStd 不作修改。先確認分件、名稱、階層、世界座標與單位能保留，不能假設 Blender 能直接開啟 FCStd。
- 工程組裝段以共同父層或鏡頭方向翻轉整組呈現，滿足螢幕在上、Nano 在下；不要改原 FCStd 的 Placement、草圖或特徵。已組裝產品轉入海邊時，保持零件相對位置，呈現姿態與腳架的接合由 agent 檢查。
- 確認 FreeCAD 每個物件的完整世界變換，只套用一次；外殼使用 Body 的最終 Tip，避免把中間 Pad／Pocket 和草圖重複渲染。保留有意呈現的內部零件，不以 GUI 的暫時可見狀態當作唯一匯出依據。
- 在增加材質前，確認外殼、164×97 mm 顯示器、底板與四根雲台支柱的尺寸及相對位置，並和現有組裝圖片核對。材質／燈光／相機／時間軸與幾何更新應能分開維護。
- 先做關鍵畫面與低解析 animatic，檢查 60 秒節奏、翻轉方向、穿模與 App 畫面，再做高品質成品。這是迭代步驟，不是把最終交付縮成一張靜態圖。
- 記錄實際 FreeCAD／Blender 版本、重建與渲染命令。檢查實際圖片及整支影片，不能只因程序正常結束就宣告完成。
- 免費素材逐一記錄作者、來源、授權與下載方式。能用它渲染，和能把原始網格／貼圖／含它們的 `.blend` 放進公開 Git 是不同問題，依各素材授權處理。
- 新建工作分支時使用 `codex/` 前綴。大型暫存網格、render cache、逐格輸出與自動備份不要混入提交；腳本、設定、需要保留的場景及成品依交付需要追蹤。

## 使用者已指定的材質外觀

| 零件 | 要求 |
|---|---|
| `BasePlate` 大底板 | **金屬，黑色磨砂**。FreeCAD 現有金黃色只作識別，不能照搬成黃銅材質。 |
| `Enclosure` 3D 列印外殼 | **常見奶白色／off-white 的塑膠外觀**，偏霧面；使用者未指定 PLA、PETG、ASA 或其他聚合物。 |
| PCB、螺絲、接頭、支柱與螢幕 | 依零件辨識設定合理展示材質；未確認的顏色／材質屬視覺假設，回報時說明。 |

可以用材質粗糙度、輕微表面紋理與柔和陰影表達質感。不要為了看起來更漂亮，改孔徑、外形、壁厚或工程圓角；如需純渲染效果，限制在 Blender 的衍生場景。

## 裝配資訊與必須保留的內容

- 唯一工程檔是 `Nano_Case_v05_DRAFT.FCStd`。原 `Reference` 的標籤是「裝配參考 — 不要匯出 STL」。不要開另一份工程組裝 FCStd。
- `Enclosure` 為唯一列印件，最終 Tip 是 `Pocket007`；使用者新增的 `Sketch014`／`Pocket007` 已保留，不可刪除、重算覆寫或重建替代。
- 顯示器是 Waveshare 70H-1024600 **IPS 無觸控**，主體 164×97×2.6 mm。轉接板是 50×25.5 mm、孔距 44.7×20.2 mm；細節以 README 和模型為準。
- 顯示器入口的兩個 R6 半圓小耳在螢幕背面，名義 M3×8 螺絲從背面插入；螺母仍在入口內。不要翻到顯示正面。
- `BasePlate` 圓形端**靠近 DC 電源孔**，目前 Z 軸旋轉 +90°、位置 `(-86,45,-3) mm`。插入位置是 `BasePlate.Placement.Position.x`，目前 X=-86；只做渲染時不要調整。
- `Gimbal.Placement` 使用 `MountPlate.Placement * MountOffset`，整個雲台及四顆 M4×10 固定螺絲跟隨底板。來源含 68 個零件，另有 4 顆底板固定螺絲。
- 監視器外觀已指定為 [Remastered Cameras — bunny man](https://sketchfab.com/3d-models/remastered-cameras-ea93654b1d3a4017a129e76f90df9c47)；下載狀態及 CC BY 4.0 資訊見 [素材文件](promo-assets.md)。不要用腳架素材附帶的 DSLR／電影機替代，也不要把這個外觀模型當成已確認尺寸的工程件。
- 目前雲台是靜態裝配，沒有可直接沿用的雙軸運動約束。若以後要做雲台動畫，需先辨識固定／水平／垂直的剛體分組與轉軸，不能把所有雲台零件當成同一個可動體。
- 外殼內安裝順序為：轉接板 → Nano → 大底板 → 顯示器 → 入口止擋五金；底板兩顆固定螺絲應在插入顯示器前鎖好。雲台可先和大底板以四顆 M4×10 接合，實際操作空間仍待確認。
- `assembly/assembly_sequence.mp4` 是 `667ebc6` 的歷史動畫，底板方向與轉接板高度已過時，也沒有本版完整雲台。只把現行 FCStd 與靜態組裝圖片當作本版定位參考。

## 已知驗證界線

`assembly/verification.json` 記錄的是 2026-11-18 的存檔與靜態 CAD 檢查，不是這次新做的實體驗證。

- 底板槽高 3 mm，容納 2 mm 底板與模型中 1 mm 的 Nano 底部螺絲頭，垂直名義餘量為 0 mm。
- Ø4 孔搭配 M4 螺絲為名義零孔徑餘量。
- 雲台全行程、完整滑入路徑、線材、工具空間、列印強度與實物組裝尚未確認。
- 渲染不能修正或證明這些問題；若觀察到問題，記錄並回報，不擅自修改工程檔。
