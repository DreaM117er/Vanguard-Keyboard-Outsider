# Vanguard Keyboard Outsider 先鋒二號局外者

![info](pic/outsider/info.jpg)

<br>

## 前言

「先鋒系列」是我個人基於「探索者三號」開發上的一個分支點，因爲在探索者三號的設計裡，我將鍵盤主控區域的部分以「模組化」的方式設計，可以將觸控板區域、`ProMicro` 的主控區域徹底沿用至下一把、甚至是下下一把鍵盤上。

- 爲什麼這把鍵盤是二號？

先鋒一號的代號叫做 `Mono`，中文代號叫做「雙子姐妹」，考慮到有人在敲碗 zmk 韌體的開發，因此我另開了一把鍵盤先嘗試研究 zmk，順勢做出一套屬於無線版的韌體，成功之後就會回頭將「一號」的開發完成。

——當然還有「探索者三號」的 `v2` 及無線韌體版本開發。

因爲是基於「探索者系列」，先鋒雖然比較侷限在「模組化」的區域做開發，但我不會捨棄掉「探索者」的自由度。相對的，這份自由反而讓我在開發的時候產生錯亂，算是一個小小的意外。

`Plank` 一直都是我非常喜歡的直列始祖，`3` 號也是基於 `Plank` 開發的鍵盤，我的想法很簡單：

——從最原始的開始，就一定會有更好的創意。

<br>

## 硬體設計

### 自由度及限制

`Outsider` 被我設計成這種奇怪的樣子，其中最主要的原因就是「自由度」的保留，那麼它的自由度在哪裡？答案很簡單——

——它可以「任意」翻轉使用。

然後它是基於探索者三號的延伸設計，也就是說它的主控區域是沿用下來的，使用材料的部分會註明直接從「三號」那裡直接抓取設計圖。

<br>

太極理論有「物極必反」的核心概念，任何的設計上也一樣，這把鍵盤給予了極致的自由度，它「目前」的限制列在下方：
1. 支援焊接。
2. 不支援 `Gateron Low Profile 2.0`，也就是 `GLP2.0`。
3. 觸控板只支援 QMK 有線版。

<br>

自由度的部分依據「限制」，列在下方：
1. 支援 `MX` 腳位的「鍵軸」。
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
    <td width="50%">
      <img src="pic/outsider/A.png" width="100%" alt="A">
    </td>
    <td width="50%">
      <img src="pic/outsider/B.png" width="100%" alt="B">
    </td>
  </tr>
</table>

<br>

#### 定位板及底板

<table>
  <tr>
    <td width="50%">
      <img src="pic/outsider/B1.png" width="100%" alt="B1">
    </td>
    <td width="50%">
      <img src="pic/outsider/P1.png" width="100%" alt="P1">
    </td>
  </tr>
  <tr>
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
      <img src="pic/outsider/M.png" width="100%" alt="M">
    </td>
    <td width="50%">
      <img src="pic/outsider/T1.png" width="100%" alt="T1">
    </td>
  </tr>
  <tr>
    <td width="50%">
      <img src="pic/outsider/T2.png" width="100%" alt="T2">
    </td>
    <td width="50%">
      <img src="pic/outsider/T3.png" width="100%" alt="T3">
    </td>
  </tr>
  <tr>
    <td width="50%">
      <img src="pic/outsider/T4.png" width="100%" alt="T4">
    </td>
    <td width="50%">
      <img src="pic/outsider/T5.png" width="100%" alt="T5">
    </td>
  </tr>
</table>

<br>

## 使用材料




















<br>

## 注意事項

















<br>

## 組裝方式

























<br>

## 後記















