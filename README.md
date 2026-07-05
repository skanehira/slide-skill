# SLIDE.md

**AIとスライドを作るための設計書フォーマットと、それを生成するClaude Codeスキルパッケージ。**

![アイキャッチ](docs/slide-sample-01.png)

**→ <a href="https://sho-ai-magic.github.io/slide.md/lp.html" target="_blank">ランディングページを見る</a>**

[Google DESIGN.md](https://stitch.withgoogle.com/docs/design-md/overview) のコンセプトに着想を得た、スライドに特化した設計書フォーマット「SLIDE.md」の仕様と、それを自動生成するClaude Codeスキルを公開しています。

---

## このリポジトリについて（fork版）

本リポジトリは [sho-ai-magic/slide.md](https://github.com/sho-ai-magic/slide.md) の fork です。オリジナルの SLIDE.md フォーマット・スキル・デザインシステム・パターン群はすべて sho-ai-magic 氏によるものです。

### fork で加えた改善点

- **Claude Code プラグインとして配布可能にした**：`.claude-plugin/plugin.json` と `marketplace.json` を追加し、`/plugin marketplace add skanehira/slide-plugin` → `/plugin install slide-plugin` の2コマンドでインストールできるようにしました。
- **同梱ファイルへのフォールバックを追加**：`slide-deck-builder` 実行時、カレントディレクトリにデザインシステム・パターンが無い場合はプラグイン同梱の `docs/SLIDE-md/` ・ `docs/SLIDE-PATTERN/` を自動参照します。デザインシステムは選択した1つをカスタマイズ起点としてプロジェクトへコピーし、パターンは読み取り専用の共有資産として同梱ファイルを直接参照します。
- **README のインストール手順をプラグイン方式に一本化**しました。

---

## SLIDE.md とは

AIツール（Claude Design、NotebookLM、Google Slides等）にスライドを生成させるとき、毎回「色はこれ、フォントはこれ、レイアウトはこう」と説明するのは手間がかかります。また、同じAIに何度頼んでもデザインがバラバラになりがちです。

**SLIDE.mdは、そのデザイン指示を「設計書ファイル」として一度定義し、使い回すためのフォーマットです。**

### 3種類のファイルで構成される

| ファイル                  | 役割                                                                                                                                                               |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `SLIDE.md`                | デザインシステム定義。色・フォント・余白・タイトルエリア・ページ番号などを定義する。                                                                               |
| `SLIDE-PATTERN-{name}.md` | レイアウトパターン定義。コンテンツエリアの構造（カラム数・要素の配置）を定義する。                                                                                 |
| `SLIDE-DECK.md`           | スライド設計書。上記2ファイルの内容をすべて埋め込み、スライド構成とコンテンツひな型をまとめた1枚のブリーフ。AIツールにこのファイルだけ渡せばスライドが生成できる。 |

### 設計のポイント

**デザインと構造を分離する**：`SLIDE.md` がブランドの見た目（色・フォント）とフレーム（タイトルエリア・ページ番号）を一括管理し、`SLIDE-PATTERN-*.md` はコンテンツエリアのレイアウト構造だけを定義します。これにより、同じパターンを異なるデザインシステムに使い回せます。

**1ファイルで完結する**：`SLIDE-DECK.md` にはデザインとパターンの定義がすべて埋め込まれます。AIツールに渡すファイルは1枚だけで済みます。

---

## フォーマット仕様

### SLIDE.md

デザインシステムの定義ファイル。色・フォント・レイアウト・スライドフレームをまとめて定義します。

```markdown
# SLIDE.md

## Overview
**参照ソース：** （参考にしたWebサイト・ブランドガイドライン等）
**マッチするシーン：** （このデザインが合う用途・場面）

## Colors
| 役割 | カラー名 | HEXコード |
|---|---|---|
| Primary | （色名） | #XXXXXX |
| Secondary | （色名） | #XXXXXX |
| Background | （色名） | #XXXXXX |
| Accent | （色名） | #XXXXXX |
| Text | （色名） | #XXXXXX |
| Text Sub | （色名） | #XXXXXX |
| Text Muted | （色名） | #XXXXXX |

**グラデーション：** Primary → Secondary → Accent・90deg（横）など / なし

## Typography
| 役割 | フォント | サイズ | ウェイト |
|---|---|---|---|
| 見出し（H1） | （フォント名） | 48px | Bold |
| 見出し（H2） | （フォント名） | 32px | Bold |
| 本文 | （フォント名） | 18px | Regular |
| キャプション | （フォント名） | 14px | Regular |

## Layout
- **スライドサイズ：** 16:9（1920 × 1080 px）
- **余白（上下）：** （px）
- **余白（左右）：** （px）
- **テキスト整列：** 左寄せ

## Slide Frame
（タイトルエリア・ページ番号・ロゴ・フッター装飾の定義）

## Do / Don't
（このデザインでやること・やってはいけないことの一覧）
```

### SLIDE-PATTERN-{name}.md

レイアウトパターンの定義ファイル。コンテンツエリアの構造のみを定義し、色・フォントは含みません。

```markdown
# SLIDE-PATTERN-{name}

## Overview
**パターン名：** （英小文字・ハイフン区切り）
**概要：** （このパターンの説明）
**適したシーン：** （使いどころ）

## Structure（構造）
（コンテンツエリアのレイアウト構造をYAMLで記述）

## Elements（各要素の役割）
（各UI要素の役割と推奨文字数の一覧）

## Usage Guide（AIへの使い方）
（AIへのプロンプト例と注意点）
```

### SLIDE-DECK.md

AIツールに渡す最終的な設計書ファイル。`SLIDE.md` と使用する `SLIDE-PATTERN-*.md` の内容をすべて埋め込み、スライド1枚ごとのコンテンツひな型を記述します。このファイル1枚をAIツールに渡すだけでスライドが生成できます。

```markdown
# SLIDE-DECK-{name}

## Deck Info
（タイトル・対象者・目的・枚数）

## Design System
（SLIDE.md の全内容を埋め込み）

## Slide Patterns
（使用するSLIDE-PATTERN-*.md の全内容を埋め込み）

## Slides
### Slide 1 — 表紙
（パターン指定 + コンテンツひな型 + AIへの指示）

### Slide 2 — …
（スライド枚数分くり返す）
```

---

## このリポジトリのスキルについて

SLIDE.mdフォーマットを手作業で書くのは難しいため、**Claude Code上で動作する3つのスキルを開発しました。** スライドの画像やプレゼン内容を渡すだけで、AIが自動でSLIDE.mdファイル群を生成します。

### スキルでできること

1. **既存のスライド・Webサイトのデザインを解析**して、AIが読めるデザイン定義ファイル（SLIDE.md）を自動生成
2. **スライドのレイアウト構造を抽出**して、再利用可能なパターン定義ファイル（SLIDE-PATTERN-\*.md）を生成
3. **プレゼン内容を入力**すると、デザイン＋パターン＋スライド構成をまとめた設計書（SLIDE-DECK.md）を自動生成

SLIDE-DECK.mdをClaude DesignなどのAIツールに渡すだけで、デザインの一貫したスライドが生成できます。

### スキル一覧

| スキル                  | できること                                          | 出力ファイル                                             |
| ----------------------- | --------------------------------------------------- | -------------------------------------------------------- |
| `slide-md-creator`      | スライド・画像・Webサイトからデザインシステムを生成 | `SLIDE-md-{name}/SLIDE.md` + `sample.html`               |
| `slide-pattern-creator` | スライドのレイアウト構造を解析してパターンを定義    | `SLIDE-PATTERN-{name}/SLIDE-PATTERN-{name}.md` + `.html` |
| `slide-deck-builder`    | プレゼン内容をもとにスライド設計書を生成            | `SLIDE-DECK-{name}/SLIDE-DECK-{name}.md`                 |

---

## 使い方

---

### 1. プラグインをインストールする

Claude Code で以下のコマンドを実行するだけで完了します。

```
/plugin marketplace add skanehira/slide-plugin
/plugin install slide-plugin
```

3つのスキル（`slide-md-creator`・`slide-pattern-creator`・`slide-deck-builder`）がインストールされます。サンプルのデザインシステム（9種類）とスライドパターン（99種類）はプラグインに同梱されており、別途コピーする必要はありません。**Step 4** の `slide-deck-builder` はこれらの同梱ファイルを直接参照します（スライドパターンはコピー不要。デザインシステムだけは、選んだ1つをカスタマイズの起点としてプロジェクトの `SLIDE-md/` にコピーします）。

完了したらそのまま **Step 4** に進んでください。

---

### 2. デザインシステムを作る（カスタマイズしたい場合）

プラグインには9種類のサンプルデザインシステムが同梱されており、`slide-deck-builder` を実行した際に選択した1つがプロジェクトフォルダへコピーされます。そのまま使う場合は **Step 4** に進んでください。

自社ブランドや既存スライドに合ったオリジナルのデザインシステムを作りたい場合は、Claude Codeで以下のように話しかけます。

> 「このスライドのデザインシステムを作って」（画像・Webサイト・PowerPointを添付）

`SLIDE-md-{name}/SLIDE.md` と確認用の `sample.html` が生成されます。

**sample.html について：** 生成したデザインシステムが実際にどう見えるかを確認するための6ページのHTMLスライドです。1ページ目はデザインシステムの仕様概要（カラーパレット・タイポグラフィ一覧）、2〜6ページ目は表紙・セクションタイトル・箇条書き・データ・まとめの各レイアウトが、SLIDE.mdで定義した色・フォント・余白を使って描画されます。ブラウザで開くだけで確認できます。

> **注意：** このsample.htmlはClaude Codeが生成する簡易的な確認用サンプルです。デザインはシンプルな実装にとどまります。実際にスライドを生成する際のデザインクオリティは、SLIDE-DECK.mdを渡すAIツール（Claude Designなど）の能力に依存します。

---

### 3. スライドパターンを追加する（カスタマイズしたい場合）

プラグインには99種類のスライドパターンが同梱されており、`slide-deck-builder` が同梱ファイルを直接参照します（プロジェクトへのコピーは不要）。そのまま使う場合は **Step 4** に進んでください。

自分のスライドのレイアウトや好みのパターンを新たに追加したい場合に、このスキルを使います。

> 「スライドパターンを抽出して」（スライドの画像を添付）

`SLIDE-PATTERN-{name}/SLIDE-PATTERN-{name}.md` とスケルトンHTML（グレースケール）が生成されます。

**スケルトンHTMLについて：** パターンのレイアウト構造（エリアの分割・要素の配置）を確認するためのHTMLファイルです。色・フォント・装飾をあえて取り除いたグレースケールで描画されています。これは、パターンがどのデザインシステムとも組み合わせて使えるよう、構造だけを示すことを意図した設計です。実際のスライドに色やフォントを適用するのはSLIDE.mdの役割です。

---

### 4. プレゼンの設計書を作る

> 「プレゼンの設計書を作って」

ブリーフ（タイトル・対象者・目的・枚数）をヒアリングした後、プレゼン内容を受け取り、SLIDE-DECK.md を生成します。このファイル1枚をAIツールに渡すだけでスライドが生成できます。

---

### 5. AIツールでスライドを生成する

> **注意：スライドの生成はClaude Codeではなく、別のAIツールで行います。**

SLIDE-DECK.md はどのAIツール（NotebookLM、ChatGPT、Geminiなど）にも渡せますが、**Claude Codeユーザーには [Claude Design](https://claude.ai/design) がおすすめです。** ビジュアルデザインの生成に特化しており、高品質なスライドが生成できます。

**Claude Design でスライドを生成する手順：**

1. [claude.ai/design](https://claude.ai/design) を開きます（[claude.ai](https://claude.ai) の左上メニューの「Claude Design」からもアクセスできます）。
2. チャット入力欄の添付ボタンから、Step 4 で生成した **`SLIDE-DECK-{name}.md`** をアップロードします。
3. 以下のように話しかけます。

   > 「添付のSLIDE-DECK.mdに従ってスライドを生成してください」

4. Claude Design がデザインシステムとパターン定義を読み取り、スライドを生成します。

## ワークフロー

```
Step 1: プラグインのインストール（初回のみ）
Step 2: slide-md-creator  → SLIDE.md（オリジナルのデザインシステムを作る場合）
Step 3: slide-pattern-creator → SLIDE-PATTERN-*.md（オリジナルのパターンを追加する場合）
Step 4: slide-deck-builder → SLIDE-DECK.md（プレゼンごとに設計書を生成）
Step 5: SLIDE-DECK.md を Claude Design などのAIツールへ → スライド完成
```

---

## パターンギャラリー

99種類のレイアウトパターンをブラウザで一覧確認できます。

**→ <a href="https://sho-ai-magic.github.io/slide.md/" target="_blank">https://sho-ai-magic.github.io/slide.md/</a>**

![SLIDE-PATTERN ギャラリー](docs/Gallery2.png)

## 生成サンプル

### サンプル1：SLIDE.md スキル紹介

このスキルパッケージを使って生成したスライドの例です（[PDF全文はこちら](examples/output/SLIDE.md%20スキル紹介.pdf)）。

![スライドサンプル 1](docs/slide-sample-01.png)
![スライドサンプル 2](docs/slide-sample-02.png)
![スライドサンプル 3](docs/slide-sample-03.png)

---

### サンプル2：AI時代のスライド専用デザインシステム SLIDE.md

本スキルをもとにClaude Designで生成したスライドの例です（[PDF全文はこちら](<examples/output/AI時代のスライド専用デザインシステム SLIDE.md.pdf>)）。

![スライドサンプル2 1](docs/slide-sample2-01.png)
![スライドサンプル2 2](docs/slide-sample2-02.png)
![スライドサンプル2 3](docs/slide-sample2-03.png)

## サンプルファイル

**デザインシステムのサンプル（`docs/SLIDE-md/`）：**

| フォルダ                        | 説明                                                               |
| ------------------------------- | ------------------------------------------------------------------ |
| `SLIDE-md-blue-simple-diagram/` | 青を基調としたシンプルな図解向けデザインシステム                   |
| `SLIDE-md-digital/`             | デジタル庁デザインシステム（DADS）を参考に生成したデザインシステム |
| `SLIDE-md-green-blue-business/` | 緑・青を基調としたビジネス向けデザインシステム                     |

各フォルダに `SLIDE.md`（デザイン定義）と `sample.html`（確認用6ページHTMLスライド）が含まれています。プラグインに同梱されており、`slide-deck-builder` を実行した際に選択した1つだけがプロジェクトフォルダ内の `SLIDE-md/` へコピーされます。

特定のデザインシステムだけ使いたい場合は [ギャラリー](https://sho-ai-magic.github.io/slide.md/) でカードをクリックし、「SLIDE.md」タブからファイル内容をコピーして、プロジェクトフォルダ内の `SLIDE-md/SLIDE-md-{name}/SLIDE.md` として保存してもOKです。

- `examples/output/SLIDE.md スキル紹介.pdf` — このスキルパッケージを紹介するスライドの生成例（全文）
- `docs/SLIDE-PATTERN/` — 99種類のレイアウトパターンのHTMLファイル（ギャラリーから参照）

## ライセンス

MIT
