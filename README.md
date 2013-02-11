BXttnfss パッケージバンドル
===========================

乙部厳己氏の「書体拡張プロジェクト」（TTNFSS）のパッケージをサポート
する物を集めたパッケージ。「TTNFSS を dvipdfmx／pdfTeX で使うための
マップファイル」「LaTeX の一般的なフォントパッケージ（PSNFSS 等）と
TTNFSS との親和性を向上するパッケージ群」からなる。

### 前提環境

  - 処理系： LaTeX全て
  - DVIウェア： dvipdfmx, pdfTeX, dviout
  - 前提パッケージ
      * TTNFSS パッケージ集：「[書体拡張プロジェクト]」のサイトで
        配布されている。また dviout の配布物の中にも含まれている
        （バージョンが異なるがどちらでもよい）。次のパッケージに
        対応している。
          - サイト配布の「最小キット」（winttf.lzh）
          - サイト配布の「標準キット」（winttfn.lzh）  
            ※「追加キット」「和文拡張キット」には非対応。
          - dviout の配布物の FONT\winttf.lzh
  - TTNFSS が対象とするフォントが必要である。本パッケージがサポート
    する範囲のフォント群については、全てのバージョンの Windows で
    標準でインストールされている。

[書体拡張プロジェクト]: <http://argent.shinshu-u.ac.jp/~otobe/tex/packages/fontproj.html>

### インストール

  - `*.sty`   → $TEXMF/tex/latex/BXttnfss
  - `*.enc`   → $TEXMF/fonts/enc/dvips/BXttnfss
  - pdftex-bxttnfss.map → $TEXMF/fonts/map/pdftex/BXttnfss
  - pdfm-bxttnfss.map → $TEXMF/fonts/map/dvipdfm/BXttnfss

dvipdfmx・pdfTeX用マップファイル
--------------------------------

TTNFSS パッケージ（および他の「書体拡張プロジェクト」のパッケージ）
は dviout での使用を前提としているため、他の DVI ウェアでのための
マップファイルが含まれていない。W32TeX では dvipdfmx と pdfTeX の
ためのマップ設定が最初から含まれている(※1)が、他の TeX 配布では別途
用意する必要が生じる。そこで本パッケージでは、dvipdfmx と pdfTeX に
対するマップファイルを提供している。

※1 ただし Marlett に対する設定が欠落している。

### 設定方法

「インストール」の作業を行うと、以下のマップファイルが利用可能に
なっている。

  - pdftex-bxttnfss.map： pdfTeX 用のマップファイル
  - pdfm-bxttnfss.map： dvipdfmx 用のマップファイル

TTNFSS は TrueType フォントを対象としているので、マップファイルの
有効化の手順が Type 1 フォントを対象とする「普通の」ものとは少し
異なる可能性がある。以下の何れかの方法で有効化できる。

  - ただし、TeX Live の場合は、通常通り updmap で処理できる。この
    場合、pdfTeX 用のマップファイルを使う。

        updmap --enable Map pdftex-bxttnfss.map
        (またはupdmap-sys)

  - W32TeX の場合は、上述の通り、TTNFSS のマップ設定は既に行われて
    いるので、そもそも本パッケージのファイルの使用は不要である。

  - dvipdfmx 用のマップファイルは設定ファイル（dvipdfmx.cfg）中の
    「`f`」指定や起動時の `-f` オプションでも指定できる。

  - 本バンドルに含まれる bxwinenc パッケージを `mapload` オプション
    付きで読み込むとマップファイルが読み込まれる。

  - 当該のマップファイルの内容を各ソフトウェアの標準のマップファイル
    に追記する。

### 機能

TTNFSS パッケージを用いた文書について、dvipdfmx／pdfTeX を用いて、
対照の TrueType フォントが埋め込まれた PDF 文書が作成できる。

bxwinenc パッケージ - 他のフォント拡張との共存
----------------------------------------------

TTNFSS で `\'e` や `\c{c}` 等の（合成済み）アクセント付き英字や
`\ng`、`\textyen` 等の特殊文字を出力するためには、TTNFSS が提供する
winenc パッケージを読み込みエンコーディングを「T1」に切り替えること
が求められる。ところが、このパッケージは「T1 エンコーディングの定義
を改変してしまう」という重大な副作用がある。これにより、稀ではあるが、
他のフォント／記号拡張用のパッケージが誤動作するケースが存在する
（例えば textcomp パッケージの一部の記号が化ける）。

このパッケージでは、「T1 エンコーディングの定義の改変」を行わずに
winenc パッケージと同じ機能（TTNFSS の枠内で最大のグリフ集合を使用
可能にする）を提供する。実装方針としては、winenc が規定している
「改変後の T1 エンコーディング」を「WT1」という別の名前で登録して
それに切り替えている。

### パッケージの読込

TTNFSS の winenc パッケージの代わりに読み込む。（winenc を使わずに
OT1 で TTNFSS を使う場合には本パッケージも使うメリットはない。）

    \usepackage[<オプション>]{bxwinenc}

なお、TTNFSS の個別フォントのパッケージ（timesnew、arial 等）には
`T1` というオプションがあるが、これは内部で winenc の読込を指示する
ものである。従って、本パッケージを利用する場合は、`T1` オプションは
不要となる。

    \usepackage[<オプション>]{bxwinenc}
    \usepackage{timesnew} % [T1] を省く

