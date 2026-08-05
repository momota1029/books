# 自然言語リクエストの制作ルーティング規範

## 目的

ユーザーは `.system/` command、directory 名、repository mode を知る必要がない。
「○○について論文を書きたい」「この本を翻訳したい」「この論文を読みたい」のように、欲しい成果物を自然言語で述べればよい。
エージェントが本規範に従って既存 project の継続または適切な制作領域の開始を判断し、必要な command を自ら実行する。

ルーティングは単語だけで決めない。
入力資料が本か論文かよりも、最終的にユーザーが欲しい成果物を優先する。
LLM は意図と文脈の抽出に使うが、route、曖昧時の停止条件、外部操作の権限は以下の規則で拘束する。

## 標準 route

| ユーザーの最終目的 | route | 開始または継続方法 |
|---|---|---|
| 新しい数学的結果を証明し、完全な研究論文として投稿したい | `papers/writing/<paper-id>/` | 既存の一致 project がなければ、エージェントが `.system/new-paper-project <paper-id>` を実行する |
| 書籍を忠実に翻訳し、統合された翻訳成果物を作りたい | `book-translations/<project-id>/` | 既存の一致 project がなければ、エージェントが `.system/new-book-project <project-id>` を実行する |
| 既存論文を理解し、検証可能な解説・講義ノートを作りたい | `paper-lecture-notes/` | singleton workspace の既存状態を確認して開始または継続する |
| 長編の self-contained な数学講義ノート・数学ノートを作りたい | `self-contained-math-notes/` | singleton workspace の既存状態を確認して開始または継続する |
| `papers/` で執筆中の自分の日本語論文を英訳したい | 同じ `papers/writing/<paper-id>/` または `papers/submitted/<paper-id>/` | 新しい translation project を作らず、同一論文の英訳 phase と日英同期 gate を使う |
| repository 外にある自分の既存原稿を取り込み、投稿準備・英訳・改訂を始めたい | `papers/writing/<paper-id>/` | 一致 project がなければ新規 paper project を作り、原稿を採用 input として履歴を保って取り込む |
| 投稿済み論文を改訂し、査読対応、再投稿、arXiv 差替え等を準備したい | 同じ `papers/submitted/<paper-id>/` | 送信済み snapshot を保持して継続し、外部操作は `papers/AGENTS.md` の再承認 gate に従う |
| 他者の論文を忠実に翻訳したい | 自動 route なし | `paper-lecture-notes/` は単なる和訳を目的としないため誤配属せず、専用 workflow を作るか解説ノートへ目的を変えるか確認する |
| 一回限りの質問、短い要約、会話内だけの説明が欲しい | project を作らない | 会話で回答し、永続成果物が必要になった時点で route を選ぶ |

`paper-id` と `project-id` は user-facing command 引数ではない。
ユーザーが指定しなければ、エージェントが題材から短い lowercase ASCII の `kebab-case` identifier を作る。
題名や scope がまだ不安定でも、directory 名の決定だけを理由に制作を止めない。
既存 path と衝突した場合は上書きせず、同一案件なら継続し、別案件または同一性不明ならユーザーに確認する。

## 成果物優先の判定

- 「A の論文を読んで、その手法を発展させた新しい論文を書きたい」は `papers/` に route する。A の論文は新規論文 project の `inbox/` に置く参考資料であり、最終成果物を `paper-lecture-notes/` へ変えない。
- 「A の論文を読んで、内容を自分で説明できるノートを作りたい」は `paper-lecture-notes/` に route する。新規性や投稿の語が資料中にあるだけで `papers/` を選ばない。
- 「現在の日本語論文を英語にして投稿したい」は既存 `papers/` project の英訳・投稿 phase に route する。書籍翻訳 project を作らない。
- 「この論文を日本語で読みたい」が、忠実な全訳を意味するのか、理解のための日本語解説を意味するのかで route は変わる。文脈から一意でなければ確認する。
- 「この論文を読みたい」だけでは、会話内だけの読解支援、永続的な解説ノート、忠実な翻訳のどれかを確定できない。project を作る前に一問で確認する。
- 「○○について書きたい」だけでは、投稿論文、講義ノート、解説、会話回答のいずれかを確定できない。永続 source を作る前に、最終成果物を一問で確認する。
- 複数の独立成果物を明示的に求められた場合は一つへ押し込まない。各成果物の route と共有する input を示し、canonical source や台帳を混ぜないよう別々に開始する。

## 曖昧な依頼への確認

route が変わる曖昧さだけを質問し、command 名や内部 directory の選択をユーザーへ委ねない。
原則として次の一問で判定する。

> 最終成果物は、投稿・改訂する研究論文、忠実な翻訳、永続的な解説ノート、self-contained な数学ノート、会話内だけの読解支援のどれですか？　翻訳なら source は書籍、他者の論文、自分の論文のどれですか？

以下の場合は確認せず安全に進めてよい。

