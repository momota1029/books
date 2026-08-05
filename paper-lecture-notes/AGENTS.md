# 論文理解・講義ノート PDF 制作ガイド

## 適用範囲

この `AGENTS.md` は `paper-lecture-notes/` とその配下だけに適用する。書籍翻訳や体系的数学教科書へ固有規則を持ち込まない。上位の指示と衝突する場合は上位を優先し、判断できなければ作業を止めて確認する。

各 project は repository root の `AGENTS.md`、`JAPANESE_WRITING.md`、`MATH_PROSE_REVIEW.md` と、repository root で version 管理された `.system/` tools を shared versioned 正本として直接使う。private copy を正本にせず、project-local `AGENTS.md` を作らない。案件固有の adapter が必要なら差分と理由だけを project の private area に記録する。日本語の原稿・訳注・講義ノートは repository root の `JAPANESE_WRITING.md` に従う。ただし、論文理解では原論文の主張と自分の解釈を明確に区別することを共通規範より優先する。

## 目的

論文を自分で説明・検証できる水準まで咀嚼し、主張、仮定、定義、手法、証明・導出、実験、限界、関連研究を教育的な順序に再構成した講義ノート PDF を作る。対象分野を数学に限定せず、統計、生命科学、計算科学、実験科学等では、図の各 panel、データ、統計解析、実験条件、supplement、外部 software まで説明対象に含める。原論文、執筆者の解釈、外部知識、未解決の疑問を区別し、各説明を原論文または追加出典まで追跡可能にする。

想定成果物は、学習目標を備え、合意した前提知識と範囲に対して自己完結し、必要な推論・導出・計算・図解・再現条件を本文中で完結させた講義ノート PDF、再現可能な編集ソース、書誌・権利台帳、主張・式・図表・外部資源・software・実行結果の追跡表、解釈・疑問・再計算の記録である。原論文の単なる和訳や節順の写しではなく理解のための再構成物とするが、原論文の主張範囲を変えない。

## project 分離の不変条件

`paper-lecture-notes/` は project の collection root であり、新規案件の project root ではない。新しい案件は repository root から `.system/new-paper-note-project <project-id>` を実行し、必ず `paper-lecture-notes/<project-id>/` に作る。`project-id` は成果物の scope を表す短い lowercase ASCII の `kebab-case` とし、原論文の一時的なファイル名や処理順を ID にしない。project の direct child 以外に案件 directory を作らず、project-local `AGENTS.md` も作らない。

project の単位は原論文一件ではなく、一つの講義ノート成果物と一つの lifecycle である。複数の原論文を同じ project に置けるのは、同じ読者、学習目標、scope、canonical source、`STATUS`、review gate、配布判断を共有し、一つの統合 PDF に収束する場合だけとする。原論文ごとに独立 PDF を作る、進捗・公開判断を独立させる、または一方だけを完了・再開できるなら、題材や著者が近くても project を分ける。逆に、一つの統合ノートの primary paper と supporting paper を、入力が複数という理由だけで別 project に分割しない。

書き込み、ingestion、harness 操作、build、review、draft 同期、promotion、委譲の前に、担当 Codex は current project root を一つだけ確定する。direct-child project では、real path が `paper-lecture-notes/` の直下にあり symlink でなく、`.workspace/project-id` の一行が directory basename と一致することを確認する。この確認に失敗した project、または候補が複数あり records と `STATUS` でも同一性を決められない状態では、read-only の候補確認を超えて進まず、成果物名または題材を用いた一問でユーザーに継続先を確認する。内部 command や directory の選択をユーザーへ委ねない。

全 mutable state は選択した project root の `inbox/`、`draft/`、`out/`、`.workspace/` の内側に閉じる。collection root や別 project の catalog、ingestion manifest、source、generated index、review receipt、lock、build、cache、`STATUS`、draft、out を読み書き先として再利用しない。project 間の symlink、hardlink、`\input`、相対 path 参照、共通 mutable build directory も禁止する。資料や generic code を再利用する場合は、権利を確認した別 asset または repository 共通 tool として各 project から追跡し、source project の private path に依存させない。

複数 project を一つの依頼で扱う場合は、開始前に project ID、成果物、current input snapshot、変更対象、担当、状態を project ごとに分けて記録し、各操作へ project root を明示する。project 切替前には現在 project の canonical source と `draft/STATUS` を同期し、harness transaction と build が終了し、lock と一時生成物の状態が説明可能であることを確認する。別 project の review、gate、fingerprint、ID、承認を流用せず、Git index の操作も親規約どおり直列化する。

collection root 直下にこの規約導入前から存在する `inbox/`、`draft/`、`out/`、`.workspace/` は、全体で一つの `legacy-singleton` project とみなす。既存案件と同一の場合に限り `<project-root>=paper-lecture-notes` として継続できるが、別の原論文や成果物を追加して複数案件化してはならない。direct-child project への移行は、ユーザーが移行対象と project ID を明示的に承認し、canonical source、台帳、draft/out、履歴の移行表を記録した場合だけ行う。自動で移動、複製、改名、archive、削除せず、移行後は二つの active canonical copy を残さない。

## evidence harness と発見範囲

図、表、supplement、dataset、外部 file、URL、repository、software、実行結果を自由形式の台帳だけで管理しない。repository 共通の `.system/paper-evidence-harness` と、その project が持つ機械可読 catalog を使い、発見、採否、取得、固定、説明、本文参照、生成物の lineage、build snapshot を閉じる。

「すべての外部資源」の自動発見範囲は、採用した `inbox/` の local input と canonical note source、その PDF text・PDF annotation・添付、およびそこに直接現れる URL の一段までとする。DOI landing page や repository README からさらに見つけた公式 supplement、data、code、release は、agent が内容と関連性を確認して `add-resource` で直接資源として登録する。取得物を無制限に再帰 crawl せず、次の段を調べる必要がある場合は新たな採用単位として明示する。発見した各項目は `include`、`explain`、`exclude`、または WIP 中の `pending` のいずれかとし、`exclude` には理由を必須とする。

catalog の ID と relation は少なくとも次の対象を結ぶ。

