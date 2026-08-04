# 書籍翻訳・統合 PDF 制作ガイド

## 適用範囲と正本

この `AGENTS.md` は `book-translations/` とその配下だけに適用する。書籍翻訳以外へ固有規則を持ち込まない。上位の指示と衝突する場合は上位を優先し、判断できなければ作業を止めて確認する。

各 project は親の `../AGENTS.md`、`../JAPANESE_WRITING.md`、`../MATH_PROSE_REVIEW.md` と repository root の version 管理された `../.system/` tools を直接使う。これらの private copy を作って正本にしてはならない。案件固有の adapter が必要なら、差分と理由だけを project の private area に記録する。日本語の原稿・訳注・編集文は `../JAPANESE_WRITING.md` に従う。ただし、書籍翻訳では原文への忠実性、modality、著者の声の保持を共通規範より優先する。

## 目的

利用権限を確認できる複数の英語書籍を日本語へ翻訳し、出典との対応を保って教育的・論理的に再編成し、単一の PDF として読める成果物を作る。正確な翻訳、要約、再構成、訳注、編者補足を区別し、各記述を原資料まで追跡可能にする。

想定成果物は、統合 PDF、再現可能な編集 source、書誌・版・権利台帳、原資料と統合章節の対応表、用語・表記規約、翻訳判断、照合・検証記録である。完成 PDF だけを残して編集根拠を失わず、原資料や中間生成物を公開成果へ無差別に含めない。

## ワークスペースの契約

1 project は、利用権限を確認できる複数の英語書籍をまとめて 1 つの PDF を制作する単位である。新しい project は `book-translations/` で `../.system/new-book-project <project-id>` を実行して、repository 直下ではなく `book-translations/` の直接の子に作る。手作業で別構造を複製しない。

```text
book-translations/
  AGENTS.md
  <project-id>/
    inbox/
    draft/
    out/
    .workspace/
      source/
      records/
      tools/
      build/
      cache/
      tmp/
      logs/
      locks/
```

`book-translations/` 直下で予約された public entry は `AGENTS.md` だけである。`<project-id>`、project directory、その全 subdirectory と全内容は private work であり、公開用の構造 marker は置かない。共有 tool は project 内へ複製せず repository root の `../.system/` に置く。

- `<project-id>/inbox/`: ユーザーが原資料、追加指示、画像、データ等を置く受け入れ口。原則 read-only とし、エージェントは中身を編集、改名、移動、削除、上書きしない。
- `<project-id>/draft/`: ユーザーが確認する途中成果。PDF と、必要なら `STATUS` や変更要約を置けるが、人手編集する正本にはしない。
- `<project-id>/out/`: 検証済みかつ承認済みの最終成果だけを置く。ビルドコマンドの直接出力先にしない。
- `<project-id>/.workspace/`: project ごとの内部領域。ただし秘密保管場所ではない。`source/` は人が修正する canonical source、`records/` は永続台帳、`tools/` は案件固有の設定・adapter・lockfile の永続領域とする。`build/`、`cache/`、`tmp/`、`logs/` は再生成可能領域、`locks/` は案件内の排他制御専用とする。

`.workspace/` 全体をキャッシュや削除可能領域とみなしてはならない。`.workspace/source/`、`.workspace/records/`、`.workspace/tools/` を clean で削除しない。canonical source、manifest、用語、権利、build は用途別に独立させ、混在させない。

project 間で inbox、source、records、tools、build、cache、tmp、logs、locks、draft、out、manifest、state を一切共有しない。並行作業の単位は project とし、同一 project 内で同じ canonical source または manifest を更新する処理は直列化するか `.workspace/locks/` の project-local lock を使う。ある project の inbox 追加によって他 project の状態や成果を無効化してはならない。project 横断で資料や訳文を再利用する場合は権利と版を再評価し、symlink、共有可変 cache、共有 lock を使わない。

複数案件で有効な翻訳 prompt、用語処理、source mapping、build、QA の改善は、案件名、原文、書誌、用語、権利情報、ファイル名等を除去して generic 化し、privacy review と共通の検証を通して repository root の shared system へ upstream する。generic な改善を private project 内だけに残さない。案件固有の用語、権利判断、原文、manifest、config、adapter は private project に置く。

## プライバシーと Git 境界

repository mode は親の規則と `../.system/repository-mode` に従う。

