# 数学文章レビュー規範

## 目的、適用範囲、優先順位

本規範は、数学を含む書籍翻訳、論文、講義ノート、研究ノート、演習と解答を `out/` へ昇格する前の実行可能なレビュー手順である。
レビューの優先順位は次のとおりとする。

1. 数学的正確性と主張の真偽
2. 合意した範囲での self-contained 性
3. 原典・出典への忠実性
4. 文章の明瞭さと文体

数学的レビューを文体や認知リズムの調整より先に行う。
読みやすさのために仮定、依存、非自明な推論を落とさず、論理的な行間をリズムとして残さない。
日本語の文章表現は `JAPANESE_WRITING.md` に従い、数学的レビューの完了後に文体を調整する。

本規範は repository 共通の数学品質 gate であり、特定文書の house style ではない。
参照元に固有の句読点、macro、mm-space の用語・記法、要旨や序文の構成などは移植しない。
本規範の正確性と検証可能性を損なわない範囲では、project の house style と組版・投稿規程を優先する。

## レビュー体制と記録

- authoring 担当とは分けた read-only review phase を原則とする。修正は finding を受けた authoring 担当が行い、reviewer は修正後の対象を再検証する。
- review artifact は project の private な `.workspace/records/` に保存し、public Git へ commit または push しない。
- 各 finding には severity、位置、対象の命題または表現、問題の根拠、修正案、再検証結果を記録する。
- 自動検査は索引、参照、形式上の候補を見つける補助とし、内容の正しさの判定を代替させない。

severity は次のように分類する。

- `blocking`: 誤った主張、証明の成立を妨げる gap、未定義の形式要素、循環依存、原典の意味を変える翻訳、権利・出典上の重大問題など、promotion を禁止するもの。
- `major`: 読者が結論を追えない省略、重要な仮定・場合・依存の不明確さ、例や演習の実質的欠陥など、解消または明示合意が必要なもの。
- `minor`: 真偽や追跡可能性を損なわない局所的な表記、参照、文章上の不備。

## work unit とレビュー頻度

レビューの単位となる work unit は、subsection、定理とその証明の cluster、一続きの導出、exercise set、translation batch など、依存関係と変更影響を追跡できるまとまりとして、制作前または分割時に定義する。
各 unit の範囲、source revision、依存先、状態、最終 review revision を review artifact に記録する。

- 各 unit の完成時、およびその後のあらゆる revision/change ごとに、affected unit と影響範囲を review する。label/ref、notation、punctuation、formatting、translation wording の変更も例外にせず、レビューを原稿末尾の一回だけにまとめない。
- 各 work cycle で、authoring 担当とは分けた independent review を行う。小さな変更でも affected unit を再 review し、その直接依存と直接の利用箇所への波及を確認する。純粋に編集上の小変更では checklist を affected correctness、definition、label/ref、rendering の項目に限定してよいが、review 自体を省略してはならない。
- draft PDF を公開または読者へ共有する前に、その snapshot に含まれるすべての unit を review する。公開済み draft を何らかの変更後に再公開する場合は、affected unit の再 review を完了し、変更後 snapshot に対する gate と review status を更新する。
- `reviewed`、`unreviewed`、`WIP` を project の `STATUS` と成果物本文の双方で識別し、snapshot と表示が一致していることを確認する。open blocking finding がある unit を `reviewed` と表示してはならない。
- draft は、読者が完成範囲と未完成範囲を正しく判断できる可読品質にする。未完成の後半は明示した placeholder または roadmap として隔離し、未完成の証明を完成済みの本文のように掲載しない。
- `out/` への promotion 前には、unit ごとの既存 review に依存せず、成果物全体を一つの snapshot として全 phase で再 review する。

## phase 1: inventory と truth status

レビュー対象の source revision、想定読者、self-contained の範囲、利用可能な定義・結果・出典、対象となる本文・見出し・定理環境・表・図・caption・演習・付録を確定する。
対象外とした範囲も記録し、未確認部分を確認済みと混同しない。

すべての主要な主張について truth status を次から明示し、本文の modality と一致させる。

- proved: 本文または合意済み依存から証明済み
- conditional: 明示した仮定または外部結果の下で成立
- heuristic: 直観・近似・経験的説明であり証明ではない
- conjectural: 予想であり未証明
- open: 未解決問題

著者の主張、引用した結果、reviewer の訂正・解釈、編集上の補足を混同しない。

## phase 2: definition-before-use audit

記号、用語、略語ごとに first use と definition の索引を作る。
本文だけでなく、タイトル、見出し、定理・定義、表、図、caption、脚注、演習、解答、索引も検査対象とする。