- asset: 原論文、supplement、dataset、archive、画像、note asset、software output と content hash
- visual: figure、table、plot、diagram、panel、PDF 内 raster candidate と source locator
- resource: DOI、公式 page、download、repository、license 等の direct URL
- software: repository、requested revision、resolved commit、license、環境、利用法、取得 receipt
- run: software、実行 command、環境・container、dependency lock、seed、input、output hash、成否
- relation: `derived-from`、`generated-by`、`input-to`、`reproduces`、`explains`、`uses` 等の型付き edge

自動抽出は完全性の証明ではない。caption のない vector drawing、複合 panel、scan 画像内の図、shading、PDF object として分離されない図、リンク文字列と異なる PDF annotation、外部 page の動的内容は漏れ得る。採用 PDF と採用 visual は `prepare-visual-review` で source hash に結合した全ページ・全 frame の review package を作り、担当 Codex が全 frame を実際に確認する。別 identity の Codex が同じ package を独立に再確認し、両者の immutable receipt が `pass` した後だけ該当 review status を `complete` とする。人手は既定の必須条件ではないが、判読不能、分野知識だけでは一意に定まらない科学的解釈、原資料の欠落、権利・privacy の判断不能、または両 review の不一致が残る場合は、推測で閉じずユーザーまたは domain expert へ判断を求める。自動候補の誤検出は理由付きで `exclude` にし、漏れた図は `add-visual` で登録する。

## ワークスペースの契約

各 direct-child project の作業領域は次の4つに固定する。共有 `AGENTS.md` は collection root に一つだけ置く。案件固有の必要が生じても project のトップ階層を増やさず、まず `.workspace/` 内の既存区分へ配置する。

```text
paper-lecture-notes/
  AGENTS.md
  <project-id>/
    inbox/
    draft/
    out/
    .workspace/
```

- `inbox/`: ユーザーが原論文、追加指示、画像、データ、コード等を置く受け入れ口。原則 read-only とし、エージェントは中身を編集、改名、移動、削除、上書きしない。権利不明、秘密、巨大なファイルを自動で Git に追加しない。
- `draft/`: ユーザーが確認する途中成果。PDF と、必要なら `STATUS` や変更要約を置けるが、人手編集する正本にはしない。
- `out/`: 検証済みかつ承認済みの最終成果だけを置く。ビルドコマンドの直接出力先にしない。
- `.workspace/`: 通常ユーザーが触らない内部領域。ただし秘密保管場所ではない。`source/` は人が修正する canonical source、`records/` は永続台帳、`tools/` は案件固有の設定・adapter・lockfile の永続領域とする。`build/`、`cache/`、`tmp/`、`logs/` は再生成可能領域である。Git で扱える範囲は後述の repository mode と権利条件に従う。

`.workspace/` 全体をキャッシュや削除可能領域とみなしてはならない。`.workspace/source/`、`.workspace/records/`、`.workspace/tools/` を clean で削除しない。原論文、データ、コード、参照情報と所在は案件固有の private data として扱い、private mode でも保存・commit の権利と契約条件を個別に確認する。

複数案件で再利用できる prompt、template、script、build/QA tool、test、fix は案件名、原文、書誌、研究内容、ファイル名、権利情報等を除去して generic 化し、privacy review と共通の検証を通して repository root の public shared system へ upstream する。generic な改善を `.workspace/` だけに残さない。案件固有の data、config、adapter は private area に置く。

## プライバシーと Git 境界

repository mode は親の規則と repository root の `.system/repository-mode` に従う。

- public mode では、各 project directory、その `inbox/`、`draft/`、`out/`、`.workspace/` と全内容、project 名、manifest のファイル名・hash、PDF、原稿、講義ノート、台帳、案件固有の data・config・tool を stage、commit、push しない。この collection 内で Git 対象にできるのは privacy review 済みの共有 `AGENTS.md` と既存の公開 skeleton だけである。
- verified private mode では、configured remote の同一性と GitHub visibility が `PRIVATE` と検証できる場合に限り、project directory と4 private 領域を含む processing document を通常の `git add` で stage、commit できる。互換用の repository root の `.system/repository-mode add -- <paths>` も利用できるが、`git add -f` は使わない。
- private mode でも credential、secret、token、鍵、契約・ライセンス上保存できない data、権利上 commit できない原資料は commit しない。private remote の存在は取得、保存、利用、配布の権限を与えない。
- `out/` の配布承認と GitHub repository の visibility・commit 可否は別に判定する。配布承認済みでも GitHub に置けるとは限らず、private GitHub に置けても配布できるとは限らない。

`.gitignore`、repository root の `.system/repository-mode`、hook を回避せず、`git add -f` や `--no-verify` を使わない。status、diff、作業報告にも、私的な原文、タイトル、著者、研究内容、ファイル一覧、hash を必要なく転記しない。この構造と ignore 規則は defense in depth にすぎず、secret store、暗号化、アクセス制御の境界ではない。

## 永続データの配置

- `.workspace/source/`: 講義ノート本文、成果用に作成した図表、再現可能な計算コード等の canonical source。
- `.workspace/project-id`: initializer が作る project identity。direct-child project では directory basename と完全一致させ、人手で別 project の ID に変更しない。
- `.workspace/records/bibliography.*`: Paper ID、著者、題名、掲載先、年、DOI/URL、版・改訂、参照日、ライセンス・利用条件、権利、追加出典。
- `.workspace/records/claim-trace.*`: 主張、仮定、適用範囲、根拠、限界と、原論文の節・ページの対応。
- `.workspace/records/equation-trace.*`: 式、定理、導出、変形、仮定、再計算と元番号の対応。
- `.workspace/records/figure-trace.*`: 図表、実験値、データ、元番号、扱い、権利、再描画・再現条件の対応。
- `.workspace/records/interpretation-questions.*`: 解釈、外部知識、未解決の疑問、反例、再現不能、次の確認方法。
- `.workspace/records/recalculations.*`: 次元、符号、境界、特殊例、数値、小規模再現等の独立検証。
- `.workspace/records/paper-ingestion-manifest.*`: `inbox/` の検出履歴と、採用した入力スナップショット。人向け履歴を Markdown に残してよいが、gate と build receipt が読む現行判断は harness の `record-ingestion` が管理する JSON に記録する。
- `.workspace/records/evidence/registry.json`: harness が管理する asset、visual、resource、software、run、relation、本文参照、build receipt の機械可読正本。手作業で構造を書き換えず、harness の command で更新する。
- `.workspace/records/evidence/last-audit.json`: 最新 phase gate の対象 catalog fingerprint、finding、結果。過去 cycle の review record の代用にはしない。
- `.workspace/review/visuals/`: source hash、注釈、caption、本文参照、lineage に結合された private な全ページ・全 frame PNG と review manifest。active package の frame を cache として削除・加工せず、registry の preparation receipt と一致させる。
- `.workspace/records/` のその他の用途別台帳: 学習スコープ、QA、公開判定等。台帳名と形式は案件開始時に決める。
- `.workspace/tools/`: 採用後の案件固有のビルド設定、adapter、依存 lockfile。再利用可能な prompt、template、script、build/QA tool、test、fix の正本はここに置かず public shared system へ upstream する。