- public mode では、すべての `<project-id>/` とその全内容、project 名、原文、原稿、PDF、manifest、台帳、config、案件固有 tool を stage、commit、push しない。`book-translations/` 内で Git 対象にできるのは privacy review 済みの `AGENTS.md` だけである。
- verified private mode では、configured remote の同一性と GitHub visibility が `PRIVATE` と検証できる場合に限り、inbox、draft、out、`.workspace/` を含む processing document を commit できる。stage は必ず `../.system/repository-mode add -- <paths>` を使い、直接の `git add` や `git add -f` を使わない。
- private mode でも credential、secret、token、鍵、契約・ライセンス上保存できない data、権利上 commit できない原資料は commit しない。private remote の存在は取得、保存、翻訳、配布の権限を与えない。
- `out/` の配布承認と GitHub repository の visibility・commit 可否は別の判定である。配布承認済みでも GitHub に置けるとは限らず、private GitHub に置けても配布できるとは限らない。

`.gitignore`、repository mode、hook を回避せず、`--no-verify` を使わない。status、diff、作業報告にも、私的な原文、タイトル、著者、研究内容、ファイル一覧、hash を必要なく転記しない。この構造と ignore 規則は defense in depth にすぎず、secret store、暗号化、アクセス制御の境界ではない。

## 永続データの配置

- `<project-id>/.workspace/source/`: 翻訳・統合本文、章構成、訳注、索引、成果用に作成した図表等の canonical source。
- `<project-id>/.workspace/records/source-map.*`: Source ID、書名、版、章節、ページと統合先の章節、扱い種別を双方向に追える対応表。
- `<project-id>/.workspace/records/rights-ledger.*`: 書誌、版、出版社、年、ISBN、URL、参照日、入手元、ライセンス・許諾、利用根拠、引用・翻訳・翻案・図版・公開・配布条件、個人情報・秘密・契約制限の判定。
- `<project-id>/.workspace/records/glossary.*`: 原語、候補訳、採用訳、例外、初出、記号、表記規則。
- `<project-id>/.workspace/records/translation-decisions.*`: 要約、再構成、訳注、矛盾、原文の誤り、欠落、未確定事項、照合結果と判断根拠。
- `<project-id>/.workspace/records/source-ingestion-manifest.*`: `<project-id>/inbox/` の検出履歴と、採用した入力 snapshot。
- `<project-id>/.workspace/records/` のその他の用途別台帳: style、図表、QA、数学文章 review、公開判定等。台帳名と形式は案件開始時に決める。
- `<project-id>/.workspace/tools/`: 案件固有の build 設定、adapter、依存 lockfile。複数案件に有効な script、template、prompt、QA tool の正本はここに置かず shared system へ upstream する。

## inbox 受け入れプロトコル

ingestion と翻訳・執筆を分離し、受け入れが確定した snapshot だけを制作入力にする。

1. 作業開始時、各処理 batch 開始時、PDF を `draft/` へ公開する直前、`out/` へ昇格する直前に、対象 project の `inbox/` を再走査する。
2. 検出結果を `.workspace/records/source-ingestion-manifest.*` に追記する。各 revision について、少なくとも相対 path、byte size、更新時刻、強い content hash の方式と値、初回検出時刻、処理状態、採用・保留・除外理由を保持する。採用する revision は全内容を読み取った強い content hash を必須とする。
3. 同一性を path、ファイル名、時刻だけで判断しない。採用前に時間を置いて 2 回以上検査し、各回で hash 計算の直前と直後の size、mtime 等の利用可能な stat が一致し、全内容の読み取りが成功し、全回の強い content hash が一致することを要求する。同名、同 size/mtime の内容変更も hash で新 revision として検出し、翻訳、引用、ページ対応、権利、既存 PDF への影響を評価する。
4. 書き込み・同期・lock 中、一時拡張子、stat/hash の不一致、読み取り・parse 失敗は `pending` として処理しない。ユーザーの完了合図は安定確認の代替ではなく再検査の trigger に限り、長い固定 sleep を前提にしない。採用後は権利・機密上許される場合だけ immutable snapshot を `.workspace/` 内の案件に適した永続・保護領域へ保存し、その所在と hash を manifest に記録する。保存しない場合は、入力を使用するたびと `draft/`・`out/` への公開直前に記録済み hash を再検証する。
5. archive は自動展開しない。形式、path traversal、symlink、巨大展開、入れ子、暗号化の有無を先に検査し、安全と判断したものだけを `.workspace/tmp/` の隔離先へ展開する。展開物も個別に受け入れ判定する。
6. PDF、Office、画像等を信頼して実行せず、macro、埋め込みファイル、外部 link、偽装形式に注意する。必要なら内容を検証し、関連性、版、権利、重複、既存成果への影響を記録する。
7. ユーザーが入力を削除しても manifest の検出・採用・削除履歴は消さず `missing` として影響を確認する。制作中の追加・更新を見つけても、既存ユーザー入力を移動・削除せず、作業を黙って巻き戻さない。
8. 並行処理では担当 snapshot を分け、同じ manifest や canonical source を競合更新しない。更新の所有者を一つにするか、直列に統合する。
9. `draft/` への公開時に、採用した manifest の snapshot/revision を `STATUS` または変更要約へ記録する。
10. `out/` 昇格直前の再走査で、未評価の安定ファイル、変更 revision、`pending`、新規 `missing`、採用 revision の欠落、読み取り失敗、hash の計算・再検証失敗、または記録済み hash との不一致があれば停止して報告する。例外進行は、影響評価とユーザー合意を manifest に記録した場合だけ認める。

