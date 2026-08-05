# Self-contained 数学講義ノート PDF 制作ガイド

## 適用範囲

この `AGENTS.md` は `self-contained-math-notes/` とその配下だけに適用する。書籍翻訳や単一論文の読解ノートへ固有規則を持ち込まない。上位の指示と衝突する場合は上位を優先し、判断できなければ作業を止めて確認する。

本 project は親の `../AGENTS.md`、`../JAPANESE_WRITING.md`、`../MATH_PROSE_REVIEW.md` と、repository root で version 管理された `../.system/` tools を shared versioned 正本として直接使う。private copy を正本にせず、案件固有の adapter が必要なら差分と理由だけを private area に記録する。日本語の原稿・訳注・講義ノートは `../JAPANESE_WRITING.md` に従う。ただし、数学ノートでは数学的正確性、self-contained 性、依存順、証明の完全性を共通規範より優先する。

## 目的

論理・集合・数学基礎から始め、合意した修士課程程度の到達範囲までを、完全に self-contained な講義ノート PDF として構築する。初学者が各局所を追える説明と、高度な章まで破綻しない定義・記号・定理依存の全体整合性を両立させる。

## Self-contained の運用定義

- 各定義、命題、補題、定理、系、証明、例、演習が必要とする依存先を明示できる。
- 語と記号を使用前または明示的な前提節で定義する。
- 非自明な結果には原則としてノート内の証明があり、既に導入済みの公理・定義・結果だけに依存する。
- 公理、論理規則、規約、定義、証明済み結果、明示的なスコープ外依存を区別する。
- 未証明の非自明な結果を暗黙の「よく知られている事実」として使わない。

self-contained は参考文献が不要という意味ではない。歴史、標準的定式化、証明比較、追加学習、明示的なスコープ外依存の出典は正確に示す。

想定成果物は、基礎から合意した修士水準までの講義ノート PDF、再現可能な編集ソース、カリキュラムとスコープ、基礎・定理依存・記号の台帳、演習メタデータ、参考文献、索引、相互参照、証明・PDF の検証記録である。

## ワークスペースの契約

ユーザーから見える作業領域は次の4つに固定する。管理用の本 `AGENTS.md` だけはトップに置く。案件固有の必要が生じても、トップ階層を増やさず、まず `.workspace/` 内の既存区分へ配置する。

```text
self-contained-math-notes/
  AGENTS.md
  inbox/
  draft/
  out/
  .workspace/
```

- `inbox/`: ユーザーが参考資料、追加指示、画像、データ等を置く受け入れ口。原則 read-only とし、エージェントは中身を編集、改名、移動、削除、上書きしない。権利不明、秘密、巨大なファイルを自動で Git に追加しない。
- `draft/`: ユーザーが確認する途中成果。PDF と、必要なら `STATUS` や変更要約を置けるが、人手編集する正本にはしない。
- `out/`: 検証済みかつ承認済みの最終成果だけを置く。ビルドコマンドの直接出力先にしない。
- `.workspace/`: 通常ユーザーが触らない内部領域。ただし秘密保管場所ではない。`source/` は人が修正する canonical source、`records/` は永続台帳、`tools/` は案件固有の設定・adapter・lockfile の永続領域とする。`build/`、`cache/`、`tmp/`、`logs/` は再生成可能領域である。Git で扱える範囲は後述の repository mode と権利条件に従う。

`.workspace/` 全体をキャッシュや削除可能領域とみなしてはならない。`.workspace/source/`、`.workspace/records/`、`.workspace/tools/` を clean で削除しない。参考資料、参照情報と所在は案件固有の private data として扱い、private mode でも保存・commit の権利と契約条件を個別に確認する。

複数案件で再利用できる prompt、template、script、build/QA tool、test、fix は案件名、資料内容、書誌、ファイル名、権利情報等を除去して generic 化し、privacy review と共通の検証を通して repository root の public shared system へ upstream する。generic な改善を `.workspace/` だけに残さない。案件固有の data、config、adapter は private area に置く。