生成される `.workspace/source/generated/evidence-macros.tex` と `evidence-index.tex` は canonical catalog から作る reader-safe view であり、人が編集する正本ではない。note source は両方を `\input` し、asset、visual、resource、software、run の全 current ID を索引に載せる。ただし local path、cache path、command、environment、未公開 URL、hash、添付名・archive member 名等の private ledger field は自動公開しない。各項目は `visibility=reader|internal`、`privacy_review_status`、`public_summary` を持ち、reader 表示は privacy review 完了後の field だけ、internal 項目は ID と非公開表示だけにする。本文の実質的な説明では `\EvidenceAsset`、`\EvidenceVisual`、`\EvidenceResource`、`\EvidenceSoftware` で ID を参照する。独自図の caption は `\EvidenceOriginalCaption` を使い、「本ノート独自図」の表示を caption から外さない。

索引参照を掲載の代用にしない。`include` または `explain` とした visual は、実際の `\includegraphics`、`\input` その他の採用 toolchain の描画命令と同じ figure/table scope に `\EvidenceVisualPlacement{VIS-...}{ASSET-...}` を置き、第2引数で実際に描画した asset を示す。元 visual と別の再描画・改変 asset を掲載する場合は、その asset または対応する visual から元 visual への `derived-from` relation を先に登録する。説明に必要として `explain` とした source code、notebook、再現用 script は、読者が追える範囲を listing 等で本文へ実際に掲載し、同じ listing scope に `\EvidenceCodePlacement{ASSET-...}` を置く。長大な code 全体を無条件に転載せず、本文理解に必要な excerpt と entry point、入力・出力、版を示し、残りは権利と再現条件を確認した `include` の evidence reference へ分離する。placement macro だけを置いて図や code 本体を省略してはならず、生成 macro が PDF に出す placement 表示も削除・再定義しない。

## inbox 受け入れプロトコル

ingestion と読解・執筆を分離し、受け入れが確定したスナップショットだけを制作入力にする。

1. 作業開始時、各処理バッチ開始時、PDF を `draft/` へ公開する直前、`out/` へ昇格する直前に `inbox/` を再走査する。
2. 検出結果を `.workspace/records/paper-ingestion-manifest.*` に追記する。各 revision について、少なくとも相対パス、バイトサイズ、更新時刻、強い content hash の方式と値、初回検出時刻、処理状態、採用・保留・除外理由を保持する。採用する revision は全内容を読み取った強い content hash を必須とし、省略しない。
3. 同一性をパス、ファイル名、時刻だけで判断しない。採用前に時間を置いて2回以上検査し、各回で hash 計算の直前と直後の size、mtime 等の利用可能な stat が一致し、全内容の読み取りが成功し、全回の強い content hash が一致することを要求する。同名、同 size/mtime の内容変更も hash で新 revision として検出し、版、主張、式、図表、権利、既存 PDF への影響を評価する。
4. 書き込み・同期・ロック中、一時拡張子、stat/hash の不一致、読み取り・parse 失敗は `pending` として処理しない。ユーザーの完了合図は安定確認の代替ではなく再検査のトリガーに限り、長い固定 sleep を前提にしない。採用後は権利・機密上許される場合に限り、immutable snapshot を `.workspace/` 内の案件に適した永続・保護領域へ保存し、その所在と hash を manifest に記録する。保存しない、または保存できない場合は、入力を使用するたびと `draft/`・`out/` への公開直前に記録済み hash を再検証する。`.workspace/` 自体を秘密保管場所とはみなさない。
5. アーカイブは自動展開しない。形式、path traversal、symlink、巨大展開、入れ子、暗号化の有無を先に検査し、安全と判断したものだけを `.workspace/tmp/` の隔離先へ展開する。展開物も個別に受け入れ判定する。
6. PDF、Office、画像、データ、コード等を信頼して実行せず、マクロ、埋め込み、外部リンク、偽装形式、実行依存に注意する。必要なら内容を検証し、関連性、権利、重複、既存成果への影響を記録する。
7. ユーザーが入力を削除しても manifest の検出・採用・削除履歴は消さず `missing` として影響を確認する。制作中の追加・更新を見つけても、既存ユーザー入力を移動・削除せず、作業を黙って巻き戻さない。
8. 並行処理では担当 snapshot を分け、同じ manifest や canonical source を競合更新しない。更新の所有者を一つにするか、直列に統合する。
9. `draft/` への公開時に、採用した manifest の snapshot/revision を `STATUS` または変更要約へ記録する。
10. `out/` 昇格直前の再走査で、未評価の安定ファイル、変更 revision、`pending`、新規 `missing`、採用 revision の欠落、読み取り失敗、hash の計算または再検証そのものの失敗、または記録済み hash との不一致があれば停止して報告する。例外進行は、影響評価とユーザー合意を manifest に記録した場合だけ認める。
11. manifest の走査と同じ accepted snapshot に対して evidence harness の `scan` を行う。manifest と catalog の asset ID、hash、採用状態に差があれば、どちらかを黙って正本扱いせず不整合を解消する。

複数の原論文を採用する project では、原論文ごとに project 内で一意かつ安定した Paper ID を与える。Paper ID は `PAPER-` で始まり、大文字 ASCII 英数字を hyphen で区切る形式（例: `PAPER-MAIN`、`PAPER-SUPPORT-2`）に固定する。`primary`、`co-primary`、`supporting`、`background` の役割、採用 revision、成果物内の担当 scope を bibliography と ingestion manifest に記録する。同一論文の版・改訂は別 Paper ID に黙って分割せず同一 Paper ID の revision として差分を評価し、別論文を同一 Paper ID に畳み込まない。claim、equation、figure、quotation の各 trace は Paper ID を省略せず、複数論文を合成した説明は各根拠と執筆者の統合判断を分離して追跡する。

