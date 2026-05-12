# Vanguard Keyboard Outsider 先鋒二號局外者

![info](pic/outsider/info.jpg)

<br>

## 前言

「先鋒系列」是我個人基於「探索者三號」開發架構上的一個分支點，因爲在探索者三號的設計裡，在探索者三號的設計中，我將鍵盤主控區域「模組化」，使其觸控板與 `ProMicro` 主控模組能徹底沿用至後續作品。

- 爲什麼這把鍵盤是二號？

先鋒一號的代號叫做 `Mono`，中文代號叫做「雙子姐妹」，考慮到有人在敲碗 zmk 韌體的開發，因此我另開了一把鍵盤先嘗試研究 zmk，順勢做出一套屬於無線版的韌體，成功之後就會回頭將「一號」的開發完成。

——當然還有「探索者三號」的 `v2` 及無線韌體版本開發。

因爲是基於「探索者系列」，先鋒雖然比較侷限在「模組化」的區域做開發，但我不會捨棄掉「探索者」的自由度。相對的，這份自由反而讓我在開發的時候產生錯亂，算是一個小小的意外。

`Plank` 一直都是我非常喜歡的直列始祖，`3` 號也是基於 `Plank` 開發的鍵盤，我的想法很簡單：

——從最原始的開始，就一定會有更好的創意。

<br>

- 輔助搭配：
    - `about-zmk-XXX` 教學文本。
    - `QMK Firmware`：https://qmk.fm/
    - `VIAL`：https://get.vial.today/
    - `ZMK Firmware`：https://zmk.dev/
    - `KLE`：https://www.keyboard-layout-editor.com/#/
    - `nRF Connect SDK`：
        - `Android` 連結：https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp&pcampaignid=web_share
        - 電腦端：https://www.nordicsemi.com/Products/Development-tools/nrf-connect-for-desktop/download
        - 用於初次藍牙發射訊號捕捉除錯用。
    - `FreeCAD`。

<br>

## 硬體設計

### 自由度及限制

`Outsider` 被我設計成這種獨特的外型的樣子，其中最主要的原因就是「自由度」的保留，那麼它的自由度在哪裡？答案很簡單——

——它可以「任意」翻轉使用。

然後它是基於探索者三號的延伸設計，也就是說它的主控區域是沿用下來的，材料清單將註明哪些組件直接沿用「探索者三號」的設計圖資。

<br>

太極理論有「物極必反」的核心概念，任何的設計上也一樣，這把鍵盤給予了極致的自由度，它「目前」的限制列在下方：
1. 支援焊接。
2. 不支援 `Gateron Low Profile 2.0`，也就是 `GLP2.0`。
3. 觸控板只支援 `QMK` 有線版。

<br>

自由度的部分依據「限制」，列在下方：
1. 支援 `MX` 腳位的「鍵軸」，不限「矮軸」。
2. 支援 `GLP3.0` 矮鍵軸。
3. 支援 `Choc` 腳位的「鍵軸」。
4. 支援全電路板、外殼部件翻轉組裝。
5. 支援 `zmk` 編碼器無線版。
6. 支援 `qmk` `Azoteq TPS43` 及 `Cirque TM040040` 觸控板、編碼器有線版。
7. 支援多配列焊接調整。
8. 支援物理拆板組裝使用。

<br>

### 電路設計

#### 核心電路板

<table>
  <tr>
    <td width="100%">
      <img src="pic/outsider/A.png" width="100%" alt="A">
    </td>
  </tr>
  <tr>
    <td width="100%">
      <img src="pic/outsider/B.png" width="100%" alt="B">
    </td>
  </tr>
</table>

<br>

#### 定位板、主控把擋板及底板

<table>
  <tr>
    <td width="50%">
      <img src="pic/outsider/B1.png" width="100%" alt="B1">
    </td>
    <td width="50%">
      <img src="pic/outsider/M.png" width="100%" alt="M">
    </td>
  </tr>
  <tr>
    <td width="50%">
      <img src="pic/outsider/P1.png" width="100%" alt="P1">
    </td>
    <td width="50%">
      <img src="pic/outsider/P2.png" width="100%" alt="P2">
    </td>
  </tr>
</table>

<br>

#### 主控及模組