## プライバシーと Git 境界

repository mode は親の規則と `../.system/repository-mode` に従う。

- public mode では、`inbox/`、`draft/`、`out/`、`.workspace/` の4 private 領域とその全内容、project 名、manifest のファイル名・hash、PDF、原稿、講義ノート、台帳、案件固有の data・config・tool を stage、commit、push しない。この project 内で Git 対象にできるのは privacy review 済みの `AGENTS.md` だけである。
- verified private mode では、configured remote の同一性と GitHub visibility が `PRIVATE` と検証できる場合に限り、4 private 領域を含む processing document を commit できる。stage は必ず `../.system/repository-mode add -- <paths>` を使い、直接の `git add` や `git add -f` を使わない。
- private mode でも credential、secret、token、鍵、契約・ライセンス上保存できない data、権利上 commit できない参考資料は commit しない。private remote の存在は取得、保存、利用、配布の権限を与えない。
- `out/` の配布承認と GitHub repository の visibility・commit 可否は別に判定する。配布承認済みでも GitHub に置けるとは限らず、private GitHub に置けても配布できるとは限らない。

`.gitignore`、`../.system/repository-mode`、hook を回避せず、`git add -f` や `--no-verify` を使わない。status、diff、作業報告にも、私的な資料内容、題名、著者、研究内容、ファイル一覧、hash を必要なく転記しない。この構造と ignore 規則は defense in depth にすぎず、secret store、暗号化、アクセス制御の境界ではない。

## 永続データの配置

- `.workspace/source/`: 章ごとの本文、証明、例・反例、演習、解答、成果用に作成した図表等の canonical source。
- `.workspace/records/scope.*`: 想定読者、開始点、到達水準、採用する論理・集合論的基礎、分野、対象外、スコープ外依存。
- `.workspace/records/curriculum.*`: 章構成、学習順序、到達目標、章間の橋渡し。
- `.workspace/records/foundations.*`: 公理、論理規則、規約、定義、既知事項、承認済みスコープ外事項。
- `.workspace/records/theorem-dependencies.*`: 定義・命題・補題・定理等の ID、直接依存、使用箇所、証明状態、依存グラフ。
- `.workspace/records/symbol-registry.*`: 用語、記号、型、意味、導入箇所、使用範囲、同義語。
- `.workspace/records/exercise-metadata.*`: 演習 ID、狙い、難度、依存節、ヒント・解答方針、検証状態。
- `.workspace/records/reference-ingestion-manifest.*`: `inbox/` の検出履歴と、採用した入力スナップショット。
- `.workspace/records/` のその他の用途別台帳: 参考文献、索引、図表、QA、公開判定等。台帳名と形式は案件開始時に決める。
- `.workspace/tools/`: 採用後の案件固有のビルド設定、adapter、依存 lockfile。再利用可能な prompt、template、script、build/QA tool、test、fix の正本はここに置かず public shared system へ upstream する。

## inbox 受け入れプロトコル

ingestion と数学的執筆を分離し、受け入れが確定したスナップショットだけを制作入力にする。