## draft と out

- `draft/` の PDF は途中成果であり final と呼ばない。`out/` への移動・複製は、完了条件を確認した明示的な promotion とする。
- 公開済み draft を何らかの変更後に再公開する場合は、affected work unit の再 review を完了し、変更後 snapshot に対する gate と `draft/STATUS.*` の review 状態を更新する。
- 各制作・レビュー cycle の終了時に、その時点の完成 unit と WIP を含む通読可能な draft PDF を作る。WIP は本文中で開始・終了と未完了範囲を明示し、欠落した証明や未検証の主張を完成済みに見せない。
- 公開状態を project 固有の `draft/STATUS.*` へ記録し、少なくとも project ID、ビルド日時、成功・失敗・陳腐化、source revision または commit、採用した inbox snapshot、採用 Paper ID と revision、work unit ごとの `reviewed` / `WIP` / `blocked`、検証状態、残件、対応する PDF 名を含める。direct-child project の PDF 名は project ID から始め、別 project の PDF と同名にしない。harness 用に `evidence_catalog_fingerprint`、`evidence_source_fingerprint`、`evidence_ingestion_fingerprint`、`evidence_pdf`、`evidence_pdf_sha256` を一行一 field で併記する。`reviewed` は該当 unit の必須 review gate を満たした場合だけ付与する。
- 新しいビルドを開始した時、inbox revision や canonical source が変わった時、またはビルドが失敗した時は、残っている古い成功 PDF を最新版と誤認しないよう `STATUS` を先に `building`、`failed` または `stale` に更新する。
- `out/` の既存成果を直接上書きする前に、対象名、版、検証結果、承認を確認する。可能なら成果物と共にビルド日時、source revision/commit、採用 inbox snapshot、検証状態を記録する。`out/` に含める work unit の WIP は 0 件でなければならず、WIP 表示を外すだけで promotion gate を通過したことにはしない。
- ビルド失敗時は `out/` を変更しない。`draft/` と `out/` の公開物を clean 対象にしない。

## ツールチェーンと latexmk 契約

- LaTeX、Typst、Pandoc 等をこの指示だけで固定しない。既存方式を優先し、新規案件では数式、参考文献、相互参照、コード・図表、再現性、日本語組版を比較してユーザー確認後に最小構成を作る。
- 長編日本語数学書・講義ノートの組版は、別要件またはユーザーの明示合意がなければ親 `AGENTS.md` の `bxjsbook` / upLaTeX + dvipdfmx の house-style 基準に従う。参照成果物が指定された場合は同基準の比較項目を一組として検証し、内容固有 macro を移植せず、actual-size と fit-width の render を確認する。
- 合意前に大量のテンプレートや依存を追加しない。既存エンジンを uplatex から LuaLaTeX 等へ無断で変えない。
- LaTeX/latexmk を採用する案件では、ローカル latexmk 4.83 の `-outdir`、`-auxdir`、`-emulate-aux-dir` 相当の分離機能を利用できる。`out_dir` 相当を `.workspace/build/out`、`aux_dir` 相当を `.workspace/build/aux` とする。
- latexmk が生成する PDF、DVI、SyncTeX、log、aux、fls、fdb 等は、まず全て `.workspace/build/` 配下へ置く。`draft/` や `out/` を latexmk の直接 outdir にしない。
- latexmk の終了コード成功、生成 PDF の存在と今回の更新、ログ、未解決参照、引用、フォント、ページを確認する。その後 PDF だけを `draft/` 内の一時名へコピーし、同一ファイルシステム上の rename 等で原子的に公開する。`STATUS` は公開 PDF と同じ検証結果を指すよう更新する。
- 失敗時は古い PDF を最新とみなさず `STATUS` を `failed` または `stale` にし、`out/` は不変とする。完了条件を満たした draft PDF だけを明示的に `out/` へ promotion する。
- clean は `.workspace/build/` 等の再生成領域に限定する。相対パス、`-cd`、BibTeX、MakeIndex、glossary、画像参照は案件の実文書で検証する。
- 現時点では実文書がなく未検証のため、latexmkrc や build script は作らない。案件開始時に採用方式を確定し、`.workspace/tools/` に最小設定を作って実ビルドで検証する。

## paper evidence harness の運用契約

repository root から、事前に確定した current project root を第一引数として agent が次の command を実行する。`<project-root>` は通常 `paper-lecture-notes/<project-id>` であり、legacy singleton を同一案件として継続するときだけ `paper-lecture-notes` を許す。shell の current directory、直前に扱った project、既定値から推測せず、各 command に同じ project root を明示する。ユーザーに内部 command の暗記や JSON の手編集を要求しない。

```sh
.system/paper-evidence-harness init <project-root>
.system/paper-evidence-harness scan <project-root>
.system/paper-evidence-harness prepare-visual-review <project-root> ASSET-...
.system/paper-evidence-harness record-visual-review <project-root> VREV-... --target ASSET-... --role primary ...
.system/paper-evidence-harness status <project-root>
.system/paper-evidence-harness gate <project-root> --phase internal-wip
```

