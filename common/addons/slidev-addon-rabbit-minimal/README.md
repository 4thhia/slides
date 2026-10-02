# slidev-addon-rabbit-minimal v3

全体の持ち時間を指定し, 個別指定のないスライドに残り時間を均等配分する Slidev addon です。

## 更新

以前の `slidev-addon-rabbit-minimal` フォルダーを今回のフォルダーで上書きし, Slidev を再起動します。click-memory はそのまま併用できます。

## 設定

先頭の frontmatter に全体の時間を秒単位で指定します。以下は 20 分の例です。表紙を時間配分に含めない場合は `duration: 0` を明示してください。

```yaml
---
theme: ../common/themes/neat
addons:
  - '@/../common/addons/slidev-addon-click-memory'
  - '@/../common/addons/slidev-addon-rabbit-minimal'
routerMode: hash
layout: cover
colorSchema: light
duration: 0
rabbit:
  totalDuration: 1200
---
```

既存の coverTitle / coverAuthor 等はそのまま残してください。プロジェクト直下に addon を置く場合は `./slidev-addon-rabbit-minimal` を指定します。追加の npm パッケージは不要です。

個別に時間を確保したいスライドだけ `duration` を追加します。

```yaml
---
layout: default
headerEnable: true
headerTitle: Background
pageNumber: true
duration: 90
---
```

均等配分に任せるスライドでは `duration` の行を省略してください。`duration: 0` は省略と異なり, 時間配分からの明示的な除外です。付録などにも使えます。

## 配分規則

未指定のスライド 1 枚の予定時間は:

`(totalDuration − 個別指定された duration の合計) / duration 未指定のスライド数`

- 全体指定だけなら, 表紙を含む全スライドに均等配分します。
- 表紙を除外したい場合は, 表紙に `duration: 0` を書いてください（v2 の自動除外から変更）。
- 全体 1200 秒, 個別指定の合計 300 秒, 未指定が 10 枚なら, 未指定は各 90 秒です。
- 全スライドが個別指定済みで合計が全体時間より短い場合は, 右端に未割当の時間を残します。個別指定の長さを勝手に変更しません。
- 個別指定の合計が全体時間を超える場合は表示と計時を停止し, ブラウザーのコンソールに設定エラーを出します。
- 全体時間は必須かつ正の数です。個別時間は 0 以上の数を指定します。空欄・負数・数値でない値は設定エラーです。
- `rabbit.defaultDuration` と URL の `?time=10` は使用しません。

## 表示と操作

下端の薄い線が全体時間, 少し太い灰色の区間が現在のスライドの予定開始〜終了時刻, 縦線が実際の経過時間です。縦線が区間内なら予定の時間帯, 区間より左なら先行, 右なら遅延です。

- 0 秒の表紙では待機し, 正の予定時間を持つスライドを開くと計時開始します。
- 表紙にも正の時間が配分されている場合は, 表紙を開いた時点で開始します。
- 前後のスライド移動やクリック段階の変更では時計をリセットしません。
- 0 秒の表紙に戻るとリセットします。再読み込みでもリセットします。
- 途中のページを直接開くと, その時点を経過 0 秒として開始します。
- 0 秒の中間ページでは予定区間を表示しません。開始済みの時計は継続します。
- 全体時間に達した縦線は右端で停止します。色変更や点滅はありません。
- 全体時間や配分を変更すると時計をリセットします。
- 各ブラウザー画面の時計は独立しており, 発表者画面との同期は行いません。
- 印刷時は非表示です。クリック操作を妨げません。

## 見た目の調整

```yaml
rabbit:
  totalDuration: 1200
  opacity: 0.45
  color: '#808080'
  bottom: 8
  inset: 24
  slideNum: false
  enabled: true
```

`opacity: 0.3` で薄く, `opacity: 0.65` で濃くできます。位置は Slidev キャンバス上の px 単位です。

## 検証

Slidev 0.49.29 の hash router 設定で production build を確認。均等配分・個別指定との混在・0 秒指定・未割当時間・時間超過エラー・タイマー処理をコード上で検証しました。ブラウザーでの描画と, ユーザーの neat / click-memory の実ファイルとの組み合わせは未検証です。

## ライセンス

[kaakaa/slidev-addon-rabbit](https://github.com/kaakaa/slidev-addon-rabbit) に基づく MIT ライセンスの改変版です。LICENSE に元の著作権表示を保持しています。動物アイコンや Iconify の依存はありません。
