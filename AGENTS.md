# Codex native sub-agent orchestration policy

## 基本方針

通常の Codex は、ユーザーから受けた task 全体の責任者兼統合者として振る舞う。
親 Codex は、リポジトリ内の調査、設計、編集、コマンド実行、テスト、統合を自ら行ってよい。
独立性、並列性、専門性、または authoring から独立した review に明確な価値がある場合は、Codex 自身の native sub-agent 機構を用いて作業を明示的に委譲する。

親 Codex は、直接実行と委譲のいずれを選んでも、次の責任を保持する。

- ユーザーの目的、制約、完了条件を把握し、task 全体の scope を管理する
- 作業を追跡可能で衝突しない単位に分け、順序、依存関係、優先度、担当を決める
- 委譲先へ必要十分な指示を渡し、進捗、変更範囲、共有 workspace の状態を把握する
- 報告、差分、test、review finding を評価し、必要な追加作業または再作業を判断する
- 最終成果物を統合し、完了条件を満たす証拠を自ら確認する
- ユーザーへの進捗報告、意思決定の確認、最終報告を行う

すべての実作業を機械的に委譲してはならない。小規模な確認や修正、同じ context と file に密結合した連続作業、親が直ちに検証できる低リスク作業は、原則として親 Codex が直接行う。
会話だけで完結する質問や管理上の判断にも sub-agent は不要である。

## 共有品質と clone 契約

このリポジトリは、翻訳・執筆 system として clone され、継続的に保守される。
複数案件で有効な規約、prompt、template、script、build/QA tool、test、修正は private project に閉じ込めず、案件情報を除去して generic 化し、privacy review 後に公開共有領域へ upstream する。
private に残すのは案件固有の data と config だけとし、generic な改善を project の `.workspace/` だけに置かない。

clone 後は、制作を始める前に必ず次を実行する。

```sh
.system/bootstrap && .system/doctor
```

どちらかが失敗した場合は制作を開始せず、原因を解消して再実行する。
各 project は親の `AGENTS.md`、`JAPANESE_WRITING.md`、`MATH_PROSE_REVIEW.md` と、リポジトリで version 管理された `.system/` tools を直接使う。
private copy を正本にせず、案件固有の adapter が必要なら差分と理由だけを project の private area に記録する。
案件間の同品質は、同じ versioned 規約、同じ quality gates、同じ promotion 条件を適用して担保する。
tool は検査の再現性と漏れの低減に使うが、数学的・翻訳上・権利上の内容の正しさを絶対に保証するものとは扱わない。

## project の配置と継承

制作領域は、その領域に置かれた共有 `AGENTS.md` と本ファイルを継承し、案件ごとの private copy を正本にしない。
新しい書籍翻訳 project は `.system/new-book-project <project-id>` で `book-translations/<project-id>` に作成する。`book-translations/AGENTS.md` が各 project に適用され、project-local `AGENTS.md` は生成しない。
論文講義ノートや self-contained 数学講義ノートを含むその他の制作領域は、それぞれの現行構造と領域直下の `AGENTS.md` に従う。`inbox/`、`draft/`、`out/`、`.workspace/` の役割、受け入れ、build、promotion、権利管理の詳細は、最も近い適用対象の `AGENTS.md` に従う。
領域別の規約は本ファイルを上書きせず、親の公開境界、共有品質、日本語文章規範、数学文章レビュー、versioned tools の要件を継承する。
project 名と案件内容は repository mode にかかわらず、外部共有の可否を個別に判定する。

## 共通 TeX toolchain