- `init`: private な catalog、config、asset/figure/software-output 用 canonical directory、quarantine cache、生成 index を初期化する。既存 source や input を上書きしない。
- `scan`: `inbox/` と canonical source を content hash で再走査する。通常の大容量 dataset は一回の bounded streaming hash だけで inventory し、text は上限以下だけを memory capture する。走査開始時 size を read loop の hard limit とし、走査中の増大・短縮を不安定状態として拒否する。PDF と複数 member archive だけを設定上限・空き容量確認後の private immutable parse snapshot に掛け、解析前後の SHA-256 を一致させ、外部 parser 自体にも memory、CPU、file size、stdout/stderr capture の上限を課す。既知の binary 画像、PDF、圧縮形式は拡張子だけでなく magic を照合し、SVG は外部 entity を取得・展開しない bounded XML parse で文書全体と root 要素を検査する。単一 stream の gzip/bzip2/xz は magic header を検証してから、展開せず一 member として header-only index する。7z/rar 等の未対応形式は自動展開せず、採用するなら対応可能な安全な形式へ別 asset として固定し、除外するなら理由を記録する。PDF metadata、caption 候補、raster object、annotation URL、添付名、archive member、source URL、repository 候補、本文の evidence macro を catalog に統合する。同一 size・mtime を同一 revision と仮定しない。
- `record-ingestion`: current inbox asset の ID、revision ID、採用・除外・保留と理由を、走査済み size、mtime、SHA-256 に結合して機械可読 manifest に記録する。direct-child project で採用する入力は `--source-kind` を必須とし、原論文には `--source-kind paper --paper-id PAPER-... --paper-role primary|co-primary|supporting|background --paper-scope <担当範囲>` を全て指定する。補助資料を原論文へ結び付ける場合は `--parent-paper-id PAPER-...` を使う。採用には、config の最小間隔以上離れた別 scan による同一 hash の安定観測を二回以上要求する。JSON を手編集しない。
- `annotate` / `add-visual` / `add-resource` / `add-software` / `relate`: 採否、rights、reader/internal visibility、privacy review、読者向け要約、由来、alt text、完全性確認、手動発見項目、lineage を型付き field と edge で記録する。PDF 添付と通常 archive member は private ledger に名称・sizeを全件残し、個別に `adopted` / `excluded` / `deferred` と理由を解決する。XLSX、NPZ、DOCX 等の署名検証済み container は一つの asset として完全性 review し、内部 member は全件を private ledger に索引するが、個別解決を強制せず名称も reader index に出さない。採用した通常 member は別の current inbox asset、ingestion decision、`supplements` edgeへ結ぶ。完全性 review を `complete` にするときは reviewer を記録する。caption のない図や landing page から手動発見した公式資源を catalog 外に残さない。
- `prepare-visual-review`: PDF は MuPDF で全対象ページ、raster/SVG は形式を明示した ImageMagick decoder で全 frame を private PNG に decode/render する。decoder には live path や書込み可能な一時 path を直接渡さず、走査済み SHA-256 と照合してから unlink した read-only FD だけを選択した子processへ継承し、decode 前後に bytes、hash、size、inode を再照合する。PDF page 数と raster frame inventory を先に固定し、期待数と出力数の完全一致、連番、frame directory の全 file を receipt と再照合する。外部参照、CSS data URL、入れ子の data SVG を持つ SVG を拒否し、埋め込みを許すのは size 制限内の base64 raster で MIME と実 bytes の magic が一致するものだけとする。拡張子・magic の不一致、symlink を含む review package も拒否し、package 公開と再検証は no-follow directory descriptor に固定する。process、時間、memory、入力・出力 byte、frame 数、寸法を制限する。package は source bytes だけでなく対象注釈、caption、locator、関係 edge、対象を参照する canonical source file の hash に結合するため、図の説明・由来・本文対応を変えたら古い review は失効する。decoder は科学的 verdict を自動生成しない。
- `record-visual-review`: review package の全 frame を Codex が useful zoom で読み、decode/render、crop・欠落、source completeness、panel と読み順、軸・単位・scale、凡例・色・記号、統計表現、生命科学 context、caption と本文、geometry と semantics、科学的解釈、accessibility の全項目を `complete` / `not-applicable` / `blocked` で記録する。decode/render、crop・欠落、source completeness の3項目は `pass` なら必ず `complete` とし、`not-applicable` にできない。primary と、それに依存する別 identity の independent review を immutable receipt として保存し、厳格 gate を満たす pair は双方 `reviewer-kind=codex` とする。human/domain-expert receipt は判断不能事項の補助・escalation 記録には使えるが、Codex pair の代替にはしない。同じ reviewer の二重確認、未記入 checklist、改変 frame、stale source/caption/provenance、open finding を持つ `pass` は拒否する。
- `verify-links`: 選択した direct URL だけを検査する。credential を含む URL、HTTP、非標準 port、localhost、private・link-local address、過剰 redirect は拒否し、secret らしい query は catalog で redaction して blocking finding にする。初回と各 redirect の宛先を検査し、検査済み public IP に TLS hostname verification 付きで接続する。
- `fetch-resource`: 選択した一資源だけを size 制限付きで content-addressed quarantine cache に取得し、最終 URL、media type、size、SHA-256 を記録する。archive を自動展開せず、検査後に採用する場合だけユーザー入力を変更しない形で新しい accepted snapshot とする。
- `fetch-software`: GitHub、GitLab、Bitbucket の HTTPS Git repository を exact commit に解決し、checkout、hook、submodule、Git LFS object、外部 code の実行を行わない content-addressed bare partial clone として cache に保存する。Git環境は allowlist から構築し、外部 helper へ書き換える環境設定を継承しない。revision は wildcard・rev 式を含まない exact ref 名または full object ID に限り、ref lookup の論理候補が一意でなければ拒否する。clone/cache は時間・memory・file size・総量・entry 数、tree inventory は出力 byte・file 数の上限を持ち、gate ごとに取得時の cache entry/size、requested/resolved revision、対象 commit の tree inventory receipt と再照合する。GitHub の repository identity は大文字小文字を統一し、`releases/tag/<tag>` は tag だけを解釈し、`tree/<branch>/<path>` と短縮 commit は曖昧なため explicit revision が与えられるまで取得しない。同一 repository から異なる revision が発見された場合も一つへ黙って折り畳まず、全候補を記録して explicit revision が指定されるまで取得しない。自動発見元が全て消えた software は missing とし、手動登録した software だけを発見元なしでも保持する。revision 指定を変えたときは旧取得 receipt を無効にして再取得する。PyPI、CRAN、Bioconductor、Zenodo release 等は、公式 download を resource として size/hash固定した後、artifact 型 software と version label に結ぶ。license candidate、submodule/LFS の要確認状態、tree の file inventory を記録し、採用時は submodule/LFS の利用・除外を解決する。mutable branch 名や release 名だけを最終再現条件にしない。
- `record-run`: harness 外で明示的に隔離・承認して行った実行の receipt だけを記録する。harness は外部 code を自動実行しない。run ID は immutable とし、再実行は新しい ID で記録して古い execution lineage を改変しない。環境、command、exact software revision、dependency lock または container digest、seed、input ID、output hash と、記録時に直接生成した visual ID を固定し、dependency lock は path、size、hash を gate ごとに再照合する。input→run、output/direct-output-visual→run、run→software の execution relation を生成する。後続 scan の visual 候補や手動 panel は run 直結を自動要求せず、resource→run の `documents` 等、execution receipt でない型付き関係は追加できる。入力がない生成処理はその理由を記録する。
- `render-index`: catalog fingerprint 付きの TeX/Markdown view と、本文参照・visual placement・code placement 用 macro を再生成する。source または catalog が変わった後の古い生成 index や macro を共有しない。
- `record-build`: `.fls` の exact path で生成 macro、index、全 canonical TeX source が実際に読まれたことに加え、各 included/explained visual の明示された backing asset と各 explained code asset が実ビルドで読まれ、placement 固有表示が PDF text に描画されたことを検査する。visual が元 asset と別の backing asset を使う場合は `derived-from` lineage も要求する。構造検証済み PDF の hash・頁数、自動計算した source fingerprint、ingestion fingerprint、offline build の成否、および placement binding を current catalog fingerprint に束縛する。任意の revision label、索引上の ID、未ビルド source 上の placement macro、または backing input だけでは build receipt としない。
- `gate`: `internal-wip`、`shared-draft`、`promotion` の順に fail-closed 検査を行う。interrupted transaction、input の same-size 差し替え、二回安定観測、ingestion/status/build の同一 fingerprint、orphan ID・型違い・cycle edge、stale index、未分類項目、由来・rights・alt text・本文参照の欠落、図と説明用 code の本文 placement・backing build input の欠落、included/explained source document と visual の current primary/independent review pair、未固定 software、cache bytes/commit/tree inventory、生成物と exact run の断線、危険 archive、未解決 attachment/member、secret URL、current build receipt の欠落を検査する。

