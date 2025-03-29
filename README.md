# sly

<img width="2371" alt="Image" src="https://github.com/user-attachments/assets/bfa449ac-71c0-4c19-a162-e2b2c87b64a6" />

TouchDesignerで、スライドショーを実行します。  
次のような意図があります。
- スライドショーを作成するツールのような編集をすることなく、対象となる写真や映像にタイトルやクレジットのみを追加してスライドショーを実現させたい。
- なるべくファイルの追加やクレジットの変更を手軽に行ないたい。

## Specification

- ファイル名の昇順でスライドショーを再生する。
  - 写真の表示秒数を、UI（スライダー）で任意に設定可能。
  - 動画は自動的に尺に合わせて表示する。
- 写真と動画が混在していても再生する。
  - 対応しているファイル
    - `.jpg`
    - `.png`
    - `.mov`
    - `.mp4`
  - 動画の表示非表示を、UI（ボタン）で設定可能。
- 画面の左上と右下に、それぞれテキストを表示できる。
  - 表示テキストは、ファイル名と `text_conversion.json` からルールに則って表示される。
  - フォントサイズを、UI（スライダー）で設定可能。
  - 表示位置を、画面の角隅からの距離として、UI（スライダー）で設定可能。
- 設定用のUI
  - 実行中にキーボードの `o`で表示、 `c`で非表示 ができる。

## Environment

作成時（2025-03-29）の環境は以下になります。
- Mac
  - macOS: Ventura 13.6.1
  - Chip: Apple M1
  - Memory: 16GB
- TouchDesigner
  - 64-Bit Build 2022.29850

## Preparation

ディレクトリ構造はこんな感じです。  
`Backup/` や `main.144.toe` は、TouchDesignerを編集したりすると自動生成されるファイルです。
```zsh

❯❯❯ tree
.
├── Backup
│   ├── main.1.toe
│   ├── main. ... .toe
│   └── main.143.toe
├── LICENSE
├── README.md
├── data
│   └── 2024_evala
│       ├── 00-001.mov
│       ├── 00-002.mov
│       ├── 01-EBBTIDE
│       │   └── 01-001-evala2024-ebbtide.jpg
│       ├── 02-SPROUT
│       │   └── 02-001-evala2024-sprout.jpg
│       ├── 03-SCOREOFPRESENCE
│       │   └── 03-001-evala2024-scoreofpresence.jpg
│       ├── 04-003.mp4
│       ├── 04-INTERSCAPE
│       │   └── 04-001-evala2024-interscape.jpg
│       ├── 05-EMBRYO
│       │   └── 05-001-evala2024-embryo.jpg
│       └── 06-STUDIESFOR
│           └── 06-001-evala2024-studiesfor.jpg
├── main.144.toe
├── main.toe
└── text_conversion.json
```

### Data

- `data/` など、写真や動画を格納するディレクトリを用意すると整理しやすくなると思います。必須ではないです。  
- 写真や動画のファイルを用意します。対応しているファイルは以下です。
  - `.jpg`
  - `.png`
  - `.mov`
  - `.mp4`
- ファイル名は、`01-001-evala2024-ebbtide.jpg` のように、以下のルールに従ってください。  
  後述するテキスト変換処理により、ファイル名の文字列から実際に表示する文字列を生成するので、ファイル名自体はシンプルにするのが良いです。
  - 英数字のみを使用し、2バイト文字は使わない。
  - `-` (ハイフン)で繋いだ4項でなるファイル名とする。
    - `（大項目番号）-（小項目番号）-（左上テキスト用）-(右下テキスト用).（拡張子）`
      - 大項目番号： e.g. 作品の種類の番号
      - 小項目番号： e.g. 同じ作品で複数の写真があるときの整理番号
      - 左上テキスト用： e.g. 展覧会タイトルや会期に変換される文字列
      - 右下テキスト用： e.g. 作品名やフォトクレジットに変換される文字列
    - e.g. 
      - 文字列を左上にも右下にも表示しない場合： `00-001.mov`
      - 文字列を左上のみに表示する場合： `00-001-evala2024.mov`
      - 文字列を右下のみに表示する場合： `00-001--ebbtide.mov`
### Text Conversion

- `text_conversion.json` が、ファイル名の文字列から、スライドショーで表示する文字列に変換する対応テーブルになっています。
- ファイル構造は以下のとおりです。
    ```json
    {
        "exhibitions": {
            "evala2024": "evala　現われる場　消滅する像\n2024年12月14日（土）— 2025年3月9日（日）"
        },
        "works": {
            "ebbtide": "《ebb tide》2024年／撮影：冨田了平",
            "sprout": "《Sprout “fizz”》2024年／Photo:Yutaro Yamaguchi",
            "scoreofpresence": "《Score of Presence》2019年／撮影：清水はるみ",
            "interscape": "《Inter-Scape “slit”》2024年／撮影：冨田了平",
            "embryo": "《Embryo》2024年／撮影：冨田了平",
            "studiesfor": "《Studies for》2024年／撮影：冨田了平"
        }
    }
    ```
    - `exhibitions` と `works` は残して、それらに内包される部分を編集してください。
    - 改行は `\n` で反映されます。

## Note

- テキスト色は `#000` としてあり、背景が明るい時に溶けてしまわないように、細く黒い輪郭線を追加しています。
- 動画の尺ぴったりで次に移ってしまうので、改良の余地ありです。
- トランジション等のエフェクトもありません。
