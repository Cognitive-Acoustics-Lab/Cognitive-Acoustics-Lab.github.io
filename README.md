# 認知音響学研究室 Web サイト

長崎県立大学 情報システム学部 認知音響学研究室（Cognitive Acoustics Lab.、中研究室）の公式サイト。
GitHub Pages で公開中（https://cognitive-acoustics-lab.github.io/ ）。HTML と CSS だけで構成しています。

## 雛形の方針

| 項目 | 方針 |
| --- | --- |
| 技術 | 純粋な HTML + CSS。ビルド工程・JS フレームワークなし。モバイルメニューも CSS のみ（チェックボックス方式） |
| 構成 | マルチページ。各ページが同じヘッダー・フッターを持つ |
| デザイン | ファミコン画面の「作法」だけを借りた 8bit 調。無彩色の地 + 青緑 2 階調 + 墨色（アクセント色なし）、箱は塗りのみで枠線なし（`--box-bw: 0`）、区切り線は 1px、影なし、アニメーションなし、具象イラストなし。見出しは黒い箱 1 つ（英語の添え字なし）。フォントはドット書体ではなく丸ゴシック（検討の経緯は `_drafts/font-compare*.html`） |
| デザイン調整 | 色・フォント・余白は `assets/css/style.css` 冒頭の `:root` に集約。ここを変えれば全ページに反映 |
| 旧デザイン | `assets/css/style-classic.css` に退避。各 HTML の `<link rel="stylesheet">` を差し替えれば戻せる |
| 言語 | 日本語が主。英語は見出しの添え字（eyebrow）に使用。将来 `/en/` を足せる構造 |
| 画像 | ヒーローの「ファミコン画面風の額」に写真を入れる想定（下記）。メンバーは頭文字タイル。写真が揃ったら差し替え |
| フォント | Google Fonts の Zen Maru Gothic（見出し・ラベル）＋ Zen Kaku Gothic New（本文）。`--font-head` / `--font-body` で切り替え |

### 配色（`:root` のトークン）

| トークン | 値 | 用途 |
| --- | --- | --- |
| `--ink` | `#22332f` | 文字・見出しの箱・フッターの地・区切り線 |
| `--paper` | `#f5f5f3` | ページ背景（無彩色） |
| `--g0` `--g1` `--g2` | `#e9eae7` `#d2d4d0` `#9ea49f` | 無彩色の淡色（タグ・番号・アイコンの地・点線・補足文字） |
| `--g3` `--g4` | `#577a74` `#2f4d49` | 青緑。g3 はお知らせ窓・額の縁・リンク，g4 はトップのヒーローの地 |
| `--news-line` `--news-time` | `#bfd0ca` `#e3ebe7` | お知らせ窓の中だけ元の淡い青緑を保つ |
| `--bezel-light` `--bezel-dark` | `#4e625c` `#161f1c` | ヒーローの額（ベゼル）。上・左が明，下・右が暗の立体ベベル。厚さ 10px，四隅は 3 段・3px の階段（`.crt__bezel` の clip-path） |
| `--hero` | `#ececea` | 下層ページの見出し帯の背景（トップのヒーローは g4 の暗い地＋まばらな小さい正方形の固定 SVG。`style.css` の `.hero` を参照。再生成は seed=11 の乱数で 420 個） |

2026-10-28 に「緑が多すぎる」との判断で，淡い緑を無彩色に，フッターを墨色に変更。緑の面はヒーローとお知らせ窓の 2 つ。アクセント色は検討の末，置かないことにした（三角・ゲージ・下線は墨色か白）。

### ヒーローの写真

- 表示に使うのは `assets/img/hero-1600.jpg`（横 1600px、約 0.5 MB）。原寸の `hero.jpg`（4032×3024、2.6 MB）はローカルにだけ置き、`.gitignore` でリポジトリから除外している。
- 写真を差し替えるときは、原寸を `hero.jpg` に置き、`sips -s format jpeg -s formatOptions 82 -Z 1600 assets/img/hero.jpg --out assets/img/hero-1600.jpg` で縮小版を作り直す（`index.html` と `en/index.html` が参照）。
- 額の下端の文言は `<div class="crt__hud">` 内で変更できる。

### タブのアイコン（favicon）

- 元画像は `assets/img/favicon.png`（正方形に近い写真。ローカルのみ、リポジトリ除外）。表示に使うのは `favicon-16.png`・`favicon-32.png`（タブ）と `favicon-180.png`（iOS ホーム画面）。全ページの `<head>` の `<link rel="icon">` と `<link rel="apple-touch-icon">` が参照する。
- 差し替えるときは元画像を `favicon.png` に置き、`sips -c 736 736 assets/img/favicon.png --out /tmp/sq.png` で正方形にしてから `sips -z 32 32 /tmp/sq.png --out assets/img/favicon-32.png`（16, 180 も同様）。
- ルートの `favicon.ico` は 16/32 の PNG を束ねたもので、古いブラウザや `/favicon.ico` を直接取りに来るツール向け。作り直すなら ImageMagick で `magick favicon-16.png favicon-32.png favicon.ico`。

### フッターのステータス表示

`<div class="hud">` の MEMBERS の人数と SINCE の年は手書き。メンバーが増減したら全ページの数字を揃えて直す（検索置換で可）。

## ファイル構成