`internal-wip` は pending を warning として許すが、安全・参照整合性・snapshot freshness の error は許さない。`shared-draft` は読者に見える全項目の採否、権利、由来、説明、current build receipt と上記の二重 visual review を要求する。`promotion` はさらに pending 0、current catalog に対する offline build receipt を要求する。decoder/render の成功を科学的意味の自動判定と混同せず、Codex が図の内容を明示的に読み、根拠、限界、`not-applicable`、finding を receipt に記録する。tool と二重 review は誤りの可能性をゼロにする保証ではないため、判断不能事項だけを隠さず escalation する。

## 標準ワークフロー

1. `inbox/` を受け入れプロトコルどおり走査し、対象論文と版、目的、読者、前提知識、範囲、深さ、公開範囲、既存ツールチェーンを確認する。
2. 各原論文に project 内で一意な Paper ID と役割を付け、書誌、版・改訂、ページ方式、権利を bibliography と各 trace に登録する。補助資料は親 Paper ID との関係を持つ別 asset ID とする。
3. evidence harness を `init`、`scan` し、原論文、supplement、全 local file、図表候補、直接 URL、repository/software 候補を ID 化する。`prepare-visual-review` で全ページ・全 frame を render し、primary Codex と別 identity の independent Codex が完全性と科学的意味を review して自動抽出漏れを補い、採否と権利を記録する。
4. 問題設定、主要主張、前提、定義、手法、証明・導出、実験、限界、関連研究、著者の未解決点を分解する。
5. ノートの項目を原論文の節、ページ、式・定理・図表番号、付録、asset、resource、software、run ID へ対応させる。
6. 途中式、次元、符号、極限、境界条件、数値例、特殊例、反例、実験条件を検討し、可能なら独立に再計算・小規模再現する。
7. 学習目標と前提知識を示し、動機、直観、定義、主張、導出、図解、例、限界、節末要約の順に教育的に再構成する。
8. 原論文、解釈、外部知識、未解決の疑問を識別し、不確かな点を推測で埋めない。
9. 引用精度、主張の強さ、仮定、式・図表対応、理解検証、および途中の推論・導出・計算・図の読み方・software 再現条件に欠落がないことをレビューする。
10. 数学を含む範囲では、work unit 完成時と semantic change 後の checkpoint で repository root の `MATH_PROSE_REVIEW.md` の affected phase を authoring 担当とは分けた read-only review phase として実施し、finding と再検証結果を private records に保存する。structural change と editorial change は親 `AGENTS.md` の分類に従って検査範囲を絞る。
11. WIP 制作中は変更箇所を incremental build し、checkpoint で受け入れ snapshot、catalog fingerprint、影響範囲を再確認する。読者へ共有する draft または `out/` 候補ではログ、evidence build receipt、全ページ表示を検証して公開する。
12. 権利、ライセンス、配布範囲、不要物混入、直前の inbox と catalog 状態を確認し、承認後にだけ `out/` へ昇格する。

## work unit と review cycle

- 制作物を追跡可能な work unit に分ける。work unit は、少なくとも claim、definition、method、proof、derivation、experiment の各 subsection、または同程度に独立して authoring・review・status 判定できる範囲とする。各 unit に安定した一意の ID、対象 source revision、依存 unit、対応する trace 項目を与える。
- 細かな編集は checkpoint まで batch する。unit 完成時と semantic change 後の checkpoint で affected-unit review を行い、影響範囲、finding、対応、再検証、review status を private records に記録する。structural change は対象を絞った検査、editorial change は差分確認、incremental build、必要な変更ページの render を行い、意味・定義・参照・依存・文書構造へ影響しない unit の独立 review を再度開かなくてよい。
- 数学を含む semantic change と、意味・定義順序・依存・読者解釈へ波及する structural change の affected unit は、authoring 担当とは別の担当による独立 read-only review として repository root の `MATH_PROSE_REVIEW.md` の unit gate を適用する。blocking gap、unlabeled formal claim、broken reference、definition-before-use violation、未追跡依存のいずれかがあれば `reviewed` にしない。
- cycle ごとに、採用した input snapshot、cycle 対象 unit、各 unit の `reviewed` / `WIP` / `blocked`、open finding、次の作業を draft 本文または `draft/STATUS.*` から確認できるようにする。draft 本文は WIP を隔離・表示しつつ通読可能に保ち、reviewed 部分と混同させない。
- unit review は成果物全体の review を代替しない。`out/` への promotion 前に、採用 input snapshot と含まれる全 unit を固定し、repository root の `MATH_PROSE_REVIEW.md` の全 phase と本ガイドの全体 gate を再実行する。