- 形式的な記号、用語、略語は使用前に必ず定義する。first use が definition より前なら blocking とする。
- 定義には対象の型、domain、codomain、引数、添字範囲、有効範囲を必要なだけ含める。
- motivation で後の概念を自然語により preview してよいが、未定義の形式記号や未導入の推論には依存させない。
- 同じ記号の再定義、重なる scope での別用途、表記だけ異なる重複概念、略語の揺れを検査する。
- 定義環境は原則として一つの概念を定義する。複数概念、補助構成、性質、結論を一つに詰め込まない。

## phase 3: statement と dependency audit

各定義、補題、命題、定理、系、例、演習に対して、直接依存する定義・仮定・先行結果・外部結果を追跡する。

- formal な数学的 assertion を prose に埋め込まず、definition、theorem、lemma、proposition、corollary、example、exercise など意味に合う semantic environment に置く。motivation や直観の prose は許容するが、formal claim の代用にはしない。
- theorem-like statement には必ず自動生成の番号と一意で安定した `label` を与える。本文から theorem、definition、proof、equation を指すときは `label` による参照を使い、手書きの番号を使わない。
- domain と codomain、量化子の順序、添字範囲、変数の scope を確認する。
- 定数が何に依存するか、局所的な主張か一様な主張か、点ごとか概収束・ほとんど至る所かを明示する。
- 存在、一意性、well-definedness、境界値、退化ケース、場合分け、極限操作を確認する。
- 必要に応じて可測性、可積分性、連続性、コンパクト性、完備性、閉性など、使用する外部結果の前提を確認する。
- 各仮定が証明で使われているか、使われない仮定が不要か、暗黙に使った仮定が欠けていないかを調べる。
- 結論の各部分がどの仮定・結果に依存するかを確認する。
- 循環依存、未宣言の前方依存、存在しない参照、定義と定理の相互依存を禁止する。
- 定理 statement は仮定と結論に集中させ、長い構成、証明、解説は環境外へ置く。

依存台帳がある project では本文と台帳を双方向に照合する。
食い違いを推測で解消せず、証明と定義に基づいて双方を更新する。

## phase 4: proof gap audit

証明を含意ごとに分解し、次を一つずつ検証する。

- すべての proof を明示した proof environment に置く。標準の proof environment が無番号なら、選択した toolchain で番号付き proof environment を定義する。各 proof 自体に自動生成の番号と一意で安定した `label` を与え、冒頭で何を証明するかを対象 theorem-like statement の `label` 参照により示す。
- 各 proof について、goal、hypotheses と変数の domain、construction または必要な intermediate claim、各 implication の justification、尽くすべき cases、target conclusion を識別する。末尾では得られた結論が target conclusion であることを明示する。
- 各 implication の根拠が定義、先行結果、外部定理、または明示した直接計算に対応するか。
- 暗黙の補題を必要としていないか。必要なら statement と証明を追加するか、正確な外部依存として記録する。再利用する、または構造上独立した非自明な intermediate claim は、番号と `label` のある lemma として分離する。
- 代数変形、等式・不等式、符号、零除算、根号・対数等の domain、極限操作、写像の well-definedness を、読者が追跡できる step 列として示す。等号・不等号の向きと、極限と演算の交換の根拠も確認する。
- 全 cases が排反かつ尽くされているか、境界・空集合・零・低次元などの退化ケースが落ちていないか。
- 「明らか」「容易」「標準的」「同様」だけを根拠として step を省略しない。これらを使う場合も、直後に具体的な定義、置換、計算、先行結果、または推論を示す。「同様に」では実際の置換、前の議論との差分、追加条件を記す。
- 外部定理は名称、原典の定理番号と位置、正確な statement を示し、適用箇所ですべての条件が成立することを検証する。
- 想定読者に routine でも、合意した self-contained scope 上で非自明なら説明または承認済み外部依存が必要である。

proof の十分な長さに任意の語数・page 数の下限を設けない。
想定読者と合意した self-contained scope に対して、すべての非自明な step が明示され、reviewer が statement から conclusion までの proof dependency trace を再構成できることを十分性の基準とする。
再構成できなければ blocking とし、単なる冗長な反復で長さを増やすことは求めない。

unit ごとに statement、proof、definition、label、ref の件数を集計し、少なくとも proof を要する statement に対応する proof の欠落、proof の対象 statement 参照の欠落、必要な label の欠落、未定義・重複 label、broken ref、orphan proof がすべて 0 件であることを checklist に記録する。

計算機、CAS、proof assistant、検索結果は検証証拠になり得るが、入力、前提、適用範囲、再現方法を記録し、人間が読める論証との関係を示す。