オプションは以下の通り。

  - `WT1`（既定）： 既定のエンコーディングを WT1 にする。
  - `T1`： 既定のエンコーディングを T1 にする。
  - `OT1`： 既定のエンコーディングを T1 にする。
  - `mapload`： マップファイルを読み込む。
  - `nomapload`（既定）： マップファイルを読み込まない。

「既定のエンコーディングの設定」は fontenc パッケージにおけるそれと
同じ意味である。この設定に関わらず、以下で述べる WT1／T1 用の設定は
常に実施される。

### WT1 エンコーディングでの使用

WT1 エンコーディングは、元々 winenc にて（「T1」として）規定された
8 ビットのエンコーディングである。TTNFSS のフォントファミリをこの
エンコーディングで使用した場合は以下のような動作になる。

  - winenc パッケージで利用可能なグリフが全て同じ命令名で使える。
  - 「（正統な）T1 にあるが WT1 にないアクセント付き英字」、例えば
    `\'n` や `\v{r}` が合成により正しく出力できる。（これらの文字は
    winenc では化けてしまう。）
  - 既存のエンコーディング定義の改変を一切行わないため、他のフォント
    や記号を拡張するパッケージに影響を与えない。
  - WT1 エンコーディングは TTNFSS のフォントファミリでしか定義されて
    いないので、他のファミリに切り替える時はエンコーディングを一緒に
    変更する必要がある。（初期化時のエラーを防ぐために cmr／cmss に
    ついて WT1 用のダミーの定義を与えているが、これを実際に使っては
    いけない。）

### T1 エンコーディングでの使用

T1 エンコーディングの他の（TTNFSS 外の）ファミリとの併用をスムーズ
にするため、T1 エンコーディングでの使用も可能にしている。ただし、
実際に使われるのは WT1 用のフォントなので、一部の符号位置については
不正な文字が割り当てられているになる。

  - TTNFSS 外のフォントファミリとの間での切替の際にエンコーディング
    の切替の必要がない。
  - 「T1 にあるが WT1 にないアクセント付き英字」では文字化けが起こる
    （winenc と同じ）。
  - 「WT1 にあるが T1 にない文字」（`\textyen` 等）について、通常の
    文字命令では出力できない。（textcomp 読込時は TS1 への振替が
    起こるが TTNFSS のファミリでは無効なので cmr で代替される。）
    そこで、これらの文字については、`text～` を `WTT～` に変えた
    名前の命令（`\WTTyen` 等）で現在のフォントのグリフが出力できる
    ようにした。

bxwinpi パッケージ - winpi と pifont の共存
-------------------------------------------

PSNFSS の pifont と同様の機能（記号フォントのサポート）を提供する
TTNFSS のパッケージが winpi であるが、元々 TTNFSS が PSNFSS の「代用」
となることを意図していたため、pifont と winpi は同時に使用することが
できない。これを可能にするのが bxwinpi パッケージである。

### パッケージの読込

次の通り。オプションはない。

    \usepackage{bxwinpi}

bxwinpi は pifont を内部で読み込み実装コードを利用する。また bxwinpi
の読込の前に winpi と pifont の何れか一方を既に読んでいても構わない。
（両方を読むとその時点でエラーになるから、その状況はあり得ない。）
bxwinpi 読込後は、winpi と pifont 両方が読込済みとして扱われ、これら
が再び読み込まれることはない。

### 機能

原則として、winpi と同じ機能を提供する。ただし、`ding` 系列の命令は
（`\ding` 命令や `dinglist` 環境等）は pifont のものと重複するので
`ding` を `WTTding` に変えた名前で提供する。ただ、`WTTding` で使われる
フォント「Monotype Sorts」は Windows に標準で含まれるものでなく、
またこれ自体が pifont の `ding` 系列で使われる「Zapf Dingbats」と
互換のフォントである（つまり `WTTding` は `ding` と同じ出力になる）
のであまり使う機会はないと思う。

  - 次項で挙げる系列 `XXX` について以下の命令・環境を提供する。
    `XXX` に対応するフォントファミリで `Pi～` を実行したのと同等の
    動作をする。（例えば、`\dingaline{255}` は `\Piline{wmswd}{255}`
    と同じ。）
      * `\XXXsymbol{符号値}`
      * `\XXXfill{符号値}`
      * `\XXXline{符号値}`
      * `\begin{XXXlist}{符号値}～\end{XXXlist}`
      * `\begin{XXXautolist}{符号値}～\end{XXXautolist}`

  - 系列の一覧。
      * `XXX`=`WTTding`： wpi ファミリ「Monotype Sorts」
      * `XXX`=`dinga`： wmswd ファミリ「Wingdings」
      * `XXX`=`dingb`： wmswd2 ファミリ「Wingdings 2」
      * `XXX`=`dingc`： wmswd3 ファミリ「Wingdings 3」
      * `XXX`=`webding`： wmsd ファミリ「Webdings」
      * `XXX`=`marlett`： wmsml ファミリ「Marlett」

更新履歴
--------

  * Version 0.2  <2013/02/11>
      - 最初の公開版

--------------------
Takayuki YATO (aka. "ZR")  
http://zrbabbler.sp.land.to/