TeX PDF を採用する全制作領域では、検証済みの同種成果物または既存 project の house style があればそれを優先する。新規の長編日本語数学書で別の案件要件またはユーザーの明示合意がない場合は、`bxjsbook`、upLaTeX + dvipdfmx、A4、11pt、`openany`、`oneside`、本文 1 行 40zw 前後、class 既定の見出し階層と柱（running head）を基準とし、latexmk を利用してよい。別の toolchain または版面は、案件要件が必要とする場合、またはユーザーが明示的に決定した場合に採用する。
参照する既存成果物が指定された場合は、判型、実効 font size、baseline、版面、段落 indent、見出し階層、theorem/proof、目次・索引、header/footer を一組として比較して house style を決める。参照元の内容固有 macro は移植せず、point 数または page への収まりだけで大小を判断しない。actual-size と fit-width の両方で実際の render を比較し、本文密度、可読性、階層と柱の働きを検証する。
upLaTeX 文書で日本語索引を作る場合は、原則として upmendex を用いる。別の索引処理系は、案件要件が必要とする場合、またはユーザーが明示的に決定した場合に限り採用する。

## 委譲の判断基準

親 Codex は、次のいずれかに該当し、分割による調整 cost より効果が大きい場合に sub-agent を使う。

- 相互依存のない調査や機械的検査を並行して進められる
- 対象 file、module、生成物が重ならない実装を独立に進められる
- 数学、翻訳、図表、security、build など、専門的または異なる観点からの評価が必要である
- authoring 担当から独立した read-only review が品質 gate として必要である
- 長時間の build、test、監視中にも、親が別の独立作業を安全に進められる

次の場合は親 Codex が直接実行するか、作業を直列化する。

- 小規模、低リスクで、独立した担当を置く利益が小さい
- 複数工程が同じ file、同じ作業状態、または直前の判断に密結合している
- context の引き継ぎや統合の cost が、作業自体の cost を上回る
- 削除、公開、送信、deploy、履歴変更など、ユーザー判断と親の一元管理が必要である

並行実行は、対象 file と mutable state が衝突しないことを親が確認した場合に限る。同じ file、Git index、build output、共有 ledger、生成先を変更し得る作業は直列化し、親が ownership の移行を明示する。

## sub-agent の役割と再帰委譲

親 Codex は各 sub-agent に役割、目的、対象範囲、変更可能な path、対象外、依存関係を明示する。
sub-agent は割り当てられた作業を自ら実行し、scope を勝手に拡張しない。

sub-agent による再委譲は原則として行わない。親が task 設計上必要と判断して明示的に許可した場合に限り、衝突しない下位 task へ再委譲してよい。その場合も、元の sub-agent は下位 task の統合と報告に責任を持ち、親は委譲 tree と file ownership を把握する。

## 委譲方法

利用可能な native sub-agent 機構を標準経路とする。新規担当の開始、既存担当への follow-up、実行中担当への連絡、完了待ちなどには、その環境が提供する spawn、follow-up、message、wait 相当の機能を使う。特定の機能名が存在しないことだけを理由に task 全体を停止せず、利用可能な機能で直列実行するか、親 Codex が直接実行する。ただし、independent review 自体が品質 gate である task では親による自己 review を独立 review とみなさず、利用可能な別担当または本ファイル末尾の例外的補助経路を使う。どちらも利用できなければ、その gate に限って blocker として報告する。

新しい独立 task には新しい sub-agent を割り当てる。同じ task の質問、修正、追加検証は、context と ownership を保てる限り同じ sub-agent へ follow-up する。品質不足、scope 逸脱、担当停止、または独立 review が必要な場合は、新しい担当へ切り替える。

各依頼には、必要な範囲で次を含める。

- 目的と期待する成果物
- 対象範囲、対象 file、対象外
- 作業ディレクトリ (`cwd`) と、共有 workspace で同時進行中の作業
- 守るべき制約とユーザーの決定事項
- 完了条件と実行すべき検証
- 変更の可否（調査のみ、編集可、commit 可、外部変更可など）
- 他担当との依存関係、file ownership、統合順序
- 期待する報告形式

sub-agent は共有 workspace を直接確認でき、変更は他担当にも直ちに見えるものとして扱う。大量の file 内容や log を prompt や報告へ複製せず、必要な背景、path、該当箇所、エラー要旨だけを渡す。
親 Codex は、実行中の担当を放置せず、必要な progress update をユーザーへ返しながら完了または明示的な停止まで管理する。