<table>
  <tr>
    <td width="50%">
      <img src="pic/outsider/T1.png" width="100%" alt="T1">
    </td>
    <td width="50%">
      <img src="pic/outsider/T2.png" width="100%" alt="T2">
    </td>
  </tr>
  <tr>
    <td width="50%">
      <img src="pic/outsider/T3.png" width="100%" alt="T3">
    </td>
    <td width="50%">
      <img src="pic/outsider/T4.png" width="100%" alt="T4">
    </td>
  </tr>
</table>

<br>

#### 3D 部件

<table>
  <tr>
    <td width="50%">
      <img src="pic/outsider/T5.png" width="100%" alt="T5">
    </td>
    <td width="50%">
      <img src="pic/outsider/T6.png" width="100%" alt="T6">
    </td>
  </tr>
</table>

<br>

## 使用材料

使用材料的部分比較簡單，`Outsider` 並沒有做過多的 `3D` 設計，一貫保留「三明治」結構。

||名稱|數量|數量||
|--|--|--|--|--|
|⚠️ **電路板**|**配列**|**4x10**|**4x12**|**備註**|
||主電路板|1|1|厚度 `1.2mm`|
||底板|1|1|厚度 `1.6mm`|
||`4x8` 定位板|1|1|**下方備註**|
||`4x2` 定位板|1|2|**下方備註**|
|`TM040040`|模組 `A`|1|1|**三號部件**|
|`TM040040`|模組 `B`|1|1|**三號部件**|
|`TM040040`|模組 `C`|1|1|**三號部件**|
|`TPS43`|模組 `D`|1|1|**三號部件**|
|⚠️ **主控**|**腳位**||||
|`ATMega32U4`|`ProMicro`|1|1|**基礎有線版**|
|`RP2040`|`ProMicro`|1|1|**有線觸控板**|
|`nRF52840`|`ProMicro`|1|1|**支援旋鈕無線版**|
|⚠️ **電子部件**|**名稱/規格**||||
|`1N4148`|`SOD-123`|40-41|48-49||
|`EC-11`/`EC-12`|`L10mm` `A4.5-5.0mm`|1|1|**選配**|
|輕觸開關|`6x3.5x4.3mm` `2 Pin`|1|1||
|輕觸開關|`SKRK` `2mm`|1|1|**選配**|
|撥動開關|`MSKT-12D14`|1|1|**選配**|
|小於 `501735`|`3.7V` 聚合物鋰電池|1|1|**選配，置於 MCU 上方**|
|-|`3.7V` 鋰電池電量顯示模組|1|1|**選配**|
|`PH2.0` `THT` 公母接頭|`JST PH S2B-PH-K`|1|1|**選配**|
|💡 **TM040040**|||||
|**名稱**|**規格**||||
|觸控板|-|1|1|**選配**|
|電阻|`1/4W` `4.7K`|2|2||
|排線|`0.5x5mm` `FFC 12pin` `AA`|1|1||
|排線座|`0.5mm` `FFC 12pin`|1|1|**不限翻蓋類型**|
|螺絲|`M2x3`|20|24||
|螺絲|`M2x6`|4|4||
|銅柱|`M2x5` `ø3.05-4mm`|6|8|**矮軸高度**|
|銅柱|`M2x6` `ø3.05-4mm`|4|4|**推薦高度**|
|銅柱|`M2x7` `ø3.05-4mm`|2|2|**有線主控**|
|銅柱|`M2x10` `ø3.05-4mm`|2|2|**無線主控**|
|💡 **TPS43**|||||
|觸控板|-|1|1|**選配**|
|排線|`0.5x5mm` `FFC 6pin` `AA`|1|1||
|排線座|`0.5mm` `FFC 6pin`|1|1|**不限翻蓋類型**|
|螺絲|`M2x3`|20|24||
|螺絲|`M2x6`|4|4||
|銅柱|`M2x5` `ø3.05-4mm`|6|8|-|
|銅柱|`M2x6` `ø3.05-4mm`|4|4||
|銅柱|`M2x8` `ø3.05-4mm`|4|4|**推薦高度**|
|銅柱|`M2x7` `ø3.05-4mm`|2|2|**有線主控**|
|銅柱|`M2x10` `ø3.05-4mm`|2|2|**無線主控**|
|📌 **3D 列印部件**|||||
|`TPS43`|觸控接觸面|1|1||
|`EC-11`|`ø39mm`, `L4.5mm` 編碼器帽蓋|1|1||
|📌 **其他**|||||
|自黏貼 `Poron` 棉|厚度 `2mm`|-|-||
|自黏貼矽膠腳貼|`ø8mm` `0.5-1mm`|4|4||

