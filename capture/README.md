# 撮影VC(CaptureCredential)の語彙

撮影アプリで写真を撮るときに、研究室のIssuer Nodeが撮影者に発行するVCです。このページは、JSON-LDコンテキストの項目名(`pvc-capture:…`)の説明を兼ねています。

撮影者は撮影VCを自分の端末に保管し、他の人には渡しません。他の人(編集者・チェック担当者)には、撮影VCから作った**開示証明**(Privado IDのZKPを使った提示)を渡します。

## credentialSubject の項目

| 項目 | 型 | 開示 | 内容 |
|---|---|---|---|
| `id` | DID | — | 撮影者(受け取る人)のDID |
| <a id="capture_record"></a>`capture_record` | 文字列 | 必ず見せる | まとめ項目。下の「まとめ項目の中身」をJCS(RFC 8785)で正規化したJSON文字列 |
| <a id="loc_L1"></a>`loc_L1` | 文字列 | 撮影者が選ぶ | 国 |
| <a id="loc_L2"></a>`loc_L2` | 文字列 | 撮影者が選ぶ | 国+都道府県(日本のみ) |
| <a id="loc_L3"></a>`loc_L3` | 文字列 | 撮影者が選ぶ | 国+都道府県+市区町村(日本のみ) |
| <a id="loc_L4"></a>`loc_L4` | 文字列 | 撮影者が選ぶ | それより粗い情報+約1kmに丸めた座標 |
| <a id="loc_L5"></a>`loc_L5` | 文字列 | 撮影者が選ぶ | それより粗い情報+精密な座標 |

位置の項目は積み上げ型です。どのレベルの項目も、それより粗いレベルの情報をすべて含みます。そのため、開示証明で見せるのは「まとめ項目」と「選んだレベルの位置の項目1つ」の2つになります。

作れないレベルの項目は作りません。

- 測位できなかったとき: 位置の項目はなし
- 誤差が1000mを超えるとき: L4・L5なし
- 海外のとき: L2・L3なし

## まとめ項目の中身

形は `v0.1/capture-record-v0.1.schema.json` で定義しています。例は `v0.1/examples/capture-record.example.json` にあります。

- `basic_metadata`: 撮影日時とその出所、位置の取得方法、端末、撮影設定、画像の情報
- `integrity`
  - この撮影VCの識別番号と失効番号: 現在も有効かを問い合わせるために使います
  - 画像のSHA-256
  - 保証レベル
  - チェーン内の位置
- `photographer_intent`: 撮影者の自己申告(撮影意図・1枚ごとの補足・用途・同意の状況など)。`self_attested: true`

## 検証するときの確認点

1. 開示証明(ZKP)が正しく、発行者が研究室のIssuer Nodeであること
2. `capture_record` を取り出し、JSON Schemaに合っていること
3. `photographer_intent.photographer_id` が、開示証明を作った撮影者のDIDと一致すること
4. `integrity.credential_id` と `integrity.revocation_nonce` を使ってIssuer Nodeに問い合わせ、撮影VCが現在も有効であること
5. 手元の画像ファイルのSHA-256が `integrity.content_hash.value` と一致すること

## 版の履歴

- v0.1(開発版): 最初の版
