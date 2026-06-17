# カードドラフト

シーズンを選び → クラスを選び（入場カード2枚つき）→ 19ラウンドのドラフトでデッキ40枚を組むアプリ。静的サイト（HTML + JS のみ、ビルド不要）。

## 遊び方の流れ

1. **シーズン選択**（例: 1st Anniversary Cup）
2. **クラス選択** … 各クラスの「入場カード2枚」（レジェンド2枚 or レジェンド＋ゴールド）を見て選ぶ。選んだ2枚で開始
3. **ドラフト 19ラウンド** … 毎回 2枚×2セットから片方を選択。途中で最大2回「引き直し」可。合計40枚で完成

## ファイル構成

- `index.html` — アプリ本体
- `seasons/<シーズンID>.js` — シーズンごとのデータ（カード・レアリティ・入場除外）。**ゲームが読むのはここ**
- `rarity-editor.html` — レアリティをクリックで設定するツール
- `cards.js` — レアリティ編集ツール（rarity-editor）の作業用ファイル。**ゲーム本体は読みません**

## シーズンを追加する

1. `seasons/1st-anniversary.js` をコピーして `seasons/<新ID>.js` を作る
2. 中の登録キー（`"1st-anniversary"`）・`name`・`cards`・`offerExclude` を新シーズンの内容に差し替え
3. `index.html` の `<script src="seasons/1st-anniversary.js">` の下に `<script src="seasons/<新ID>.js"></script>` を1行追加

これで既存シーズン（1st Anniversary Cup など）と新シーズンの両方が選べるようになります。

### 新シーズンのレアリティ設定

`rarity-editor.html` でクリック設定 → `cards.js` をダウンロード → その内容を新シーズンファイルの `cards` に反映します。
（`offerExclude`＝入場カードに出さない調整リストは、シーズンファイルに直接記述します）

## ローカルで動かす

`index.html` をブラウザで直接開くだけでも動きます。サーバ経由で確認したい場合:

```bash
cd card-draft-app
python3 -m http.server 8000
# → http://localhost:8000/ を開く
```

## Vercel にデプロイ

ビルド設定不要の静的サイトです。**この `card-draft-app` フォルダをプロジェクトルートとして**デプロイしてください。

### 方法A: Vercel CLI（手軽）

```bash
npm i -g vercel        # 初回のみ
cd card-draft-app
vercel                 # 初回デプロイ（質問にEnterで進めてOK）
vercel --prod          # 本番公開
```

- Framework Preset を聞かれたら **Other**
- Build Command / Output Directory は **空のまま**（静的配信）

### 方法B: GitHub 連携（自動デプロイ）

1. `card-draft-app` を GitHub リポジトリにする（リポジトリ直下に `index.html` が来るように）
2. [vercel.com](https://vercel.com) → New Project → そのリポジトリを Import
3. Framework Preset = **Other**、Build 設定は空のままで Deploy

公開後は発行された URL を共有すれば誰でも使えます。

## データを編集する

カード・レアリティ・入場除外は `seasons/<シーズンID>.js` を書き換えて再デプロイすれば全員に反映されます。