<br>

## 注意事項

### 零部件方面

- 如要安裝 `EC-11` 編碼器，請選擇支援 `TM040040` 的模組。

- 以上銅柱規格以「矮軸」配置為主；若使用標準 `MX` 軸，高度規格需全面調整至至少 `M2x7mm`。

- 定位板標準 `MX` 軸使用厚度 `1.6mm`；矮軸統一使用厚度 `1.2mm`。

- 鍵軸準備「至少」 `40` 或 `48` 顆以上，類型請根據自身喜好做挑選。

<br>

### 組裝方面

- 電烙鐵屬於高熱量操作技術，務必注意用電用火安全。

- 松香、焊錫屬於高毒性物質，務必注意環境通風。

- 操作時建議佩戴手套、口罩及護目鏡，以確保安全。

- 操作技術推薦：
  - 電烙鐵：恆溫控制攝氏 `250`-`330` 度間。
    - `K`。
    - `BC`。
  - 焊錫：攝氏 `183` 度。
  - 電線：
    - 規格至少 `24`-`28` `AWG`。
    - 顏色至少能清晰分辨 `2` 色。

- 操作工具：
  - 壓線鉗：
    - 需要能夠處理 `PH2.0` 規格。
    - 多準備一些金屬端子備用。
    - 先調整壓力卡楯，再做壓製動作。
  - 剝線鉗：
    - 用於鋰電池及其電量顯示模組使用。