- 「○○について新しい論文を書いて投稿したい」のように成果物が一意である。
- 「この本を全訳したい」のように source 種別と忠実な翻訳が明示されている。
- 「この論文の証明を検証し、講義ノートにしたい」のように解説成果物が明示されている。
- 既存 project 名または path が指定され、その project の継続であることが明らかである。

以下は確認または停止が必要である。

- 「翻訳を作りたい」だけで、書籍、他者論文、自分の論文のどれか不明である。
- 「この論文を読みたい」だけで、会話回答、永続的な解説、忠実な翻訳のどれか不明である。
- 同じ題材について複数の既存 project があり、継続先を一意に決められない。
- singleton workspace に別案件の active source、未完了 `STATUS`、未処理 `inbox` がある。
- 忠実な他者論文翻訳のように、現在の共有制作領域に適合する route がない。
- project 作成が既存 path の上書き、既存入力の移動、repository mode の変更、権利・privacy 境界の変更を必要とする。

## 自然言語から開始するときの実行規則

1. 依頼から最終成果物、source 種別、新規作成か継続か、投稿・配布の有無を抽出する。
2. repository 内の候補 project と `STATUS` を read-only で確認し、同一 project の重複作成を避ける。private な題名、著者、ファイル名、内容を進捗報告へ不要に転載しない。
3. route が一意なら、選んだ route と理由を短く commentary で伝える。内部 command の実行許可を改めてユーザーへ求めず、依頼範囲内の通常の初期化としてエージェントが実行する。
4. 新規 project では適用される `AGENTS.md` と shared system を確認し、必要な bootstrap/doctor gate と repository mode を確認する。public mode で private な制作物を保護・復旧できない場合は、空の ignored skeleton の作成を超える制作を始めず、親 `AGENTS.md` の recovery 規則に従って verified private mode または承認済みの復旧経路を確定する。エージェントが実行可能な設定 command は自ら実行し、credential や外部 repository の準備などユーザーにしかできない操作だけを具体的に依頼する。
5. 初期化 command が失敗した場合は手作業で半端な構造を作らず、原因を解消または報告する。
6. 作成後は user-facing な project path、最初に必要な資料、最初の成果物を伝える。ユーザーに `.system/` command の入力を要求しない。
7. route 決定は外部投稿、配布、購入、license 同意、著者変更等の権限を与えない。それらは各領域の明示承認 gate に従う。

## 既存 project と singleton workspace

- `papers/writing/`、`papers/submitted/`、`book-translations/` は direct child を調べ、題材、records、status から同一性を確認する。名前が似ているだけで既存 project と断定しない。
- 一致する active project が一つなら新規作成せず継続する。submitted 論文の改訂や英訳も同じ project で行う。
- repository 外の自著原稿を初めて取り込む場合は `papers/writing/` に project を作り、原稿を上書き可能な working copy として直接扱わず、採用 input revision と canonical source の起点を記録する。すでに外部投稿済みなら送信 snapshot と識別番号を記録したうえで `submitted/` lifecycle に置く。
- `paper-lecture-notes/` と `self-contained-math-notes/` は現在 singleton workspace である。別案件の active data がある場合は混在させず、既存案件を継続するか、ユーザー承認後に project-isolated 構造を追加するかを確認する。既存 data を自動で archive、移動、削除しない。
- 入力資料がまだない場合でも、題材と成果物が一意で、repository mode と recovery 条件を満たすなら project の空構造と status を開始してよい。public mode で保護経路が未確定なら private な status は作らず、ignored skeleton に留める。資料がなければ検証できない作業を推測で埋めず、次に必要な input を具体的に伝える。

## routing acceptance examples

- 「スペクトル系列の収束について論文を書きたい」→ `papers/writing/` に新規または既存の研究論文 project。
- 「今書いている日本語論文を英訳したい」→ 該当する既存 `papers/` project。複数候補なら project を確認。
- 「手元の原稿を雑誌投稿できる形に整えたい」→ 一致 project がなければ `papers/writing/` に取り込み、投稿準備 phase を開始。
- 「投稿済み論文の査読コメントに対応したい」→ 同じ `papers/submitted/` project で送信版を保持して改訂。外部再投稿は別途承認。
- 「この本を日本語に全訳したい」→ `book-translations/`。
- 「この論文を読み、証明を補って解説 PDF にしたい」→ `paper-lecture-notes/`。
- 「この論文を一字一句に近い形で日本語化したい」→ 他者論文の忠実翻訳かを確認し、`paper-lecture-notes/` へ自動配属しない。
- 「この論文を読みたい」→ 会話内の読解支援、永続的な解説 PDF、忠実翻訳のどれかを一問で確認。
- 「測度論を前提から説明する長いノートを作りたい」→ `self-contained-math-notes/`。
- 「この定理の意味を3行で教えて」→ project を作らず会話で回答。
- 「この論文を読んで、その障害を解消する新しい結果を論文化したい」→ 最終成果物を優先して `papers/writing/`。参照論文は同 project の input。