## draft と out

- `draft/` の PDF は途中成果であり final と呼ばない。`out/` への移動・複製は、完了条件を確認した明示的な promotion とする。
- 公開状態を `draft/STATUS.*` 等へ記録し、少なくとも build 日時、成功・失敗・陳腐化、source revision または commit、採用した inbox snapshot、検証状態、残件、対応する PDF 名を含める。
- 新しい build を開始した時、inbox revision や canonical source が変わった時、または build が失敗した時は、古い成功 PDF を最新版と誤認しないよう `STATUS` を先に `building`、`failed` または `stale` に更新する。
- `out/` の既存成果を直接上書きする前に、対象名、版、検証結果、承認を確認する。可能なら成果物と共に build 日時、source revision/commit、採用 inbox snapshot、検証状態を記録する。
- build 失敗時は `out/` を変更しない。`draft/` と `out/` の公開物を clean 対象にしない。

## ツールチェーンと latexmk 契約

- LaTeX、Typst、Pandoc 等をこの指示だけで固定しない。既存方式を優先し、新規案件では日本語組版、数式、索引、相互参照、長文分割、再現性を比較してユーザー確認後に最小構成を作る。
- 合意前に大量の template や依存を追加しない。既存 engine を uplatex から LuaLaTeX 等へ無断で変えない。
- LaTeX/latexmk を採用する案件では、local latexmk 4.83 の `-outdir`、`-auxdir`、`-emulate-aux-dir` 相当の分離機能を利用できる。`out_dir` 相当を `.workspace/build/out`、`aux_dir` 相当を `.workspace/build/aux` とする。
- latexmk が生成する PDF、DVI、SyncTeX、log、aux、fls、fdb 等は、まず全て `.workspace/build/` 配下へ置く。`draft/` や `out/` を latexmk の直接 outdir にしない。
- latexmk の終了 code 成功、生成 PDF の存在と今回の更新、log、未解決参照、引用、font、page を確認する。その後 PDF だけを `draft/` 内の一時名へ copy し、同一 filesystem 上の rename 等で原子的に公開する。`STATUS` は公開 PDF と同じ検証結果を指すよう更新する。
- 失敗時は古い PDF を最新とみなさず `STATUS` を `failed` または `stale` にし、`out/` は不変とする。完了条件を満たした draft PDF だけを明示的に `out/` へ promotion する。
- clean は `.workspace/build/` 等の再生成領域に限定する。相対 path、`-cd`、BibTeX、MakeIndex、glossary、画像参照は案件の実文書で検証する。
- 実文書がなく未検証なら latexmkrc や build script を先回りして作らない。案件開始時に採用方式を確定し、`.workspace/tools/` に案件固有の最小設定を作って実 build で検証する。汎用化できる改善は root shared system へ upstream する。

## 標準ワークフロー

1. 対象 project の `inbox/` を受け入れプロトコルどおり走査し、対象書籍、版、翻訳範囲、読者、単一 PDF の構成、公開範囲、納期、既存 toolchain を確認する。
2. 資料ごとに一意な Source ID を付け、書誌・版・権利・参照方式を rights ledger と source map に登録する。版違いは別 revision/record とする。
3. 統合章節ごとに Source ID、原章節、版、page 範囲、翻訳・要約・再構成・引用・補足の種別を source map に登録する。
4. 重要語の候補訳、採用訳、例外、初出説明、原語併記を glossary に定める。
5. 意味、論理関係、条件、否定、数量、定義、数式を原文に照らして翻訳する。不確かな箇所は推測で確定せず、検索可能な未確定表示と根拠を残す。
6. 配列変更、要約、接続文、重複整理を翻訳と分離し、変更種別、採用元、省略元、理由を translation decisions に記録する。
7. 原文対応、固有名詞、引用、数値、数式、図表、page、用語を照合する。重要箇所は authoring と別の視点で再確認する。
8. 数学を含む書籍では `../MATH_PROSE_REVIEW.md` の全 phase を authoring から独立した review phase として実施し、private records に結果を保存する。open blocking が 0 件になるまで次へ進まない。
9. 受け入れ snapshot を再確認して build し、log と全 page 表示を検証して `draft/` へ公開する。
10. 権利、license、配布範囲、不要物混入、直前の inbox 状態を確認し、承認後にだけ `out/` へ昇格する。

## 翻訳・再構成・出典規約