- 觸控板選配：
  - `TM040040`：
    - 需要焊接 `4.7K` 電阻。
    - 需要自行移除觸控板上的 `R1` 電阻。
    - 教學參考：
      - [40mm Cirque GlidePoint Circle Trackpad Module DIY Kit for Split Mechanical Keyboard](https://shop.beekeeb.com/product/40mm-cirque-glidepoint-circle-trackpad-module-diy-kit-for-split-mechanical-keyboard/)
      - [GlidePoint Cirque Trackpad TM040040 TM035035](https://keycapsss.com/keyboard-parts/parts/211/glidepoint-cirque-trackpad-tm040040-tm035035)
  - `TPS43`：不需焊接電阻。

<br>

![](pic/guide/m1.png)

- 關於跳線：
  - 只需要焊接電路板其中一面即可。
  - `PWR`：
    - 一律根據 `MCU` 選擇 `3V3` 供電的位置。
    - 通常 `3V3` 都在 `P1` 的位置上。
  - `VCC`：
    - 使用跳線：組裝 `TM040040` 觸控板時焊上。
    - 不焊跳線：`4.7K` 電阻可不用解下就能將 `TM040040` 更替成 `TPS43`。

<br>

<table>
  <tr>
    <td width="50%">
      <img src="pic/guide/m2.jpg" width="100%" alt="m2">
    </td>
    <td width="50%">
      <img src="pic/guide/m3.jpg" width="100%" alt="m3">
    </td>
  </tr>
</table>

- 關於 `MCU`：
  - 根據 `MCU` 的腳位圖安裝。
  - 參照上圖「有 `2` 個 `GND`」的方向做判讀比較不容易裝反。

<br>

### 圖標辨識

<table>
  <tr>
    <td width="40%">
      <p>二極體有方向性。</p>
      <p>注意二極體前端的標線。</p>
    </td>
    <td width="30%">
      <img src="pic/guide/d1.png" width="100%" alt="d1">
    </td>
    <td width="30%">
      <img src="pic/guide/d2.png" width="100%" alt="d2">
    </td>
  </tr>
  <tr>
    <td width="40%">
      <p>電阻不分方向。</p>
      <p>電阻請跟「MCU」一樣安裝在同一面。</p>
    </td>
    <td width="30%">
      <img src="pic/guide/r1.png" width="100%" alt="r1">
    </td>
    <td width="30%">
      <img src="pic/guide/r2.png" width="100%" alt="r2">
    </td>
  </tr>
</table>

<br>

## 組裝方式

閱覽完前述規範後，進入實體組裝階段時，請特別留意以下具備挑戰性的環節：
- 焊接技術：
  - 溫度控制。
  - `3` 秒內原則。
  - `0.5mm` `FFC` 排線座。
- 除錯技術：
  - `0/1` 原則。
  - 防短路原則。

<br>

<table>
  <tr>
    <td width="38%">
      <img src="pic/info/layout2.png" width="100%" alt="layout2">
    </td>
    <td width="62%">
      <img src="pic/info/layout3.png" width="100%" alt="layout3">
    </td>
  </tr>
</table>

<br>

拿出電路板，然後先思索一下：
  - 配列要長什麼樣子？
  - 鍵盤是有線還是無線驅動？
  - 我能搭載什麼設備？

<img src="pic/guide/g0.jpg" width="100%" alt="g0">

<br>

以上問題思索完畢後，就可以開始進入正式組裝。

> 注意：以前我會傾向要大家先燒錄韌體到主控裡面，但這裡我必須做一些小修正。

<br>

### 共通部分

> 注意事項：部分照片有線及無線版本交互在一起使用，但不會影響安裝。

根據你的理想配列，首先將二極體焊上。

<img src="pic/guide/g1.jpg" width="100%" alt="g1">

<br>

然後將需要用到的週邊元件都焊上：
  - `EC-11` 或觸控板的 `FFC 排線座`、`1/4W 4.7K 電阻`。
  - 跳線。
  - `6x3.5x4.3mm` `2 Pin` 輕觸開關。
  - 無線版本：
    - `MSKT-12D14` 滑動開關。
    - `SKRK` `2mm` 輕觸開關。
    - `PH2.0` 母座。

<img src="pic/guide/g2.jpg" width="100%" alt="g2">

<br>

根據你選擇的 `MCU` 來決定是不是要先燒錄韌體還是先焊上去。
- `RP2040`：
  - 大部分版本先做燒錄動作。
  - `Sea-Picro` 可以先焊接固定好之後再燒錄韌體。
  - 燒錄韌體的方式普遍爲「按住 `BOOT` 按鈕」後接上裝置後將 `uf2` 韌體燒錄到內部。
- `nRF52840`：
  - 先焊接固定好之後再燒錄韌體。
  - 接上裝置後，連續觸發「`2` 次」`RST` 按鈕，再將 `uf2` 韌體燒錄到內部。

<img src="pic/guide/g3.jpg" width="100%" alt="g3">

<br>

`MCU` 焊接固定好、韌體也燒錄進去之後，下一步不要急著把軸體焊上去，你需要做幾件事情：
1. 確認你的鍵盤矩陣有沒有動，也就是你的按鍵有沒有全部都觸發。
  - `VIAL` 的 `Matrix Tester`。
  - `ZMK Studio`——`about-zmk-XXX` 教學文本會說明。
  - 第三方按鍵檢測器：https://hwtest.in/keyboard
2. 確認你的模組有沒有正常驅動。
  - `EC-11` 編碼器。
  - 觸控板的指向裝置韌體操作。

以上都沒問題，如果你是安裝觸控板，記得先把排線拔下來，再焊接軸體；如果是 `EC-11` 編碼器，就直接焊接軸體。

<img src="pic/guide/g4.jpg" width="100%" alt="g4">
<img src="pic/guide/g5.jpg" width="100%" alt="g5">

> 注意事項：`EC-11` 可以不用焊接就直接裸測，我的照片示意圖裡就是這樣的操作。

<br>

### 觸控板

#### TPS43

`TPS43` 的部分，首先需要準備好「觸控面」、「模組 `D`（框架）」，先將 `TPS43` 上頭的 `3M` 膠帶撕下來，跟觸控面一起結合固定好。

<img src="pic/guide/g6.jpg" width="100%" alt="g6">
<img src="pic/guide/g7.jpg" width="100%" alt="g7">

<br>

將銅柱安裝在整套模組上之後，接上排線，然後將主控擋板連同模組一起固定好，就可以安裝底板收尾了。

<img src="pic/guide/g8.jpg" width="100%" alt="g8">
<img src="pic/guide/g9.jpg" width="100%" alt="g9">

<br>

#### TM040040

`TM040040` 的部分，需要將本體跟「模組 `A`、`B`、`C`」先組裝在一起，然後再安裝銅柱完成整套的模組。

<img src="pic/guide/g10.jpg" width="100%" alt="g10">
<img src="pic/guide/g11.jpg" width="100%" alt="g11">
<img src="pic/guide/g12.jpg" width="100%" alt="g12">

<br>

`TM040040` 整套模組的安裝方式，要先將 `5mm` 排線接上，然後穿過洞口。

<img src="pic/guide/g13.jpg" width="100%" alt="g13">

> 最終完成安裝的示意圖會跟「教學」的完全一樣成「`S`」形。

![](https://camo.githubusercontent.com/b15feb1fb0ef73124fbc38e109d9254808041f45607b13747b30ec43c3a2b845/68747470733a2f2f6265656b6565622e636f6d2f747261636b7061642f736964652e6a7067)

<br>

### 關於電池

這裡開始會講述關於無線版本的一個很重要的一些事項：

#### 鋰電池

規格標註為 `501735` `3.7V` 聚合物鋰電池，其尺寸限制以能置於 `ProMicro` 主控上方區塊為準。

<img src="pic/guide/g14.jpg" width="100%" alt="g14">

> 注意事項：你的鋰電池通常拆封之後會做像這樣的絕緣處理，把絕緣的保護拿掉之後，特別注意不要讓這 `2` 根「正負極」碰觸在一起，會導致你的電池純粹地放電在電線上導致最終走火，處理它的時候務必小心。

<br>

首先你需要將 `PH2.0` 的金屬端子先壓制好，一樣確保「正負極」不會互相接觸，然後拿萬用電錶，切到「`20V`」檔位，直接測試「正負極」 查看：
1. 端子有沒有壓制成功。
2. 電池有沒有電。

<img src="pic/guide/g15.jpg" width="100%" alt="g15">

<br>

確認沒問題，電有導通、電池有電之後，就可以將公頭安裝上去。

<img src="pic/guide/g16.jpg" width="100%" alt="g16">

<br>

#### 鋰電池電量顯示模組

`Outsider` 礙於沒有多餘的腳位可以去做 `LCD` 的電量顯示，因此只能「退而求其次」使用硬體操作的顯示模組，這個小東西在網路商城買上非常便宜，而且很好買，也不會缺貨。

<img src="pic/guide/g17.jpg" width="100%" alt="g17">
<img src="pic/guide/g18.jpg" width="100%" alt="g18">

<br>

組裝方式很簡單，顯示模組的上頭有「`+`」、「`-`」標示，分別焊上容易辨識的電線，然後再接到電路板側邊對應位置的接口上。

> 操作及測試方式：
> 1. 滑動開關要先打開，這很重要。
> 2. 然後才能按下側邊的輕觸開關，這個時候顯示模組的電量才會亮起來。

<table>
  <tr>
    <td width="50%">
      <img src="pic/guide/g19.jpg" width="100%" alt="g19">
    </td>
    <td width="50%">
      <img src="pic/guide/g20.jpg" width="100%" alt="g20">
    </td>
  </tr>
</table>

<br>

最後將電池及顯示模組用膠固定好之後就完成安裝了。

<img src="pic/guide/g21.jpg" width="100%" alt="g21">

<br>

#### 底板及收尾

最後一步將銅柱安裝在底板上，然後將底板跟鍵盤主體固定好。

再將鍵帽裝上去就完工了。

<table>
  <tr>
    <td width="50%">
      <img src="pic/guide/e1.jpg" width="100%" alt="e1">
    </td>
    <td width="50%">
      <img src="pic/guide/e2.jpg" width="100%" alt="e2">
    </td>
  </tr>
  <tr>
    <td width="50%">
      <img src="pic/guide/e3.jpg" width="100%" alt="e3">
    </td>
    <td width="50%">
      <img src="pic/outsider/info.jpg" width="100%" alt="info">
    </td>
  </tr>
</table>

<br>

## 參考資料

- 主設計工具：
  - `Google Gemini`  協助。
  - `KiCAD v9.0.0`。
  - `FreeCAD`。
- 參考資料：
  - `Rotray Encoder` `EC10E`。
  - `Rotray Encoder` `EC11`。
  - `Cirque TM040040`。
  - `TPS43`-`TPS65-201A-B`。
  - `ProMicro nrf52840`。
  - `RP2040 ProMicro`。