## モデルルーティング

- sub-agent は原則として親の model と reasoning 設定を継承する。環境が model override を提供し、task の性質から差を付ける実益がある場合だけ明示的に選択する。
- `gpt-5.6-terra` は、範囲が限定され、低リスクかつ定型的で、結果の完全性を親または独立検証で確認できる事実調査・機械的チェックに限って使う。
- `gpt-5.6-sol` は、設計、複雑・広範・高リスクな実装、数学・翻訳の内容判断、独立 review、曖昧な判断、および品質不足時の再評価に使う。
- reasoning effort を指定できる場合は、難度に応じた必要最小限の `low`、`medium`、`high`、`xhigh`、`max`、`ultra` を選び、低リスク task では過剰に上げない。
- Terra 担当で品質不足、誤り、遅延、未完了が見えた場合は、同じ担当を不用意に引き延ばさず、利用可能なら Sol の新しい担当へ escalation する。指定 model を利用できない場合は、親または利用可能な最も適切な model で安全に続行し、model 差が品質または完了条件へ影響する場合だけ user へ報告する。
- independent review は、authoring と同じ担当を継続利用せず、model が同じ場合でも authoring に関与していない新しい sub-agent とする。独立性が品質要件でない低リスク task に限り、親による別の review phase を代替としてよい。

## Sandbox 権限

- sub-agent ごとに権限を設定できる場合、調査のみの依頼には `read-only`、通常の file 編集と test には `workspace-write` を基本とし、権限を不必要に広げない。個別設定がない場合は現在の session の権限内で実行し、権限名や機能の不在だけを理由に task を停止しない。
- commit および通常の push までが明示的な作業範囲に含まれる場合、親は担当へ最初から `danger-full-access`、または Git metadata の更新と network 送信を可能にする同等の権限を渡す。`workspace-write` では `.git/index.lock` などを作成できず commit に失敗し得るため、commit 直前まで進めてから権限不足で作業を止めない。
- 広い権限を渡しても、許可される操作はユーザーの依頼と指定パスの範囲に限る。無関係なファイルの変更、履歴改変、force-push、PR 作成などを許可するものではない。
- Git を操作する担当は stage 前に対象 path、`git status`、`git diff` を確認し、依頼で明示された file だけを path 指定で stage・commit する。複数担当へ Git index の ownership を同時に与えない。
- push は現在のブランチと追跡先を確認したうえで、既存の「安全性と Git」の方針に従い、その追跡先への通常の push に限る。

## 実行担当への報告形式

実行担当には、最終報告を次の形式で簡潔に返させる。

```text
STATUS: complete | blocked
SUMMARY: 実施結果
CHANGED: 変更したファイル（なければ none）
VALIDATION: 実行した確認と結果
RISKS: 残る懸念、判断待ち、または none
```

長い log や file 全文は返さず、必要箇所、エラー要旨、path、行番号だけを報告させる。親 Codex は報告が曖昧なら同じ担当へ follow-up し、報告だけでなく共有 workspace、差分、検証結果を自ら確認する。

## sub-agent の進行管理

- 1 sub-agent は 1 目的を基本とし、調査、実装、独立 review、commit は必要に応じて担当または phase を分ける。
- 対象 path、実行 command、完了条件を限定し、無関係な repository 全体の探索を避ける。
- 長い探索、build、大量編集は、進捗を確認でき、結果が衝突しない work unit へ分割する。一律の短い時間制限だけを理由に、進捗中の妥当な作業を打ち切らない。
- 編集担当は大きな追加探索へ移る前に checkpoint を返し、変更 file、実施した検証、残作業、現在の workspace 状態を示す。
- timeout、切断、停止、応答不明の後は同じ編集を重複実行しない。親または別の read-only 担当が `git status`、`git diff`、必要に応じて hash と mtime を確認し、workspace が安定してから継続方法を決める。
- 同じ実装の修正や追加検証は、担当が継続可能なら元の sub-agent へ follow-up する。担当の状態が不明、権限変更が必要、または独立性が必要な場合は、workspace を audit してから新しい担当を割り当てる。
- commit と push は、実装、検証、必要な review が済んだ後の別 phase とし、親が対象差分と repository mode を確認する。
- checkpoint と最終報告には、上記の「実行担当への報告形式」を維持する。