## phase 5: examples、counterexamples、exercises

- 例が定義の全条件を満たし、主張した性質を実際に持つことを計算または証明する。
- 非例・反例が否定対象を正確に破り、より強い別主張だけを破っていないことを確認する。
- 例の特殊性を一般的証拠として扱わず、heuristic と proof を区別する。
- 演習は導入済みの定義・結果だけで解答可能か、問題文の条件が十分か、解が存在するか、曖昧な複数解釈がないかを実際に解いて確認する。
- hint、解答、難度、依存先、本文の使用箇所が互いに整合するか確認する。

## phase 6: notation と LaTeX audit

- 定義、定理、命題、証明、例、演習には意味に合う semantic environment を使い、見た目だけの手動見出しで代用しない。
- 括弧の対応と scope、上付き・下付き、添字範囲、演算子、delimiter、式番号、数式内 punctuation を確認する。
- `label` の一意性、`ref` の存在と参照対象、定理・proof・式・定義・図表番号、文中の参照名を build 結果と照合する。未定義・重複 label、broken ref、orphan proof を 0 件にする。
- project の house macro は定義、引数、意味、scope を確認し、未定義 macro、意味の衝突、表示だけで意味を担う用法を残さない。
- TeX の compile 成功だけで数式の意味が正しいとは判定しない。PDF 上でも theorem、proof、equation、definition の番号と参照、欠落 glyph、改行、式の切断、図表との対応を目視確認する。

## 文書種別ごとの追加検査

### 書籍翻訳

- 数式、量化、否定、条件、modality、定義域、式番号を原文と逐語的に照合する。
- source error の疑いを黙って修正しない。原文、問題、採用判断、根拠を private review record に残し、必要に応じて訳注・正誤注として原著者の主張と区別する。
- 記号統一や節の再構成が原文の依存順と意味を変えていないか確認する。

### 論文

- author の claim、引用した既知結果、reviewer の correction、reviewer の interpretation を明確に区別する。
- 新規性、適用範囲、比較対象を証明以上に強く書かず、未検証の優先権・網羅性を断定しない。
- reviewer の修正案を著者の合意なしに原 claim として扱わない。

### 数学ノート

- definition、result、proof、exercise、external dependency の ledger を維持し、使用箇所まで追跡する。
- self-contained scope 内に未解決の blocking gap を残さない。未解決事項は truth status と依存不能範囲を明示し、完成部分に逆流させない。

## phase 7: fix と再検証

finding を severity 順に修正し、修正が定義、依存、後続証明、例、演習、参照へ与える影響を検索する。
局所的な文言修正だけで済ませず、変更された主張の利用箇所を再検証する。
修正後は finding ごとに結果と証拠を記録し、未対応を黙って閉じない。

draft に完成 unit を含める場合、各 unit は次の unit gate を満たさなければならない。

- [ ] definition-before-use violation が 0 件である。
- [ ] unjustified step が 0 件である。
- [ ] unlabeled formal claim が 0 件である。
- [ ] numbering error と reference error が 0 件である。

WIP は draft に限り、状態、未完了箇所、依存不能範囲を `STATUS` と本文で明示して完成 unit から隔離できる。
WIP を `reviewed` の本文に混ぜず、`out/` では WIP を 0 件にする。

## phase 8: independent re-review と promotion gate

修正担当と分けた reviewer が、少なくとも変更箇所、直接依存、直接の利用箇所、全 blocking finding を read-only で再確認する。
次をすべて満たした場合だけ `out/` への promotion を許可する。

- [ ] blocking finding が 0 件である。
- [ ] major finding が解消済み、または影響を理解した明示合意と理由が記録されている。
- [ ] definition-before-use violation が 0 件である。
- [ ] unjustified step と unlabeled formal claim が 0 件である。
- [ ] statement、proof、definition、label、ref の件数を記録し、対応・label・参照の欠落、未定義・重複 label、broken ref、orphan proof が 0 件である。
- [ ] WIP、unreviewed unit、未完成の proof が 0 件である。
- [ ] 未追跡の仮定と依存が 0 件である。
- [ ] truth status、source fidelity、文書種別ごとの追加検査が完了している。
- [ ] build、参照、数式表示の検査が完了している。
- [ ] 成果物全体について全 phase の再 review と independent re-review が完了している。
- [ ] review artifact の保存先が private local area であり、public 差分に混入していない。

gate を満たさない成果物は `draft/` に留める。
blocking finding は単なる waiver では閉じない。
対象部分を成果物と self-contained scope から除外する場合は、影響、依存不能範囲、責任者、ユーザーの明示合意を private record に残し、残った成果物について gate を再実行する。