## 論文の分解・記述規約

- 問題設定では対象と対象外、主張では条件・比較対象・根拠・適用範囲、仮定では明示と執筆者の解釈を分ける。
- 原論文の定義、一般的な別定義、ノート内の便宜的規約を区別する。手法は入力、処理、出力、設計選択、比較、計算量・実施条件に分解する。
- 証明・導出は前提、使用結果、変形根拠、結論までを復元し、省略箇所を推測で確定しない。実験はデータ、分割、前処理、ベースライン、指標、ハイパーパラメータ、乱数、統計、アブレーション、再現条件を整理する。
- 著者が述べる限界と執筆者が考える追加の限界、原論文による関連研究の位置付けと外部調査による補足を分ける。
- 本文では少なくとも「原論文」「解釈」「外部知識」「未解決の疑問」「AI 生成の補足」を識別する。AI 生成の説明、例、導出は、出典または独立検証なしに確定本文へ入れない。
- 原論文著者の claim、引用した既知結果、reviewer の correction、reviewer の interpretation を明確に区別する。誤りが疑われても原 claim を黙って訂正せず、原文、問題、根拠、truth status、採用判断を private review record に残し、correction を著者の主張として扱わない。
- 直接引用は原文と照合して正確性を保ち、脱落・省略、原文にない強調、翻訳の併記を明示する。引用、要約、翻訳の表示と照合結果を bibliography または claim trace に記録する。
- 相関を因果へ、限定結果を一般結果へ、実験観察を証明済み命題へ拡張しない。否定的結果、不確実性、信頼区間、評価条件を省略して結論を強めない。
- 文体、句読点、用語、略語、原語併記、数値・単位、引用形式を用途別台帳で統一する。教育的な要約・簡略化でも条件、限界、反例を落として誤解を招かないよう claim trace と照合する。

## 講義構成・追跡・理解検証

- 冒頭に対象読者、前提知識、学習目標、論文の位置付け、読了後に説明できる事項を示す。背景知識を原論文の新規性と混同しない。
- 主要節に必要性、定義、直観、形式的説明、例、論文内の役割、節末要約を置く。演習・確認問題・読者への課題は作らず、理解と検証に必要な推論、導出、計算、場合分けはすべて本文中に記述する。
- 本文で正式に導入した専門用語・記号の索引を設ける。索引の各ページ参照は、参照先ページへ移動する PDF 内リンクとし、複数のページ参照はすべて個別にリンクする。
- 原論文由来の主張、式、定義、数値、図表を Paper ID、版、ページ、節、番号まで追跡する。式を変形・改番した場合は元番号と変更内容を記録する。
- 導出を次元、符号、境界値、極限、簡単な入力、既知の特殊例で検算する。条件を外した反例や適用範囲外も検討する。
- 実験値を再計算できる場合は式と丸めを確認する。再現不能なら不足情報、試行条件、次の確認方法を記録し、滑らかな文章で隠さない。
- 統計的結果は estimand、sample、欠測・除外、前処理、model、検定、multiple comparison、effect size、不確実性、乱数、software/version を必要な粒度で示す。図の error bar、band、asterisk、色、shape、facet、normalization、aggregation を本文で読めるようにし、相関・予測・因果を混同しない。
- 生命科学の図は species、strain、cell line、sample 数、replicate、対照、処置、時間、濃度、assay、stain/marker、scale bar、画像処理、panel 間の関係を原資料に応じて説明する。模式図と測定像、代表像と集計、technical replicate と biological replicate を区別する。
- 原図だけでは理解に必要な関係が見えにくい場合、権利と忠実性を確認した再描画、または講義ノート独自図を追加してよい。独自図は原論文由来に見せず caption に「本ノート独自図」と明示し、使った source/data/software と relation を登録する。忠実再描画と改変図も origin を分け、改変点を説明する。
- 原論文と外部資料の書誌、版、DOI/URL、ページ、参照日、ライセンス・利用条件、権利を bibliography に記録する。直接引用、要約、翻訳に出典を示し、孫引きなら明示する。
- 図表、付録データ、コード、非公開査読資料、個人情報、契約制限データには別の利用条件があり得る。判定を bibliography、figure trace またはQAへ記録し、秘密情報、権利不明素材と共にコミット・公開しない。

## 数式・図表・PDF 品質

