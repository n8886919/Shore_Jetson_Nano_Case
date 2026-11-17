# Shore Jetson Nano Case

Shore Jetson 的 Jetson Nano、顯示器外殼與舵機大底板組裝參考。

![組裝參考](assembly/01_assembled_reference.png)

## 設計與組裝檔

- `Nano_Case_v05_DRAFT.FCStd`：原始可編輯模型，保留草圖、特徵與參數。
- `nano_case_with_screen.stl`：本機既有列印匯出檔，按原樣保存；此次未重新匯出。
- `大底板.DWG`、`大底板.IGS`：本機既有大底板來源，按原樣保存。
- `assembly/Shore_Jetson_Nano_Case_Assembly.FCStd`：獨立組裝快照，包含外殼、既有 Nano 硬體參考與大底板。原始模型的草圖、尺寸與特徵不受此檔影響。
- `assembly/02_top_alignment.png`、`assembly/03_slot_side.png`：孔位與插槽位置預覽。
- `assembly/verification.json`：來源雜湊、定位與幾何檢查結果。
- `README_v05.txt`：Drive 原版的歷史設計說明，尺寸與驗證不代表目前加上顯示器框的版本。

組裝檔內是凍結的幾何快照。後續修改原始設計後，需要另外更新組裝快照；不會自動修改或重算原始檔。

## 版本歷史

1. `5def314`：匯入 Google Drive 的原始 v05，排除 FCBak 與 STEP。
2. `a1d5e46`：保存 2026-11-18 本機現況，包含顯示器外殼與底板來源；原始 FCStd 和既有匯出檔逐位元組保留。移除原版 `Nano_Case_v05_DRAFT.stl`、舊圖與舊驗證檔，反映本機目錄當時的實際內容。舊圖與舊驗證仍可從第一個 commit 取回。
3. 後續組裝更新：新增獨立組裝檔、本頁及預覽圖，原始設計檔保持不變。

第二個 commit 的 `Nano_Case_v05_DRAFT.FCStd` SHA-256：

```text
958c75cda8235a8606d0e8c6ded55bea8f2996a24d02a64355517ed111d412b6
```

## 大底板定位與檢查

以 FreeCAD 26.3.0 RC2 建立與檢查組裝。

- 大底板以原 IGS 的最大頂面沿原有 2 mm 厚度建立單一實體，保留該面的輪廓及孔洞；來源 IGS 未修改。原始 IGS 本身是曲面集合，沒有封閉實體。
- 大底板繞 Z 軸旋轉 90°，平移 `(-86, 45, -2.5) mm`。
- 底板端部兩孔對準外殼 `(113, 18.2)` 與 `(113, 71.8) mm`，孔距 `53.6 mm`。
- 2 mm 厚底板位於 Jetson Nano 與顯示器框之間的 3 mm 插槽中央，Z 範圍 `-2.5 .. -0.5 mm`，上下名義間隙各 `0.5 mm`。
- 底板長柄寬 85 mm，插槽名義寬 86 mm。
- 組裝外殼與底板皆為有效單一實體，交集體積為 `0 mm³`。保存後重新開啟組裝副本，核對外殼體積與底板位置。

以上是 CAD 靜態定位與干涉檢查，尚未實體列印、試裝或驗證滑入路徑。Nano、風扇與五金沿用原檔參考；未提供精確顯示器實體模型，因此圖中顯示器部分為既有外殼框。

## 日常保存

完成一項修改後儲存 FCStd，再用 Git 提交；版本由 commit 或列印用 tag 管理，不必再複製多個版本檔名。

準備列印時，從確認過的最終外殼重新匯出 STL，並和對應的 FCStd 一起提交。記錄實際列印與試裝結果，區分 CAD 檢查和實物驗證。

FCBak、FCStd 自動備份與 STEP 已由 `.gitignore` 排除；FCStd 以二進位檔案管理，避免文字合併。