```
HP作成/
├── index.html          トップ（ヒーロー・お知らせ・研究テーマ・研究室について）
├── research.html       研究内容（3テーマの詳細・手法・共同研究）
├── members.html        メンバー（教員＋学部生）
├── profile.html        教員の経歴・資金・受賞（members から遷移）
├── publications.html   業績（論文・学会発表・受賞）
├── for-students.html   配属希望の方へ（進め方・年間行事・関連学会）
├── access.html         アクセス・連絡先
├── en/                 英語版（index, research, members, profile, publications, access）
├── assets/
│   ├── css/style.css   共通スタイル（デザイントークンはここ）
│   ├── css/style-classic.css   旧デザイン（退避）
│   └── img/            hero-1600.jpg（トップの写真）、favicon-16/32/180.png（タブ・ホーム画面のアイコン）、members/（メンバー写真 400×400）
├── _drafts/                デザイン検討に使ったモックとフォント比較ページ（公開前に削除してよい）
├── .nojekyll           GitHub Pages の Jekyll 処理を無効化
├── favicon.ico         互換用アイコン（favicon-16/32.png を束ねたもの）
├── .gitignore          _drafts/・原寸写真（hero.jpg, favicon.png, members/naka.JPG）・.DS_Store・HANDOFF.md を除外
├── HANDOFF.md          引き継ぎメモ（ローカルのみ、公開しない）
└── README.md
```

## 英語版（`en/`）

- `en/` に英語版 5 ページ（index, research, members, publications, access）と `profile.html`。「配属希望の方へ」は英語版なし。CSS・画像は共通（`../assets/...`）。
- ヘッダー右端の「EN」「日本語」で相互に行き来する。`for-students.html` の EN は `en/index.html` へ。
- 日本語版が正。英語版は論文・メンバーなど構造が変わるものだけ追従し，お知らせは主要項目（論文掲載・資金・開設など）のみ。業績の各項目は原語のまま，見出しだけ英語。
- 日英ペアの 12 ページには `<head>` に `<link rel="alternate" hreflang="ja|en|x-default" href="絶対URL">` を入れてある（2026-10-29）。ページを増やしたら同じ形式で追加する（`for-students.html` は英語版がないので無し）。

## 更新のしかた

- **連絡先**：研究室宛（配属相談・見学・一般の問い合わせ）は `cognitive.acoustics.lab [at] gmail.com` を `access.html` にだけ載せる。教員個人宛は `kiichinaka [at] sun.ac.jp` を `profile.html` にだけ載せる。他のページには書かず，リンクで誘導する。表記は `xxx [at] yyy`，`[at]` の注記は `access.html` のみ，mailto リンクは使わない。

- **お知らせ**：`index.html` の `<ul class="news">` に `<li>` を先頭追加。
- **メンバー**：`members.html` の `<ul class="members">` に `<li class="member">` を追加。学年ごとに姓の五十音順で並べる（見出しは学年のみ。配属年度は書かない）。写真は `assets/img/members/<ローマ字姓>.jpg`（400×400px、正方形）に置き，`<div class="avatar">` の中を `<img src="assets/img/members/xxx.jpg" alt="">` にする。元の大きな写真はリポジトリに入れない。正方形への切り抜きは `ffmpeg -i 元.jpg -vf "crop=min(iw\,ih):min(iw\,ih),scale=400:400" -q:v 3 出力.jpg`。**出力は必ず元と別の名前**（例: `naka-sq.jpg`）にする。Mac は大文字小文字を区別しないので `naka.JPG` と `naka.jpg` は同じファイルになり，上書き・削除事故の原因になる。元ファイルの削除は出力を確認してから別途行う。
- **業績**：`publications.html` の各 `<ol class="pubs" reversed>` の先頭に `<li>` を追加。研究室メンバー（教員・学生）の名前は `<strong>` で囲む（凡例「太字は研究室メンバー」に対応）。サブナビの件数も更新する。
- **ヘッダー・フッター**：全ページに同じ HTML が入っているので、変更時は全ファイルを揃えて修正する（検索置換で可）。ページ数が増えて手間になったら Jekyll の `_includes` に切り替えるのが次の手。

## ローカルで確認

ファイルをブラウザで直接開くだけで表示できます。
ルート相対パスを使っていないので、`file://` でも GitHub Pages のサブパスでも動きます。

## 公開先（2026-10-29 公開）

| 項目 | 値 |
| --- | --- |
| 公開 URL | https://cognitive-acoustics-lab.github.io/ （英語版は `/en/`） |
| リポジトリ | https://github.com/Cognitive-Acoustics-Lab/Cognitive-Acoustics-Lab.github.io （組織 Cognitive-Acoustics-Lab、公開） |
| Pages の設定 | Deploy from a branch、`main` / `(root)`。`<組織名>.github.io` という名前なのでルート URL で配信 |
| git 管理 | この `HP作成/` フォルダがそのままリポジトリ。`.gitignore` で `_drafts/`・`.DS_Store`・原寸写真（`hero.jpg`, `members/naka.JPG`）・`HANDOFF.md` を除外（ローカルには残る） |

### 更新を反映する

ファイルを編集したら、このフォルダで次を実行する。1 分ほどで公開に反映される。

```
git add -A
git commit -m "お知らせを追加"   # 内容に合わせて
git push
```

- 公開前にローカルで見るなら `python3 -m http.server 8000` → http://localhost:8000
- 反映されないときは GitHub のリポジトリ → Actions タブ（pages-build-deployment）でビルド結果を確認する。
- 独自ドメインにする場合は `CNAME` ファイルを追加し、各ページの hreflang の URL を書き換える。
## 要確認・未記入（プロトタイプ段階の仮置き）

ページ内の黄色いラベル「要確認」「要記入」が目印です。

- 研究テーマ 3 本の説明文（業績から起こした草案）
- 研究室について／配属希望の方へ の文章（Notion「はじめに」から要約した草案）
- 住所（シーボルト校の住所を仮置き）、研究室の棟・部屋番号、交通案内
- 実験室 W303・W401 の記載の要否
- Notion の Members にある出身地・趣味は公開していない（本人同意があれば追加可）
- 競争的資金の金額は非掲載にした（旧サイトには記載あり）
