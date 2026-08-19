# PoE1 BD 產生器與驗證器

本技能為指定版本或聯盟產生可重現的《Path of Exile 1》BD 推薦。它結合公開的 PoE Ninja 統計資料與透過 `qpooqp777/pob-cli` 執行的 Path of Building Community 計算，最後輸出**初期、中期與後期**三階段配置。

主動技能預設語言為英文；本檔為繁體中文說明。簡體中文版本請見 [`README.zh-CN.md`](README.zh-CN.md)。實際載入的預設技能文件是英文版 [`SKILL.md`](SKILL.md)。

## 功能

產生器可接收聯盟、主技能、職業／昇華、遊戲模式、經濟環境、預算與目標。它會取得或接收合法的 Ninja JSON 快照、篩選候選 BD、解釋排名，並在本機具備 PoB Community 根目錄與 LuaJIT 時驗證 PoB XML。

每個 BD 都會拆成三個階段，而不是只列一份裝備清單。初期涵蓋劇情與初入地圖，中期涵蓋穩定刷圖，後期涵蓋成熟或高預算配置。每一階段都包含技能與連線、天賦路線、裝備目標、防禦門檻、轉換條件、PoB 指標、假設與警告。

## PoE 3.29 範例

當使用者要求「推薦 PoE1 3.29 BD」時，產生器以 **Curse of the Allflame** 作為人類可讀的聯盟範例，接著核對實際聯盟識別碼並取得帶時間戳的資料快照。不可從影片、搜尋摘要或單一人氣圖表硬編一個「最強 BD」。

報告可以比較 `Vortex Occultist`、`Kinetic Fusillade Totems Hierophant`、`Static Strike Slayer` 等候選，但前提是它們確實存在於指定的 3.29 快照中。這些名稱只是查詢假設，不是預先認證的 Tier List；最終選擇必須符合模式、預算、主技能、防禦要求與 PoB 驗證狀態。

> GGG 官方 3.29.0 公告將聯盟名稱列為 Curse of the Allflame，並提供 Standard、Hardcore 與 Solo Self-Found 變體。[1]

## 資料與驗證模型

PoE Ninja 被視為統計證據。由於公開建構頁主要由前端動態載入，產生器優先使用合法的 JSON 端點、資料傾印、使用者提供的匯出檔、公開角色資料或 PoB code，並記錄來源 URL、擷取時間、聯盟、篩選條件、回應格式與樣本數。不得繞過登入、速率限制或私人角色權限。

PoB Community 被視為計算權威。產生器先檢查技能、天賦與裝備，再以明確指定的主技能執行 `pob analyze` 與 `pob calc`。若缺少 LuaJIT、PoB root、XML 不相容或計算失敗，必須輸出警告並標示未驗證，不得自行捏造數字。

## 常用命令

```bash
python scripts/poe1_build_pipeline.py \
  --league "Curse of the Allflame" \
  --skill "Vortex" \
  --class "Witch" \
  --ascendancy "Occultist" \
  --build-name "3.29 Vortex Occultist" \
  --ninja-json ninja.json \
  --build BUILD.xml \
  --pob-root "$POB_ROOT" \
  --output report.json
```

內建流水線會保留候選資料與警告，能呼叫 `pob analyze` 時就執行，且永遠不會上傳 BD。若要只產生 code 而不公開：

```bash
pob share BUILD.xml --dry-run
```

公開 `pobb.in` 連結必須另外取得明確確認；「已產生 code」與「已公開 URL」是不同狀態。

## 參考資料

[1]: https://www.pathofexile.com/forum/view-thread/3985332 "Content Update 3.29.0 — Path of Exile: Curse of the Allflame"
[2]: https://poe.ninja/poe1/builds "PoE Ninja — Path of Exile 1 Builds"
[3]: https://github.com/qpooqp777/pob-cli "qpooqp777/pob-cli"