- 数学を含む成果物は repository root の `MATH_PROSE_REVIEW.md` の独立 read-only review と再レビューを完了する。definition-before-use violation、未追跡の仮定・依存、open blocking finding がいずれも 0 件になるまで `out/` へ promotion しない。scope 除外で対応する場合も、影響とユーザーの明示合意を private record に残して gate を再実行する。
- formal claim は、意味に応じた番号付き semantic environment（`theorem`、`lemma`、`proposition`、`corollary`、`definition` 等）に置き、自動生成番号と一意で安定した `label` を付ける。形式的主張を無番号の強調文や通常 prose だけで提示しない。
- 各 formal proof は明示的な proof environment に置く。直前の theorem-like statement を証明する通常の proof は、見出しを原則として「証明」とする bare な無番号 proof とし、proof 番号、proof 自体の `label`、対象 statement への明示的な cross-reference を付けない。statement から離れた証明には対象 statement を `label` で示す無番号の説明見出しを用いてよい。番号付き proof は、複数の証明・別証明を区別する場合、または proof 自体を他所から参照する場合に限り、一意で安定した `label` と対象 statement への cross-reference を持たせる。必要な statement、番号付き proof、definition、equation 間の参照は番号の直書きではなく `label` による cross-reference とし、prose だけの形式的証明を作らない。
- proof と derivation は、前提、依存結果、各変形の根拠、場合分け、境界条件から結論までを明示して完結させる。「明らか」「同様」「容易に分かる」等で非自明な段階を省略しない。derivation の主要な段階は番号付き・ラベル付きの式または適切な semantic environment に置き、本文から参照できるようにする。
- 原論文の claim・proof と、ノート執筆者による reconstruction・補足導出・correction を、見出し、環境名、注記等で読者が明確に識別できるようにする。再構成や訂正を原論文の主張として表示せず、その根拠と truth status を trace および review record に対応付ける。
- 記号は初出で定義し、型、次元、定義域、添字範囲、確率変数か実現値かを必要に応じて示す。原論文と記号を変える場合は対応表と理由を残す。
- 式変形の仮定、定理、近似、極限操作を明記し、近似と等号を混同しない。式番号、本文参照、演算子、書体を統一する。
- 図表ごとに出典、元番号、ページ、引用・再描画・改変・新規作成の別、権利を figure trace に記録する。値、軸、単位、凡例、集計条件、番号、キャプション、本文参照、白黒・縮小時の可読性、アクセシビリティ、画像解像度を確認する。索引または caption だけで掲載済みとせず、本文 PDF に図表本体が描画され、placement binding と build input が一致することを確認する。
- 手法、再計算、再現条件の理解に code が必要な場合は、該当 revision に結合した必要最小限の listing を本文へ掲載し、前提、入力、主要処理、出力、論文内の役割を説明する。repository URL、software ID、ファイル名、実行 receipt だけを code 掲載の代用にせず、code placement と build input を照合する。秘密、credential、権利上掲載できない code は掲載せず blocking または明示した scope 除外として扱う。
- 原論文・supplement の全 figure/table/panel に disposition を与え、本文で扱うものは図の目的、読み順、各 panel、軸、単位、符号、色・線・記号、sample、統計、比較対象、結論と限界を説明する。省略する場合も index から消さず、理由を表示する。
- note visual は全て catalog、caption、本文参照、origin、rights、alt text を持つ。`source-original`、`permitted-copy`、`faithful-redraw`、`adapted`、`note-original`、`software-output` を混同せず、redraw/adapted は元 visual、software output は exact software/run/input へ edge を持たせる。
- ビルド成功だけで完成としない。エラー、警告、未解決参照、重複ラベル、欠落引用、参考文献、目次、索引、しおり、式・図表番号を確認し、索引の全ページ参照が PDF 内リンクとして機能して正しいページへ移動することを検証する。
- 日本語・欧文・数式・コードのフォント、埋め込み、文字化け、行送り、禁則、長い URL、脚注、余白、ページ番号、はみ出しを確認する。最終候補は全ページを目視またはレンダリング検査する。
- 検証記録には実施日、対象 source revision、採用 inbox snapshot、引用・文体・権利・図表を含む実施項目、結果、残件を残す。Git 差分に原資料、中間物、秘密情報、権利不明素材、無関係な変更がないことを確認し、台帳またはQAに未解決の不備があれば `out/` へ promotion しない。

## 禁止事項

- collection root または別 project を current project root と誤認して harness、build、review、draft 同期、promotion を実行すること。
- 独立成果物の原論文、canonical source、台帳、ID、fingerprint、receipt、`STATUS`、draft、out、承認を同一 project に混在させ、または project 間で流用すること。
- 論文にない主張を論文の結論として書き、条件、不確実性、限界を落とすこと。
- 原論文、解釈、外部知識、疑問、AI 生成補足を無表示で混ぜること。
- 式、図表、実験値、引用を版・ページ・番号へ追跡できないまま転載・変形すること。
- 不足情報を推測で埋め、検証済みとして扱うこと。
- 必要な推論、導出、計算、場合分けを読者向けの問題または課題として本文外へ委ねること。
- inbox の入力をエージェント都合で変更し、不安定・未評価の入力を採用すること。
- 権利不明の資料、データ、コード、非公開情報、秘密情報をコミット・公開すること。
- 合意なくツールチェーンやエンジンを固定・置換し、draft を無検証で out に置くこと。

## 完了条件

- 合意した読者・範囲に対し、前提知識、学習目標、教育的本文、節末要約を備え、演習・確認問題・読者への課題を含まず、必要な推論・導出・計算が本文中で完結した PDF が完成している。
- 主張、仮定、定義、手法、証明・導出、実験、限界、関連研究が必要な深さで分解・再構成されている。
- 原論文、解釈、外部知識、未解決の疑問が区別され、主要内容をページ・式・図表番号まで追跡できる。
- 専門用語・記号の索引が本文と一致し、すべてのページ参照が正しい参照先への PDF 内リンクになっている。
- 引用精度と理解検証が確認済みで、未解決事項と再現上の制約が明示されている。
- direct-child project の project root と `.workspace/project-id` が一致し、canonical source、台帳、build、review、`STATUS`、draft、out が当該 project の内側だけにあり、別 project への mutable path 依存と流用された gate/receipt が 0 件である。
- 複数の原論文を含む場合、全 Paper ID の役割、revision、担当 scope、相互関係が確定し、各 claim、式、図表、引用が正しい Paper ID へ追跡できる。
- 全 work unit について作成・最終修正後の affected-unit review が記録され、成果物に含まれる unit がすべて `reviewed` で WIP が 0 件である。数学を含む場合は repository root の `MATH_PROSE_REVIEW.md` の独立 read-only review が完了し、definition-before-use violation、unlabeled formal claim、broken reference、未追跡依存、open blocking finding が 0 件である。
- 全 current asset、visual、resource、software が講義ノートの evidence index に載り、採否が確定している。説明対象は本文の evidence reference を持ち、全 included/explained visual は図表本体、実描画 asset を明示した visual placement、必要な lineage を持ち、全 explained code asset は読者に必要な listing と code placement を持つ。各 backing file が current build receipt の input と一致し、各 placement 固有表示が PDF に存在している。全 source document と全 included/explained visual に current preparation と primary/independent Codex review pair があり、全 source visual の完全性確認、全 note visual の origin・rights・alt text、全 software の exact revision・license・環境・利用法・出力 lineage が確定している。
- evidence catalog、生成 index、source revision、software/run receipt、PDF build receipt、`draft/STATUS` が同じ fingerprint/snapshot を指し、`shared-draft` または `promotion` の該当 gate を通過している。`out/` は pending 0 かつ offline build receipt を持つ promotion gate の pass を必須とする。
- 合意した手順で再生成でき、警告、参照、目次、数式、フォント、全ページのレイアウト検証を通過し、参照成果物がある場合は house style を一組として actual-size と fit-width の両方で比較済みである。
- 採用 inbox snapshot、source revision、検証結果、承認が記録され、最終成果だけが `out/` に明示的に昇格されている。
- 配布物に秘密情報や権利不明素材がなく、公開・配布条件が確認されている。