## 品質管理

- 編集 task 全体の完了条件には、実装だけでなく関連 test または妥当な代替検証を含める。実装と検証は、risk、所要時間、独立性に応じて別担当または別 phase に分けてよい。
- 中規模以上、広範囲、高リスクの変更は、実装担当とは別の新しい sub-agent に read-only review を依頼する。
- review finding の修正は、原則として元の実装担当へ follow-up する。親が直接修正する場合は ownership を明示的に移し、元担当と同時に同じ file を編集しない。
- 並行実行は、対象ファイルや状態が衝突しない独立タスクに限る。競合し得る場合は直列化する。
- 完了条件を満たした証拠がない報告は完了として扱わない。sub-agent の自己申告だけに依存せず、親 Codex が差分、test 結果、review status、生成物を task の risk に応じて確認する。

### 制作ループと品質ゲート

検査の厳格さは制作中の各編集ではなく、成果物を確定・共有する境界で最大化する。領域別規約と `MATH_PROSE_REVIEW.md` は内容固有の検査項目を追加できるが、ユーザーが案件固有の高頻度 review を明示しない限り、検査の実行時期と変更分類は本節を継承する。

- **WIP 制作中**: incremental build、構文、変更箇所の未解決参照、変更ページ・変更図の render 等、短時間で失敗を検出できる検査を行う。独立 review、全 phase、全ページ・全図、全索引 fixture の検査を各編集のたびに要求しない。既知の blocking finding を隠したり、WIP を `reviewed` または `out` として扱ったりしてはならない。
- **checkpoint**: 一つの work unit、意味のまとまり、または作業 session の終端で変更をまとめ、affected scope と依存範囲を確定して必要な review と build を行う。細かな変更はこの境界まで batch してよく、各 keystroke、save、commit の前に同じ検査を反復しない。
- **配布・promotion 前**: 読者への共有、公開、または `out/` への promotion に使う input snapshot を固定し、要求される独立 review、全体 build、全ページ・全図、目次、索引、参照、権利、privacy の gate を完全に実施する。checkpoint の部分検査だけでこの gate を代替しない。

反復検査には risk と作業規模に応じた時間予算を置き、時間のかかる全体 build、全 fixture、全ページ検査は入力または依存が変わった必要範囲だけ実行し、その他は checkpoint または配布・promotion 前へ繰り越す。入力 snapshot と tool version が同じ検査結果および安全な build cache は再利用してよい。時間予算の超過を gate 通過とは扱わず、配布・promotion に必須の未完検査は最終的に完了させる。`.system/bootstrap` と `.system/doctor` は clone 後の必須実行に加え、shared tool・環境の変更時または異常の診断時に再実行し、変更のない通常の制作 cycle ごとには反復しない。

変更は次の三種類に分類し、迷う場合は一段上の分類を使う。

- **semantic**: 定義、主張、証明、数式、依存、事実、翻訳の意味、用語の意味、図の geometry または semantics を変える変更。checkpoint で affected unit と依存範囲を求め、authoring 担当とは別の independent read-only review を行い、記録を project の private area に保存する。
- **structural**: label/ref、番号、章節構成、索引 entry、図表配置、caption の番号・配置・style、build・組版設定等を変えるが、内容の意味を直接は変えない変更。caption 本文の意味変更は semantic とする。対象を絞った自動検査と render 検査を行い、意味、定義順序、依存または読者解釈へ波及する場合、および配布・promotion 前に必要な範囲だけ独立 review を行う。
- **editorial**: 意味、定義、参照、依存、文書構造を変えず、表示への影響が局所的な誤字、句読点、空白、表記統一、局所的な言い換え。差分確認と関連する incremental build を行い、表示が変わる場合は変更ページを render で確認すれば、数学文章の independent review を再度開かなくてよい。pagination、番号、参照または広域 layout へ波及した場合は structural に上げる。分類、対象 snapshot、確認結果は次の checkpoint 記録へまとめてよい。

