# 論文理解・講義ノート PDF 制作ガイド

## 適用範囲

この `AGENTS.md` は `paper-lecture-notes/` とその配下だけに適用する。書籍翻訳や体系的数学教科書へ固有規則を持ち込まない。上位の指示と衝突する場合は上位を優先し、判断できなければ作業を止めて確認する。

本 project は親の `../AGENTS.md`、`../JAPANESE_WRITING.md`、`../MATH_PROSE_REVIEW.md` と、repository root で version 管理された `../.system/` tools を shared versioned 正本として直接使う。private copy を正本にせず、案件固有の adapter が必要なら差分と理由だけを private area に記録する。日本語の原稿・訳注・講義ノートは `../JAPANESE_WRITING.md` に従う。ただし、論文理解では原論文の主張と自分の解釈を明確に区別することを共通規範より優先する。

## 目的

論文を自分で説明・検証できる水準まで咀嚼し、主張、仮定、定義、手法、証明・導出、実験、限界、関連研究を教育的な順序に再構成した講義ノート PDF を作る。原論文、執筆者の解釈、外部知識、未解決の疑問を区別し、各説明を原論文または追加出典まで追跡可能にする。

想定成果物は、学習目標を備え、合意した前提知識と範囲に対して自己完結し、必要な推論・導出・計算を本文中で完結させた講義ノート PDF、再現可能な編集ソース、書誌・権利台帳、主張・式・図表の追跡表、解釈・疑問・再計算の記録である。原論文の単なる和訳や節順の写しではなく理解のための再構成物とするが、原論文の主張範囲を変えない。

## ワークスペースの契約

ユーザーから見える作業領域は次の4つに固定する。管理用の本 `AGENTS.md` だけはトップに置く。案件固有の必要が生じても、トップ階層を増やさず、まず `.workspace/` 内の既存区分へ配置する。