1. 作業開始時、各処理バッチ開始時、PDF を `draft/` へ公開する直前、`out/` へ昇格する直前に `inbox/` を再走査する。
2. 検出結果を `.workspace/records/reference-ingestion-manifest.*` に追記する。各 revision について、少なくとも相対パス、バイトサイズ、更新時刻、強い content hash の方式と値、初回検出時刻、処理状態、採用・保留・除外理由を保持する。採用する revision は全内容を読み取った強い content hash を必須とし、省略しない。
3. 同一性をパス、ファイル名、時刻だけで判断しない。採用前に時間を置いて2回以上検査し、各回で hash 計算の直前と直後の size、mtime 等の利用可能な stat が一致し、全内容の読み取りが成功し、全回の強い content hash が一致することを要求する。同名、同 size/mtime の内容変更も hash で新 revision として検出し、定義、証明、演習、権利、既存 PDF への影響を評価する。
4. 書き込み・同期・ロック中、一時拡張子、stat/hash の不一致、読み取り・parse 失敗は `pending` として処理しない。ユーザーの完了合図は安定確認の代替ではなく再検査のトリガーに限り、長い固定 sleep を前提にしない。採用後は権利・機密上許される場合に限り、immutable snapshot を `.workspace/` 内の案件に適した永続・保護領域へ保存し、その所在と hash を manifest に記録する。保存しない、または保存できない場合は、入力を使用するたびと `draft/`・`out/` への公開直前に記録済み hash を再検証する。`.workspace/` 自体を秘密保管場所とはみなさない。
5. アーカイブは自動展開しない。形式、path traversal、symlink、巨大展開、入れ子、暗号化の有無を先に検査し、安全と判断したものだけを `.workspace/tmp/` の隔離先へ展開する。展開物も個別に受け入れ判定する。
6. PDF、Office、画像等を信頼して実行せず、マクロ、埋め込み、外部リンク、偽装形式に注意する。必要なら内容を検証し、関連性、権利、重複、既存成果への影響を記録する。
7. ユーザーが入力を削除しても manifest の検出・採用・削除履歴は消さず `missing` として影響を確認する。制作中の追加・更新を見つけても、既存ユーザー入力を移動・削除せず、作業を黙って巻き戻さない。
8. 並行処理では担当 snapshot を分け、同じ manifest や canonical source を競合更新しない。更新の所有者を一つにするか、直列に統合する。
9. `draft/` への公開時に、採用した manifest の snapshot/revision を `STATUS` または変更要約へ記録する。
10. `out/` 昇格直前の再走査で、未評価の安定ファイル、変更 revision、`pending`、新規 `missing`、採用 revision の欠落、読み取り失敗、hash の計算または再検証そのものの失敗、または記録済み hash との不一致があれば停止して報告する。例外進行は、影響評価とユーザー合意を manifest に記録した場合だけ認める。

## draft と out

- `draft/` の PDF は途中成果であり final と呼ばない。`out/` への移動・複製は、完了条件を確認した明示的な promotion とする。
- 各 work unit の checkpoint ごとに、必要ならその時点で通読可能な PDF を `draft/` へ公開する。公開状態を `draft/STATUS.*` 等へ記録し、少なくともビルド日時、成功・失敗・陳腐化、source revision または commit、採用した inbox input snapshot、work unit ごとの `reviewed` / `WIP`、検証状態、残件、対応する PDF 名を含める。checkpoint 前の private preview は `unreviewed internal WIP` として配布対象から隔離する。
- `reviewed` と表示する work unit は、当該 input snapshot に対する独立レビューと再検証を完了し、数学的 gap、未定義語・記号、未追跡依存、壊れた参照を 0 件にする。未完の work unit は `WIP` として完成部分から隔離し、PDF 本文と `STATUS` の双方で未完了範囲と依存不能範囲を明示する。
- 新しいビルドを開始した時、inbox revision や canonical source が変わった時、またはビルドが失敗した時は、残っている古い成功 PDF を最新版と誤認しないよう `STATUS` を先に `building`、`failed` または `stale` に更新する。
- `out/` の既存成果を直接上書きする前に、対象名、版、検証結果、承認を確認する。可能なら成果物と共にビルド日時、source revision/commit、採用 inbox snapshot、検証状態を記録する。
- `out/` に含める work unit はすべて `reviewed` とし、`WIP`、unresolved blocking gap、壊れた参照、open blocking finding を 0 件にする。
- ビルド失敗時は `out/` を変更しない。`draft/` と `out/` の公開物を clean 対象にしない。

## ツールチェーンと latexmk 契約