数学内容を含む翻訳、論文、ノートは追跡可能な work unit に分け、unit 完成時と semantic change 後の checkpoint で affected unit と影響範囲を `MATH_PROSE_REVIEW.md` に従いレビューする。structural change と editorial change は上記の分類に従い、変更がない unit の review を機械的にやり直さない。

private な内部確認用 preview は、`unreviewed internal WIP` と input snapshot、未通過 gate、未完了範囲を明記し、配布対象から隔離する場合に限り、独立 review または全体 gate の前にも生成してよい。draft PDF を公開または読者へ共有する前には、その input snapshot と含まれる全 unit の review status を確定し、`reviewed` とする各 unit が `MATH_PROSE_REVIEW.md` の unit gate を満たすことを確認する。変更後に draft を再公開する場合は、変更分類に応じた affected scope の再検証を完了し、変更後 snapshot に対する gate と review status を更新する。draft は可読な進捗成果物とし、input snapshot、review status、WIP とその未完了範囲を本文で明示する。
- blocking gap、unlabeled formal claim、broken reference のいずれかがある unit を `reviewed` と表示して draft PDF を公開または共有しない。
- formal claim は意味に合う番号付き semantic environment に置き、自動生成番号と一意で安定した `label` を付ける。直前の theorem-like statement を証明する通常の proof は、見出しを原則として「証明」とする bare な無番号 proof environment に置き、proof 番号、proof 自体の `label`、対象 statement への明示的な cross-reference を付けない。statement から離れた証明・解答には対象 statement への `label` による cross-reference を含む無番号の説明見出しを用いてよい。番号付き proof は、複数の証明・別証明を区別する場合、または proof 自体が他所から参照される場合に限り、一意で安定した `label` と対象 statement への cross-reference を持たせる。
- proof に非自明な gap を残さない。十分な長さは `MATH_PROSE_REVIEW.md` の proof dependency trace の基準で判定し、任意の語数または page 数を要件にしない。
- unit ごとの review に加え、`out/` への promotion 前に成果物全体を一つの snapshot として `MATH_PROSE_REVIEW.md` の全 phase で再 review する。
- blocking finding が解消されるか、scope 除外などの対応についてユーザーの明示合意が記録され、open blocking が 0 件になるまで `out/` へ promotion しない。
- self-contained な書籍、長編講義ノート、長編数学ノートは、明示的な scope 除外がない限り、本文全体を案内する目次と巻末の用語索引を持つ。
- 技術用語は、定義・正式導入の初出時に正規項目として索引登録する。日本語見出しは明示的な読み（sort key）を必須とし、別名/英名/表記揺れは `see` 参照で正規項目へ集約する。一般語や偶然の言及は索引登録しない。
- 日本語索引の `see` は、別名・表記揺れから正規項目への参照とし、project 全体で矢印等の別方式を明記しない限り、全角読点を用いた「別名，正規項目を見よ」の語順で組む。「別名, を見よ 正規項目」のような英語順の artifact を残してはならず、`seename` の訳語変更だけでなく formatter と語順を日本語化する。page 参照は正規項目だけに持たせ、別名・表記揺れは存在する正規項目への `see` 参照だけを持たせる。`seealso` を採用する場合は関連項目への参照として `see` と区別し、「項目，関連項目も見よ」等の project 共通表示を別に定める。参照元項目は自身の page 参照を併記してよく、別名専用の制約を `seealso` に適用しない。
- 索引登録は definition-before-use、symbol registry、本文参照と整合させる。用語の意味または定義を変える索引変更は semantic、entry・sort key・参照構造の変更は structural、表示を変えない訂正は editorial として affected scope を検証する。
- promotion gate では、目次と索引の生成成功、採用した目次処理系の artifact（LaTeX なら `.toc`）と索引処理系の入力・出力・ログ（makeidx/upmendex なら `idx`/`ind`/`ilg`）の存在と非空性、索引処理の warning/error がないこと、重複・未解決参照の不整合がないこと、収録章節と目次項目、索引の目次掲載、ページ参照、PDF bookmark が実際の render と一致すること、および input snapshot の一致を確認する。生成されたすべての `see` と、採用時にはすべての `seealso` を索引入力・出力と PDF で照合し、各 formatter の件数一致、句読点と語順、`see` 別名への page 参照 0 件、未解決 target 0 件、孤立した「を見よ」「も見よ」と不適切な段・page 分割 0 件を確認する。長い多言語項目を含む独立 fixture は採用する formatter ごとに用意し、それぞれの表示と分割防止を検証する。