```text
paper-lecture-notes/
  AGENTS.md
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

repository mode は親の規則と `../.system/repository-mode` に従う。

- public mode では、`inbox/`、`draft/`、`out/`、`.workspace/` の4 private 領域とその全内容、project 名、manifest のファイル名・hash、PDF、原稿、講義ノート、台帳、案件固有の data・config・tool を stage、commit、push しない。この project 内で Git 対象にできるのは privacy review 済みの `AGENTS.md` だけである。
- verified private mode では、configured remote の同一性と GitHub visibility が `PRIVATE` と検証できる場合に限り、4 private 領域を含む processing document を通常の `git add` で stage、commit できる。互換用の `../.system/repository-mode add -- <paths>` も利用できるが、`git add -f` は使わない。
- private mode でも credential、secret、token、鍵、契約・ライセンス上保存できない data、権利上 commit できない原資料は commit しない。private remote の存在は取得、保存、利用、配布の権限を与えない。
- `out/` の配布承認と GitHub repository の visibility・commit 可否は別に判定する。配布承認済みでも GitHub に置けるとは限らず、private GitHub に置けても配布できるとは限らない。

`.gitignore`、`../.system/repository-mode`、hook を回避せず、`git add -f` や `--no-verify` を使わない。status、diff、作業報告にも、私的な原文、タイトル、著者、研究内容、ファイル一覧、hash を必要なく転記しない。この構造と ignore 規則は defense in depth にすぎず、secret store、暗号化、アクセス制御の境界ではない。

## 永続データの配置

- `.workspace/source/`: 講義ノート本文、成果用に作成した図表、再現可能な計算コード等の canonical source。
- `.workspace/records/bibliography.*`: Paper ID、著者、題名、掲載先、年、DOI/URL、版・改訂、参照日、ライセンス・利用条件、権利、追加出典。
- `.workspace/records/claim-trace.*`: 主張、仮定、適用範囲、根拠、限界と、原論文の節・ページの対応。
- `.workspace/records/equation-trace.*`: 式、定理、導出、変形、仮定、再計算と元番号の対応。
- `.workspace/records/figure-trace.*`: 図表、実験値、データ、元番号、扱い、権利、再描画・再現条件の対応。
- `.workspace/records/interpretation-questions.*`: 解釈、外部知識、未解決の疑問、反例、再現不能、次の確認方法。
- `.workspace/records/recalculations.*`: 次元、符号、境界、特殊例、数値、小規模再現等の独立検証。
- `.workspace/records/paper-ingestion-manifest.*`: `inbox/` の検出履歴と、採用した入力スナップショット。
- `.workspace/records/` のその他の用途別台帳: 学習スコープ、QA、公開判定等。台帳名と形式は案件開始時に決める。
- `.workspace/tools/`: 採用後の案件固有のビルド設定、adapter、依存 lockfile。再利用可能な prompt、template、script、build/QA tool、test、fix の正本はここに置かず public shared system へ upstream する。

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

## draft と out

- `draft/` の PDF は途中成果であり final と呼ばない。`out/` への移動・複製は、完了条件を確認した明示的な promotion とする。
- 公開済み draft を何らかの変更後に再公開する場合は、affected work unit の再 review を完了し、変更後 snapshot に対する gate と `draft/STATUS.*` の review 状態を更新する。
- 各制作・レビュー cycle の終了時に、その時点の完成 unit と WIP を含む通読可能な draft PDF を作る。WIP は本文中で開始・終了と未完了範囲を明示し、欠落した証明や未検証の主張を完成済みに見せない。
- 公開状態を `draft/STATUS.*` 等へ記録し、少なくともビルド日時、成功・失敗・陳腐化、source revision または commit、採用した inbox snapshot、work unit ごとの `reviewed` / `WIP` / `blocked`、検証状態、残件、対応する PDF 名を含める。`reviewed` は該当 unit の必須 review gate を満たした場合だけ付与する。
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

## 標準ワークフロー

1. `inbox/` を受け入れプロトコルどおり走査し、対象論文と版、目的、読者、前提知識、範囲、深さ、公開範囲、既存ツールチェーンを確認する。
2. 原論文に Paper ID を付け、書誌、版・改訂、ページ方式、権利を bibliography と各 trace に登録する。補助資料は別 ID とする。
3. 問題設定、主要主張、前提、定義、手法、証明・導出、実験、限界、関連研究、著者の未解決点を分解する。
4. ノートの項目を原論文の節、ページ、式・定理・図表番号、付録へ対応させる。
5. 途中式、次元、符号、極限、境界条件、数値例、特殊例、反例、実験条件を検討し、可能なら独立に再計算・小規模再現する。
6. 学習目標と前提知識を示し、動機、直観、定義、主張、導出、例、限界、節末要約の順に教育的に再構成する。
7. 原論文、解釈、外部知識、未解決の疑問を識別し、不確かな点を推測で埋めない。
8. 引用精度、主張の強さ、仮定、式・図表対応、理解検証、および途中の推論・導出・計算に欠落がないことをレビューする。
9. 数学を含む範囲では、work unit 完成時と semantic change 後の checkpoint で `../MATH_PROSE_REVIEW.md` の affected phase を authoring 担当とは分けた read-only review phase として実施し、finding と再検証結果を private records に保存する。structural change と editorial change は親 `AGENTS.md` の分類に従って検査範囲を絞る。
10. WIP 制作中は変更箇所を incremental build し、checkpoint で受け入れ snapshot と影響範囲を再確認する。読者へ共有する draft または `out/` 候補ではログと全ページ表示を検証して公開する。
11. 権利、ライセンス、配布範囲、不要物混入、直前の inbox 状態を確認し、承認後にだけ `out/` へ昇格する。

## work unit と review cycle

- 制作物を追跡可能な work unit に分ける。work unit は、少なくとも claim、definition、method、proof、derivation、experiment の各 subsection、または同程度に独立して authoring・review・status 判定できる範囲とする。各 unit に安定した一意の ID、対象 source revision、依存 unit、対応する trace 項目を与える。
- 細かな編集は checkpoint まで batch する。unit 完成時と semantic change 後の checkpoint で affected-unit review を行い、影響範囲、finding、対応、再検証、review status を private records に記録する。structural change は対象を絞った検査、editorial change は差分確認、incremental build、必要な変更ページの render を行い、意味・定義・参照・依存・文書構造へ影響しない unit の独立 review を再度開かなくてよい。
- 数学を含む semantic change と、意味・定義順序・依存・読者解釈へ波及する structural change の affected unit は、authoring 担当とは別の担当による独立 read-only review として `../MATH_PROSE_REVIEW.md` の unit gate を適用する。blocking gap、unlabeled formal claim、broken reference、definition-before-use violation、未追跡依存のいずれかがあれば `reviewed` にしない。
- cycle ごとに、採用した input snapshot、cycle 対象 unit、各 unit の `reviewed` / `WIP` / `blocked`、open finding、次の作業を draft 本文または `draft/STATUS.*` から確認できるようにする。draft 本文は WIP を隔離・表示しつつ通読可能に保ち、reviewed 部分と混同させない。
- unit review は成果物全体の review を代替しない。`out/` への promotion 前に、採用 input snapshot と含まれる全 unit を固定し、`../MATH_PROSE_REVIEW.md` の全 phase と本ガイドの全体 gate を再実行する。

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
- 原論文と外部資料の書誌、版、DOI/URL、ページ、参照日、ライセンス・利用条件、権利を bibliography に記録する。直接引用、要約、翻訳に出典を示し、孫引きなら明示する。
- 図表、付録データ、コード、非公開査読資料、個人情報、契約制限データには別の利用条件があり得る。判定を bibliography、figure trace またはQAへ記録し、秘密情報、権利不明素材と共にコミット・公開しない。

## 数式・図表・PDF 品質

- 数学を含む成果物は `../MATH_PROSE_REVIEW.md` の独立 read-only review と再レビューを完了する。definition-before-use violation、未追跡の仮定・依存、open blocking finding がいずれも 0 件になるまで `out/` へ promotion しない。scope 除外で対応する場合も、影響とユーザーの明示合意を private record に残して gate を再実行する。
- formal claim は、意味に応じた番号付き semantic environment（`theorem`、`lemma`、`proposition`、`corollary`、`definition` 等）に置き、自動生成番号と一意で安定した `label` を付ける。形式的主張を無番号の強調文や通常 prose だけで提示しない。
- 各 formal proof は明示的な proof environment に置く。直前の theorem-like statement を証明する通常の proof は、見出しを原則として「証明」とする bare な無番号 proof とし、proof 番号、proof 自体の `label`、対象 statement への明示的な cross-reference を付けない。statement から離れた証明には対象 statement を `label` で示す無番号の説明見出しを用いてよい。番号付き proof は、複数の証明・別証明を区別する場合、または proof 自体を他所から参照する場合に限り、一意で安定した `label` と対象 statement への cross-reference を持たせる。必要な statement、番号付き proof、definition、equation 間の参照は番号の直書きではなく `label` による cross-reference とし、prose だけの形式的証明を作らない。
- proof と derivation は、前提、依存結果、各変形の根拠、場合分け、境界条件から結論までを明示して完結させる。「明らか」「同様」「容易に分かる」等で非自明な段階を省略しない。derivation の主要な段階は番号付き・ラベル付きの式または適切な semantic environment に置き、本文から参照できるようにする。
- 原論文の claim・proof と、ノート執筆者による reconstruction・補足導出・correction を、見出し、環境名、注記等で読者が明確に識別できるようにする。再構成や訂正を原論文の主張として表示せず、その根拠と truth status を trace および review record に対応付ける。
- 記号は初出で定義し、型、次元、定義域、添字範囲、確率変数か実現値かを必要に応じて示す。原論文と記号を変える場合は対応表と理由を残す。
- 式変形の仮定、定理、近似、極限操作を明記し、近似と等号を混同しない。式番号、本文参照、演算子、書体を統一する。
- 図表ごとに出典、元番号、ページ、引用・再描画・改変・新規作成の別、権利を figure trace に記録する。値、軸、単位、凡例、集計条件、番号、キャプション、本文参照、白黒・縮小時の可読性、アクセシビリティ、画像解像度を確認する。
- ビルド成功だけで完成としない。エラー、警告、未解決参照、重複ラベル、欠落引用、参考文献、目次、索引、しおり、式・図表番号を確認し、索引の全ページ参照が PDF 内リンクとして機能して正しいページへ移動することを検証する。
- 日本語・欧文・数式・コードのフォント、埋め込み、文字化け、行送り、禁則、長い URL、脚注、余白、ページ番号、はみ出しを確認する。最終候補は全ページを目視またはレンダリング検査する。
- 検証記録には実施日、対象 source revision、採用 inbox snapshot、引用・文体・権利・図表を含む実施項目、結果、残件を残す。Git 差分に原資料、中間物、秘密情報、権利不明素材、無関係な変更がないことを確認し、台帳またはQAに未解決の不備があれば `out/` へ promotion しない。

## 禁止事項

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
- 全 work unit について作成・最終修正後の affected-unit review が記録され、成果物に含まれる unit がすべて `reviewed` で WIP が 0 件である。数学を含む場合は `../MATH_PROSE_REVIEW.md` の独立 read-only review が完了し、definition-before-use violation、unlabeled formal claim、broken reference、未追跡依存、open blocking finding が 0 件である。
- 合意した手順で再生成でき、警告、参照、目次、数式、フォント、全ページのレイアウト検証を通過し、参照成果物がある場合は house style を一組として actual-size と fit-width の両方で比較済みである。
- 採用 inbox snapshot、source revision、検証結果、承認が記録され、最終成果だけが `out/` に明示的に昇格されている。
- 配布物に秘密情報や権利不明素材がなく、公開・配布条件が確認されている。
