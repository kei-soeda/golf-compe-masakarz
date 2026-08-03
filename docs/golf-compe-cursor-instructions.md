# 【大会名】コンペサイト — Cursor 作業指示書

## 0. 前提

- 対象ファイルは `golf-compe-masters.html` の1枚（HTML／CSS／JS すべて内包した単一の静的ファイル）。
- 外部依存は Google Fonts（Cormorant Garamond / Noto Serif JP）のみ。ビルド不要。ブラウザで開けばそのまま表示される。
- デザインは全米マスターズ調（オーガスタ・グリーン／ゴールド／クリーム）。参考サイトは `okusuri2026.shotty.net` と同一構成。
- 差し替えが必要な箇所はすべて全角の墨付き括弧 `【　】` で囲ってある。**まず `【` を検索して未差し替え箇所を洗い出すこと。**

---

## 1. セットアップ

1. `golf-compe-masters.html` をリポジトリ直下に配置し、公開用に `index.html` へリネームする。
2. 画像用に `images/` フォルダを作成する。
3. ローカル確認は VS Code / Cursor の Live Server 等でプレビュー、またはブラウザに直接ドラッグして表示する。

---

## 2. テキストの差し替え（`【　】` 一括対応）

`【` で全文検索し、以下を実データに置換する。同じ内容が複数箇所（ヘッダー・ヒーロー・フッター・meta 等）に出るため、**該当する全箇所を漏れなく置換すること。**

| 種別 | プレースホルダー例 | 備考 |
|---|---|---|
| 大会名 | `【大会名】` | title / ヘッダー / ヒーロー / フッター / お知らせ / エントリー / コピーライト |
| 開催日 | `2026年【　月　日（曜）】` | 表記ゆれに注意。全箇所を同一表記に統一 |
| 会場 | `【ゴルフ場名・コース名】` `【住所】` `【最寄IC】` | 開催概要・会場・フッター・meta |
| スタート | `【8:00】` `【IN】` `【8:00 ／ 8:07 ／ 8:15 ／ 8:22】` | 組時間・組数と整合させる |
| 定員 | `【16名（4組）】` `【12〜16名（4組）】` | 組数と人数を整合 |
| 参加費 | `【11,590円】` `【7,590円】` `【4,000円】` | プレー代＋会費の合算が合計と一致するか確認 |
| コース情報 | `Par【72】` `Yards【6,148】` `Holes【18】` | |
| エントリー | `【LINE URL】` | 公式LINEの友だち追加URL（`https://lin.ee/xxxx`） |
| 事務局 | `【mail@example.com】` `【主催者名】` | フッター |
| お知らせ | `2026.【07.24】` | 掲載日 |

- `title` と `meta[name="description"]` の `【　】` も忘れず更新する。
- 「開催日」は全角/半角・曜日表記を全ページで揃えること（不揃いだと機械的な印象になる）。

---

## 3. 画像の差し替え

現状、写真部分は緑グラデーションのプレースホルダー（`div`）で組んである。実写真は `images/` に置き、下記の要領で `img` に置換する。

### 3-1. 置換の基本形

**Before（例：会場写真 `.venue-photo`）**
```html
<div class="venue-photo">
  <div class="photo-ph">Course Photo<span>【コース全景写真を差し替え】</span></div>
</div>
```

**After**
```html
<div class="venue-photo">
  <img src="images/venue.jpg" alt="【コース名】のコース全景"
       style="width:100%;height:100%;object-fit:cover;">
</div>
```

- 枠（`.venue-photo` / `.gal-item` / `.recap-photo` / `.recap-strip .s`）はそのまま残し、中の `.photo-ph` や `.lab` を `img` に置き換える。
- `object-fit:cover;` を必ず付ける（枠の比率に合わせてトリミング表示される）。
- `alt` には内容を日本語で簡潔に記載する。

### 3-2. ヒーロー背景に写真を敷く場合

`.hero` の `background` を、可読性確保のためグリーンのオーバーレイ＋写真の二層にする。

```css
.hero{
  background:
    linear-gradient(180deg, rgba(7,54,36,.72), rgba(5,36,25,.86)),
    url('images/hero.jpg') center/cover no-repeat;
  /* 既存の min-height / display 等はそのまま */
}
```
※ `.hero::before` の斜線テクスチャは残してよい。文字が読みにくければオーバーレイの不透明度を上げる。