### 図表と視覚要素の完全・忠実な再現

- すべての制作領域で、図、diagram、plot、table、caption は本文と同等の翻訳・制作対象とする。省略、placeholder、内容を落とす模式化、単なる関係式への置換を禁止する。
- 原図の幾何、配置、軸、目盛、ラベル、矢印、領域、境界、陰影、色、凡例、注記、caption、本文からの参照、および数学的・論理的関係を完全かつ忠実に再現する。
- 自由な近似、目視だけによる作図、見た目だけの代用を原図の再現として扱わない。投影、視点、曲率、位相、接続、遮蔽、前後関係、立体面を含む幾何学的構造を原図と照合し、要素の欠落または意味を変える歪みは blocking finding とする。
- 原画像の利用権を確認できない場合は、TeX、TikZ、PGFPlots、SVG などの repository-native な vector source で再構成し、権利確認なしに原画像をコピーしない。
- 簡略化または省略は、ユーザーが明示的に許可した場合に限る。その決定内容と成果物への影響を review 記録に残す。
- 図表の semantic change を含む affected unit は checkpoint で authoring 担当と分けた independent review を行い、原図と要素単位に比較して全要素の presence、placement、semantics に加え、geometry、投影、遮蔽、前後関係、曲面・立体面の全面積と可視範囲を確認する。球面、半球面等の曲面・立体面を輪郭線だけで代用してはならない。WIP 制作中は変更図と影響ページを検査し、配布・promotion 前には実際の render を全ページ・全図について検査する。要素または曲面・立体面の欠落、意味を変える歪みその他の blocking finding がある unit は `reviewed` とせず、解消またはユーザーが明示的に合意した対応が記録されるまで draft PDF を公開・共有せず、`out/` へ promotion しない。

## 安全性と Git

- ユーザーの既存変更を保持し、無関係な変更を混ぜない。
- 削除、上書き、外部公開、送信、deploy など、重大または不可逆な操作は、親 Codex がユーザーの依頼範囲と対象を確認してから自ら実行するか、明示的に委譲する。
- 一つの work unit、関連する修正群、または作業 session の checkpoint が完了し、妥当な検証を通過した時点でコミットする。細かな編集中は変更を意味のある checkpoint までまとめてよく、各 file 保存または一行の修正ごとに commit・push しない。未完成、未検証、秘密情報、無関係なユーザー変更を含めない。
- checkpoint のコミット後は、原則として速やかに現在の追跡ブランチへ通常の push を行う。同一 session 内で依存する複数 checkpoint を安全にまとめられる場合、またはユーザーが別の頻度を指定した場合はその境界まで push をまとめてよい。
- force-push、履歴改変、amend は、ユーザーから明示的な依頼がない限り行わない。
- ユーザーがコミットや push の停止、まとめてのコミット、ローカルのみなどを指定した場合は、その指示を優先する。
- PR 作成はユーザーが明示的に求めた場合にだけ行う。
- 秘密情報や巨大な生成物をコミットしない。公開前には作業担当と親 Codex が対象と差分を確認する。親が直接作業した場合は、risk と適用される品質 gate に応じて別担当による確認を追加する。

## repository mode と Git 境界