- LaTeX、Typst、Pandoc 等をこの指示だけで固定しない。既存方式を優先し、新規案件では長期保守、定理・式の相互参照、索引、依存台帳との連携、日本語・数式組版、差分可読性、再現性を比較してユーザー確認後に最小構成を作る。
- 長編日本語数学書の組版は、別要件またはユーザーの明示合意がなければ親 `AGENTS.md` の `bxjsbook` / upLaTeX + dvipdfmx の house-style 基準に従う。参照成果物が指定された場合は同基準の比較項目を一組として検証し、内容固有 macro を移植せず、actual-size と fit-width の render を確認する。
- 合意前に大量のテンプレート、パッケージ、独自マクロを追加しない。独自マクロには意味と範囲を記録する。既存エンジンを uplatex から LuaLaTeX 等へ無断で変えない。
- LaTeX/latexmk を採用する案件では、ローカル latexmk 4.83 の `-outdir`、`-auxdir`、`-emulate-aux-dir` 相当の分離機能を利用できる。`out_dir` 相当を `.workspace/build/out`、`aux_dir` 相当を `.workspace/build/aux` とする。
- latexmk が生成する PDF、DVI、SyncTeX、log、aux、fls、fdb 等は、まず全て `.workspace/build/` 配下へ置く。`draft/` や `out/` を latexmk の直接 outdir にしない。
- latexmk の終了コード成功、生成 PDF の存在と今回の更新、ログ、未解決参照、引用、フォント、ページを確認する。その後 PDF だけを `draft/` 内の一時名へコピーし、同一ファイルシステム上の rename 等で原子的に公開する。`STATUS` は公開 PDF と同じ検証結果を指すよう更新する。
- 失敗時は古い PDF を最新とみなさず `STATUS` を `failed` または `stale` にし、`out/` は不変とする。完了条件を満たした draft PDF だけを明示的に `out/` へ promotion する。
- clean は `.workspace/build/` 等の再生成領域に限定する。相対パス、`-cd`、BibTeX、MakeIndex、glossary、画像参照は案件の実文書で検証する。
- 新規案件で実文書と採用方式が未確定の段階では、latexmkrc や build script を先行作成しない。案件開始時に採用方式を確定し、`.workspace/tools/` に最小設定を作って実ビルドで検証する。

## 全体設計と標準ワークフロー

1. `inbox/` を受け入れプロトコルどおり走査し、想定読者、開始点、到達分野と深さ、論理・集合論的基礎、古典・構成的立場、選択公理、対象外を合意して scope に記録する。
2. 論理・集合から各分野への前提関係を整理し、章の学習順、到達目標、橋渡しとなる結果を curriculum に記録する。
3. 公理、推論規則、メタレベル規約、定義、既知事項、スコープ外依存を foundations に分類する。「明らか」「標準的」を未分類の前提にしない。
4. 各定義・命題・補題・定理に安定した ID を付け、直接依存を登録し、使用前定義と循環参照を確認する。
5. 原則として「動機・学習目標 → 定義 → 例・非例 → 命題・定理 → 証明 → 応用 → 節末要約 → 演習」の順に、下記の work unit 単位で章を書く。
6. work unit の完成時と semantic change 後の checkpoint で、新規の語・記号、証明の各推論、参照先、例、演習の解答可能性を検証し、authoring 担当とは別の独立レビューを完了してから `reviewed` とする。structural change と editorial change は親 `AGENTS.md` の分類に従って検査範囲を絞る。
7. 依存グラフ、記号表、索引、相互参照を更新し、循環、重複定義、意味変化、前方依存、章間のギャップを全体検証する。
8. 初学者が局所的に追えるか、後続の修士水準で必要な厳密さ・一般性を損なわないかをレビューする。
9. checkpoint では `../MATH_PROSE_REVIEW.md` の affected phase を authoring 担当とは分けた read-only review phase として実施し、finding と再検証結果を private records に保存する。配布・promotion 前には同文書の全 phase を実施する。
10. WIP 制作中は変更箇所を incremental build し、checkpoint で受け入れ snapshot と影響範囲を再確認する。読者へ共有する draft または `out/` 候補ではログと全ページ表示を検証して公開する。
11. 権利、ライセンス、配布範囲、不要物混入、直前の inbox 状態を確認し、承認後にだけ `out/` へ昇格する。