### 3-3. 差し替え対象と推奨サイズ

| 箇所 | セレクタ | 比率 | 推奨サイズ | 形式 |
|---|---|---|---|---|
| ヒーロー背景 | `.hero`（CSS） | 16:9 | 1920×1080 以上 | jpg / webp |
| 会場写真 | `.venue-photo` | 4:3 | 1200×900 | jpg / webp |
| ギャラリー ×6 | `.gal-item` | 1:1 | 1000×1000 | jpg / webp |
| レポート集合写真 | `.recap-photo` | 3:2 | 1200×800 | jpg / webp |
| レポートのシーン ×4 | `.recap-strip .s` | 1:1 | 800×800 | jpg / webp |
| LINE QRコード | `.qr` | 1:1 | 400×400 | png |
| ロゴ | ヘッダー/フッターの inline `<svg class="crest">` | — | — | 現状は独自SVGエンブレム。差し替え不要 |

- 容量削減のため可能なら `webp` を優先。全体で軽く保つ。

### 3-4. OGP画像（SNSシェア用）

現状 `head` は `title` と `description` のみ。SNS共有カードを出すなら以下を `head` に追加し、`images/ogp.jpg`（1200×630）を用意する。

```html
<meta property="og:title" content="【大会名】 2026">
<meta property="og:description" content="2026年【月日（曜）】【ゴルフ場名・コース名】にて開催。参加者募集中。">
<meta property="og:image" content="https://【サブドメイン】.shotty.net/images/ogp.jpg">
<meta property="og:type" content="website">
<meta property="og:locale" content="ja_JP">
<meta name="twitter:card" content="summary_large_image">
```

---

## 4. よくある修正の指示テンプレート

Cursor へは下記のように具体的に依頼する。

### 4-1. 歴代王者の行を増減する
`#champions` の `.roll` 内、`.roll-row` を回数分だけ増減する。1行の雛形：
```html
<div class="roll-row"><span class="ed">第○回</span><span class="nm">【氏名】<small>20XX</small></span><span class="sc">Net 【00.0】</span></div>
```
- 最上部の「Defending Champion」ブロック（`.defending`）は最新回の優勝者に合わせる。

### 4-2. 開催レポートの入賞者・本文を変更する
`#recap` 内の `.recap-results` の各 `.r`（優勝／準優勝／第3位／ドラコン／ニアピン）と、`.recap-info p` の本文を差し替える。回次見出し `第15回コンペの様子` も必要に応じて更新する。

### 4-3. 配色を変える
`:root` のCSS変数を編集すれば全体に反映される。主要変数：
- `--green-deep` / `--green` / `--green-dark`：ベースの緑
- `--gold` / `--gold-soft`：アクセント金
- `--cream` / `--ivory`：明背景
※ 個別要素の色を直接いじらず、まず変数で調整すること。

### 4-4. セクションを追加・削除・並び替える
- 各セクションは `<section class="sec sec-xxx" id="yyy">` 単位。背景クラスは `sec-cream` / `sec-ivory` / `sec-green` の3種。
- **緑背景と明背景が交互になるよう配置する**（連続すると単調になる）。
- 追加・削除したらヘッダー `#menu` とフッター `Menu` のアンカーリンクも同時に更新する。

---

## 5. 公開（デプロイ）

- `okusuri2026.shotty.net` と同じ要領で、新しいサブドメイン配下に `index.html` と `images/` を配置する。
- 公開前チェック：
  1. `【` が1つも残っていない（全プレースホルダー差し替え済み）
  2. 画像リンク切れがない（`images/` のパス整合）
  3. `title` / `description` / OGP が実データになっている
  4. エントリーの `【LINE URL】` が正しい友だち追加URLになっている
  5. スマホ幅で表示崩れがない

---

## 6. 注意事項

- **マスターズの実ロゴ・商標（米国地図＋旗竿の意匠、"Masters" の名称）は使用しない。** 雰囲気のみ踏襲し、ロゴは同梱の独自SVGエンブレムを維持する。
- テキストの「開催日」「参加費」「定員」「組時間」は複数箇所に散らばるため、変更時は必ず全箇所を整合させる。
- Google Fonts をオフライン/自ホストに切り替えたい場合は、`head` の `<link>` を差し替え、フォントファイルを `fonts/` に置いて `@font-face` で読み込む。
