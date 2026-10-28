# photo-vc-schemas
「報道写真の来歴をVerifiable Credentials(VC)でたどる」で使うVCのスキーマ置き場。VCは Privado ID(iden3)の仕組みで発行・検証。

## 構成

| フォルダ | VC | 状態 |
|---|---|---|
| `capture/` | 撮影VC(CaptureCredential) | v0.1(開発版) |
| `edit/` | 編集VC | 未作成 |
| `check/` | チェックVC | 未作成 |
| `membership/` | 所属証明VC | 未作成 |

各版のフォルダには次のファイルを置きます。

- `<名前>-v<版>.jsonld`: JSON-LDコンテキスト(項目名の意味)
- `<名前>-v<版>.json`: VC全体のJSON Schema(Issuer Nodeに登録するもの)
- その他: 文字列項目の中身を説明するJSON Schema、例のデータ

## URLの使い方(重要)

VCの中には、スキーマとコンテキストのURLがそのまま記録されます。そのため、発行に使うURLは**ブランチ名(main)ではなく、コミット番号で固定したraw URL**にします。

```
https://raw.githubusercontent.com/teruteru7498/photo-vc-schemas/<コミット番号>/capture/v0.1/capture-v0.1.json
```

- 開発版(v0.x)は書き換えることがあります。開発版で発行したVCはテスト用で、使い捨てです。
- 評価で使う版(v1.0以降)は、一度発行に使ったら書き換えません。変更するときは新しい版のフォルダを作ります。
- このリポジトリの名前やアカウント名を変えたり、リポジトリを削除したりすると、発行済みのVCが検証できなくなります。