## work unit と制作サイクル

- 追跡・制作・レビューの最小単位である work unit は、(1) definition chain、(2) theorem とその補題群および全 proof、(3) 相互に関連する example set、(4) 相互に関連する exercise set、(5) subsection のいずれかとする。依存台帳と review artifact には一意で安定した unit ID、範囲、直接依存、採用 input snapshot、source revision、状態を記録する。
- 細かな編集は checkpoint まで batch する。work unit の新規作成、完成、semantic change では affected unit の review を必須とし、定義、主張、証明、依存、意味、参照に波及する変更では直接・間接の影響範囲を台帳から求め、影響を受ける unit を `WIP` または `review required` に戻す。structural change は対象を絞った検査、editorial change は差分確認と incremental build で再検証してよい。
- 各 checkpoint は、unit の authoring、必要な台帳・相互参照の更新、変更分類に応じた `../MATH_PROSE_REVIEW.md` の review、finding の修正、同一 snapshot に対する再検証、必要な draft PDF の build・検証・公開、`STATUS` 更新を一組とする。
- semantic change の independent review は affected unit ごとに行い、定義・主張・証明・例・演習・前後の接続、直接依存と下流影響を review artifact で追跡可能にする。複数 unit を一括確認しても、unit ごとの判定と finding を失わない。
- `reviewed`、`WIP`、`review required` を混同しない。`reviewed` は記録された input snapshot と source revision に対して有効とする。semantic change または意味へ波及する structural change 後は自動継承せず、editorial change では差分と再検証結果を記録して status を更新できる。

## 数学文章レビューと promotion gate

- `../MATH_PROSE_REVIEW.md` を shared versioned 正本として全 phase を適用し、authoring と independent read-only review、修正、再レビューを分離する。
- definition-before-use audit は本文、見出し、定理・定義、証明、図表、caption、脚注、例、演習、解答、索引を対象とし、語・記号・概念の violation を全箇所で 0 件にする。教育上の予告であっても未定義語を見出しや caption に置かない。
- definition、result、proof、exercise、external dependency の ledger を本文と双方向に照合し、未追跡の仮定・依存、存在しない参照、使用前依存、循環を 0 件にする。
- self-contained scope 内の unresolved blocking gap を 0 件にする。未解決部分を scope 外へ除外する場合は truth status、影響、依存不能範囲、責任者、ユーザーの明示合意を private record に残し、残った成果物について gate を再実行する。
- open blocking finding が 0 件になるまで `out/` へ promotion しない。review artifact は private area に保存し、public Git 差分へ混入させない。

## カリキュラム・定義・証明規約

- 原則として論理、証明技法、集合・写像・関係から始め、合意した代数、解析、位相、幾何、確率・測度等へ依存順に進む。分野の採否と順序は到達目標と依存関係に基づいて合意する。
- 各章の冒頭に前提、学習目標、後続章での役割、末尾に要約、依存の追加分、演習を置く。定義直後に典型例と非例を示し、直観を定義や証明の代わりにしない。
- 定義、命題、補題、定理、系、例、反例、注意、演習を区別する。すべての formal assertion は意味に対応する番号付き semantic environment に置き、一意で安定した label を付ける。説明的 prose は動機・直観・接続に使えるが、formal statement または proof の代わりにしない。
- 定義は型、量化範囲、条件、同値定式化を明確にし、定義前に本文、見出し、caption、例、演習を含むどの場所でも本質的な意味で使わない。
- 定理は仮定と結論を明示し、すべての proof を明示的な proof environment に置く。直前の theorem-like statement を証明する通常の proof は、見出しを原則として「証明」とする bare な無番号 proof とし、proof 番号、proof 自体の label、対象 statement への明示的な cross-reference を付けない。statement から離れた証明・解答には対象 statement を label で示す無番号の説明見出しを用いてよい。番号付き proof は、複数の証明・別証明を区別する場合、または proof 自体を他所から参照する場合に限り、一意で安定した label と対象 statement への cross-reference を持たせる。必要な statement、番号付き proof、definition、equation 間の参照は label による cross-reference を使い、prose だけの形式的証明を作らない。
- proof は、非自明な各推論、各含意の向き、場合分けの網羅性と各場合、計算の各変形根拠、使用する外部結果の仮定と適用条件を明示する。「証明を省略」「同様」「明らか」「容易」「標準的」だけで step または case を代替しない。proof の十分性は語数やページ数ではなく、reviewer が statement から結論までの dependency trace を先行する定義・公理・結果・直接導出に対応させて再構成できることで判定する。
- 計算証明では変形根拠、収束、定義域、零除算回避、極限と演算の交換条件を示す。存在、well-definedness、一意性を必要な順に証明し、反例と端点で仮定を検討する。
- AI 生成の証明、例、反例は候補として扱い、全推論と依存を独立検証するまで確定しない。