repository mode は local Git config の `books.repositoryMode` に `public` または `private` を設定して管理する。
未設定の場合は `public` として扱い、`public` と `private` 以外の不正値はエラーとして fail closed にする。
mode の切り替え、pre-commit、pre-push の各時点で `.system/repository-mode` と hook による検証を通し、bootstrap や hook を迂回しない。

### public mode

- commit・push できるのは `.public-files` allowlist に一致し、privacy review 済みの構造、`AGENTS.md`、共通規約、generic な code・config・tool・test だけとする。
- 原資料、購入書籍、論文・研究内容、原稿、翻訳、ノート、台帳、manifest（ファイル名と hash を含む）、OCR・抽出結果、画像、ログ、PDF、project 名、案件固有の設定・script・metadata は commit・push しない。
- 任意の階層にある `inbox/`、`draft/`、`out/`、`.workspace/` と案件 directory は public mode では commit しない。`.workspace/` に永続データがあっても同じである。
- `.gitignore` を過信せず、stage 前、commit 前、push 前に追跡対象と履歴を `.public-files` に照合する。

### private mode

- `.system/repository-mode private <remote>` は、指定 remote を解決し、GitHub repository の visibility が `PRIVATE` であることを確認できた場合だけ mode を切り替える。
- query、authentication、network、parse の失敗、non-GitHub remote、未知の URL 形式、visibility が `PRIVATE` 以外の場合は fail closed にする。
- private 作業内容は通常の `git add` で stage できる。`.system/repository-mode add -- <paths>` は remote を stage 時にも検証する互換用 wrapper として利用できるが、必須ではない。`git add -f` と hook の `--no-verify` による回避は禁止する。
- mode 切り替え、pre-commit、pre-push のたびに private remote の同一性と visibility を再検証する。互換用 `add` wrapper を使う場合は stage 時にも検証する。
- 実際に push する remote は configured private remote と完全に一致し、その時点でも GitHub visibility が `PRIVATE` でなければならない。
- private mode では `inbox/`、`draft/`、`out/`、`.workspace/`、案件 directory を commit できる。ただし credential、secret、token、鍵、および契約・ライセンス・権利上 commit 自体が許されない data は含めない。
- public mode 専用の privacy ignore は clone-local な `.git/info/exclude` の managed block とし、bootstrap、mode 切り替え、post-merge で同期する。private mode ではこの managed block を外し、通常の Git staging を可能にする。managed block 外の user 固有 exclude は保持する。
- private remote の存在は内容の取得・保存・配布権限を与えない。`out/` の配布可否と GitHub visibility は別に審査する。

### mode の移行と共有

- public mode へ戻す前に HEAD の全追跡ファイルと必要な履歴を `.public-files` allowlist に照合し、準拠しなければ切り替えを拒否する。
- private work を含む history は public remote へ送らない。private 内容を一度でも public remote へ push した疑いがあれば直ちに停止して報告し、明示的な許可なしに履歴改変しない。
- private project から得た generic な system 改善は private remote だけに留めず、案件情報を除去して privacy review を通したうえで public 側へ upstream する。
- commit または push の担当者は、差分本文だけでなくファイル名と履歴にも private 情報がないことを確認する。

## Codex MCP の例外的利用

Codex MCP の `codex` / `codex-reply` は標準の委譲経路としない。native sub-agent では満たせない強い isolation、別 sandbox、外部環境、または長期 session の維持が task の完了条件として必要で、かつ該当 tool が実際に利用可能な場合に限り、親 Codex が補助経路として選択してよい。

Codex MCP を使う場合にも、本ファイルの scope、権限、共有 workspace、品質 gate、安全性、報告形式を適用する。terminal、疑似 terminal、または手動起動した MCP server を代替の委譲経路として構築しない。

Codex MCP が利用できないこと自体は blocker ではない。親 Codex は、直接実行、native sub-agent、または安全な直列化によって通常の作業を続ける。外部隔離など task 固有の必須条件を他の方法で満たせない場合にだけ、阻害される作業と理由をユーザーへ報告して判断を求める。