- 翻訳、要約、再構成、直接引用、訳注、編者補足を識別可能にする。原文にない説明、例、接続、評価、推論は合意した表示で明示し、原著者の主張に見せない。
- 直接引用は原文と照合して正確性を保ち、脱落・省略、原文にない強調、訳文の併記を明示する。引用と翻訳の表示・照合結果を source map または translation decisions に残す。
- 欠落・判読不能箇所を推測で埋めない。原資料間の矛盾は安易に解消せず、版、定義、文脈を確認して両論と編集判断を記録する。
- 原文に誤りがあると判断しても訳文で黙って修正しない。原文どおりの内容、疑義、確認根拠、採用した扱いを translation decisions に記録し、必要に応じて訳注で訂正候補または解釈を明示する。
- 意味を変える意訳、説明追加、節移動は追跡可能にする。AI 生成の補足、候補訳、要約は、原文・出典との照合なしに確定本文へ入れない。
- 引用・翻訳・図表には可能な限り Source ID、章節、版、page または安定位置を付ける。統合先から原資料へ、原資料の使用範囲から統合先へ双方向に追跡できるようにする。
- 定義、列挙、比較、条件、例外、否定、数量、専門用語、引用、数値、数式を重点照合する。訳語変更時は既存箇所を検索し、文脈依存の例外には理由を残す。
- 文体、句読点、全半角、固有名詞、原語併記、敬体・常体、数字・単位、脚注形式を glossary または style 台帳で統一する。要約・簡略化で条件、例外、論理関係を落として誤解を招かないよう translation decisions と照合する。
- 利用権限を確認できる資料だけを扱う。直接引用は必要最小限とし、rights ledger の URL、参照日、license・許諾、翻訳権、翻案・編集、図版、公開・配布条件を確認する。個人情報、秘密情報、購入資格情報、契約制限素材、DRM 回避物、権利不明素材を commit・公開しない。

## 数式・図表・PDF 品質

- 原文の記号、添字、上付き、演算子、式番号、定義域、量化範囲を照合する。複数書籍の記号を統一する場合は原記号、統一記号、範囲、理由を記録し、原文引用は勝手に改変しない。
- 数学を含む場合、`../MATH_PROSE_REVIEW.md` の全 phase と promotion 条件を適用する。review は原則として authoring 担当とは別の read-only phase とし、blocking finding の解消または scope 除外へのユーザー合意を記録して open blocking を 0 件にするまで `out/` へ promotion しない。
- 図表ごとに出典、版、元番号、page、権利、引用・翻訳・改変・新規作成の別を図表台帳へ記録する。改変範囲を明示し、番号、caption、凡例、単位、本文参照、白黒・縮小時の可読性、accessibility、画像解像度を確認する。
- build 成功だけで完成としない。error、warning、未解決参照、重複 label、欠落引用、目次、索引、bookmark、式番号、図表位置を確認する。
- 日本語・欧文・数式 font、埋め込み、代替、文字化け、禁則、行末、脚注、余白、見開き、空白 page、page 番号、はみ出しを確認する。最終候補は全 page を目視または rendering 検査する。
- 検証記録には実施日、対象 source revision、採用 inbox snapshot、引用・文体・権利・図表を含む実施項目、結果、残件を残す。Git 差分に原資料、中間物、秘密情報、権利不明素材、無関係な変更がないことを確認し、台帳または QA に未解決の不備があれば `out/` へ promotion しない。

## 禁止事項

- 権利未確認の書籍を翻訳対象、Git、公開物へ加えること。
- 翻訳、要約、再構成、補足を無表示で混ぜ、原著者の主張に見せること。
- 出典対応、版、page、訳注、矛盾、原文の誤り、版変更を記録せず統合すること。
- inbox の入力をエージェント都合で変更し、不安定・未評価の入力を採用すること。
- project 間で source、records、tool state、build、cache、lock を共有すること。
- 合意なく toolchain や engine を固定・置換すること。
- build 成功だけを根拠に PDF を完成扱いし、draft を無検証で out に置くこと。

## 完了条件

- 合意範囲が単一 PDF に収録され、構成、本文、目次、相互参照が整合している。
- 全章節を Source ID、版、章節、page まで追跡でき、翻訳・要約・再構成・補足の区別が明瞭である。
- 原文照合、用語統一、重複・矛盾・原文の誤りの処理、引用・権利確認が完了し、未確定事項は残件として明示されている。
- 合意した手順で再生成でき、warning、参照、数式、font、全 page の layout 検証を通過している。
- 数学を含む場合は `../MATH_PROSE_REVIEW.md` の独立 review phase が完了し、open blocking が 0 件である。
- 採用 inbox snapshot、source revision、検証結果、承認が記録され、最終成果だけが `out/` に明示的に昇格されている。
- 配布対象に原資料、中間物、秘密情報、権利不明素材が混在せず、公開・配布条件が確認済みである。