## スコープ外依存と依存台帳

- 非自明な結果の証明省略は原則禁止する。例外は合意したスコープ外で、展開が主目的を逸脱する場合に限り、単に長い、難しい、未完成という理由では省略しない。
- 例外は「スコープ外依存」と明示し、正確な定式化、仮定、信頼できる出典と位置、使用箇所、採用理由を scope、foundations、theorem dependencies に登録する。
- 未証明かつスコープ外として未承認の結果を後続証明に使わない。スコープ外依存を追加する前に self-contained の境界変更をユーザーと確認する。
- 各項目に ID、種類、導入箇所、直接依存、使用箇所、証明状態を記録する。依存辺は定義、証明、記法、動機等の意味を必要に応じて区別する。
- 新規追加と変更時に、未定義語、存在しない ID、使用前依存、循環、全使用箇所を検査する。教育上の予告を論証依存に逆流させず、同値定理群には基点と各含意の方向を示す。
- 台帳と本文が食い違う場合は推測で片方を正とせず、証明内容に基づいて修正する。

## 執筆・記号・演習規約

- 数学的事実、教育上の直観、歴史的説明、未確定事項、AI 生成案を区別する。欠落を推測で埋めず、`TODO`、未証明、要確認等の検索可能な表示を残して完成判定から除外する。
- 用語、文体、句読点、全半角、定理名、人名表記、数式前後の文章を用途別台帳で統一する。簡略化には適用範囲を明記し、後続章で破綻する不正確・誤解を招く説明を scope、foundations、QAとの照合なしに採用しない。
- すべての記号を初出で定義し、意味、型、定義域、添字範囲、導入箇所、使用範囲を symbol registry に登録する。同じ記号を近接文脈で別の意味に使わない。
- 等号、同型、同値、定義による等号、近似を区別する。同一視には正当化と有効範囲を示し、式番号、本文参照、演算子、書体を PDF 上で確認する。
- 各演習に ID、狙い、難度、依存節を付ける。難度基準を定義し、定義確認、基本適用、複合推論、発展を区別する。
- ヒント、完全解答、解答方針の扱いを scope で統一する。解答は導入済み事項だけで成立させ、問題文不足、解なし、複数解釈、難度を実際に解いて検証する。
- 用語・記号の導入箇所と主要使用箇所を索引または同等の導線で検索可能にし、安定ラベルを使う。予告と論証上の前方依存を区別する。

## 出典・図表・PDF 品質

- 定義・定理の名称、歴史、証明の着想、例、演習、図表を資料から得た場合は書誌と使用箇所を記録する。直接引用は原文と照合し、脱落・省略、原文にない強調、翻訳の併記を明示して正確性を保ち、大規模転載や近接した書き換えを避ける。
- 定理自体と特定表現・証明・図版の著作物としての扱いを混同しない。参考文献・権利台帳に URL、参照日、ライセンス・利用条件を残し、孫引きは明示し、外部演習、解答、図版、データ、コードの条件を確認する。個人情報、秘密情報、契約制限素材、権利不明素材をコミット・公開しない。
- 図表ごとに目的、作成者、出典、引用・改変・新規作成の別、権利、依存定義を図表台帳へ記録する。図を証明の代わりにせず、軸、尺度、向き、境界、凡例、単位、番号、キャプション、白黒・縮小時の可読性、アクセシビリティ、画像解像度を確認する。
- ビルド成功だけで完成としない。エラー、警告、未解決参照、重複ラベル、索引、参考文献、目次、しおり、定理・式番号を確認する。
- 日本語・欧文・数式フォント、埋め込み、グリフ欠落、文字化け、禁則、行間、脚注、余白、ページ番号、数式・表のはみ出しを確認する。最終候補は全ページを目視またはレンダリング検査する。
- 検証記録には実施日、対象 source revision、採用 inbox snapshot、引用・文体・権利・図表を含む実施項目、結果、残件を残す。Git 差分に中間物、秘密情報、権利不明素材、無関係な変更がないことを確認し、台帳またはQAに未解決の不備があれば `out/` へ promotion しない。

## 禁止事項

- 未定義の語・記号、未証明の非自明な結果、暗黙の公理を説明なしに使うこと。
- 必要な証明を「明らか」「同様」「標準的」「証明を省略」で置き換えること。
- 循環参照、使用前依存、定義の循環、同値命題同士の循環証明を放置すること。
- 根拠のない補足、推測、AI 生成証明を検証済み数学へ混ぜること。
- inbox の入力をエージェント都合で変更し、不安定・未評価の入力を採用すること。
- 権利不明素材、秘密情報、大規模転載をコミット・公開すること。
- 合意なくツールチェーン、エンジン、基礎づけを変更し、draft を無検証で out に置くこと。

## 完了条件

- 合意した論理・集合・基礎から修士課程程度の到達範囲まで、依存順に読める講義ノート PDF が完成している。
- 未定義語・記号がなく、非自明な結果は原則証明済みで、例外は承認済みスコープ外依存として出典、仮定、使用箇所が明示されている。
- 依存台帳に欠落、循環、前方論証依存がなく、公理、規約、既知事項、スコープ外事項が分類されている。
- `../MATH_PROSE_REVIEW.md` の独立 read-only review と再レビューが完了し、definition-before-use violation、未追跡依存、unresolved blocking gap、open blocking finding がすべて 0 件である。
- すべての formal assertion が種類に応じた番号付き semantic environment と一意で安定した label を持ち、すべての proof が明示的な proof environment に置かれている。直前の statement に続く通常の proof は原則「証明」の bare な無番号 proof で不要な proof 番号・label・対象参照がなく、離れた証明・解答と番号付き proof は前記規則どおり必要な対象参照・label を持つ。prose だけの formal statement・proof と再構成不能な proof dependency trace が 0 件である。
- 各章が定義、例・非例、命題・定理、証明、応用、要約、演習の教育的流れを持ち、演習の難度、依存、ヒント・解答方針が記録されている。
- 記号表、用語、索引、参考文献、相互参照が本文と一致し、初学者への局所説明と修士水準までの全体整合性がレビュー済みである。
- 合意した手順で再生成でき、警告、参照、目次、数式、フォント、全ページのレイアウト検証を通過し、参照成果物がある場合は house style を一組として actual-size と fit-width の両方で比較済みである。
- 採用 inbox snapshot、source revision、検証結果、承認が記録され、最終成果だけが `out/` に明示的に昇格されている。
- 各 affected work unit の independent review が記録され、draft の `reviewed` unit に gap・未定義・壊れた参照がなく、`WIP` は隔離・明示され、`out/` の `WIP` と unresolved blocking は 0 件である。
- 配布物に秘密情報や権利不明素材がなく、公開・配布条件が確認されている。
