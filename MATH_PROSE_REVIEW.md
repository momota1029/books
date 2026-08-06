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
- raw inventory、coverage matrix、mapping edge の意味保存を独立 review として認定する reviewer は、authoring 担当だけでなく、対象 inventory/mapping の作成・編集、semantic verification flag の付与、source-analysis 上の採否・同一視・統合判断を行った担当とも分ける。artifact 作成者による自己検査は WIP の一次確認には使えるが、independent review flag または promotion gate の証拠にしない。
- review artifact は project の private な `.workspace/records/` に保存し、public Git へ commit または push しない。
- 各 finding には severity、位置、対象の命題または表現、問題の根拠、修正案、再検証結果を記録する。
- 自動検査は索引、参照、形式上の候補を見つける補助とし、内容の正しさの判定を代替させない。

severity は次のように分類する。

- `blocking`: 誤った主張、証明の成立を妨げる gap、未定義の数学的・技術的要素、正当化されない循環依存、原典の意味を変える翻訳、権利・出典上の重大問題など、promotion を禁止するもの。
- `major`: 読者が結論を追えない省略、重要な仮定・場合・依存の不明確さ、例や演習の実質的欠陥など、解消または明示合意が必要なもの。
- `minor`: 真偽や追跡可能性を損なわない局所的な表記、参照、文章上の不備。

## work unit とレビュー頻度

レビューの単位となる work unit は、subsection、定理とその証明の cluster、一続きの導出、exercise set、translation batch など、依存関係と変更影響を追跡できるまとまりとして、制作前または分割時に定義する。
各 unit の範囲、source revision、依存先、状態、最終 review revision を review artifact に記録する。

- WIP 制作中、checkpoint、配布・promotion 前の gate と、semantic、structural、editorial の変更分類は親 `AGENTS.md` に従う。細かな編集は checkpoint まで batch し、各 save または revision ごとに同じ review を反復しない。
- 各 unit の完成時と semantic change 後の checkpoint で、affected unit、その直接依存、直接の利用箇所への波及を review する。authoring 担当とは分けた independent review は semantic change に必須とし、structural change では意味・定義順序・依存・読者解釈へ波及する範囲に限定して行う。editorial change は差分確認、関連する incremental build、必要な変更ページの render で再検証し、independent review を再度開かなくてよい。
- draft PDF を公開または読者へ共有する前に、その snapshot に含まれるすべての unit の status と必要な review を確定する。公開済み draft を変更後に再公開する場合は、変更分類に応じた affected scope の再検証を完了し、変更後 snapshot に対する gate と review status を更新する。`unreviewed internal WIP` と snapshot、未通過 gate、未完了範囲を明記して配布対象から隔離した private preview は、全体 review 前にも生成してよい。
- `reviewed`、`unreviewed`、`WIP` を project の `STATUS` と成果物本文の双方で識別し、snapshot と表示が一致していることを確認する。open blocking finding がある unit を `reviewed` と表示してはならない。
- draft は、読者が完成範囲と未完成範囲を正しく判断できる可読品質にする。未完成の後半は明示した placeholder または roadmap として隔離し、未完成の証明を完成済みの本文のように掲載しない。
- `out/` への promotion 前には、unit ごとの既存 review に依存せず、成果物全体を一つの snapshot として全 phase で再 review する。

## phase 1: inventory と truth status

レビュー対象の source revision、想定読者、self-contained の範囲、利用可能な定義・結果・出典、対象となる本文・見出し・定理環境・表・図・caption・演習・付録を確定する。
対象外とした範囲も記録し、未確認部分を確認済みと混同しない。
source の完全被覆を要求する project では、登録済み inventory だけを review universe にしない。authoring と inventory 作成から独立した reviewer が、採用した raw source snapshot の先頭から末尾までを page、logical range、rendered region 等の原形式に応じて直接走査し、すべての content-bearing span/region/range が独立して意味照合できる粒度の leaf Source Unit に少なくとも一つ所属し、leaf 間の重なりが明示した包含または cross-reference に根拠を持つことを inventory と照合する。catch-all unit、異なる実質要素の束ね、未分類領域、位置不明領域、意図しない重複、欠落を blocking とし、登録済み Source Unit の件数や downstream mapping が整っていることを raw-inventory audit の代用にしない。

すべての主要な主張について truth status を次から明示し、本文の modality と一致させる。

- proved: 本文または合意済み依存から証明済み
- conditional: 明示した仮定または外部結果の下で成立
- heuristic: 直観・近似・経験的説明であり証明ではない
- conjectural: 予想であり未証明
- open: 未解決問題

著者の主張、引用した結果、reviewer の訂正・解釈、編集上の補足を混同しない。

## phase 1b: 成果物 identity と教育設計 audit

原書、論文その他の source 群から再構成講義ノートを作る場合は、数学的 fidelity と完全被覆の検査とは別に、最終成果が独立した講義ノートとして設計されているかを検査する。source の順序や文章を保つことを fidelity と取り違えない。完全被覆と成果物 identity は独立した hard gate であり、一方の合格、件数、品質を他方の代用または相殺に使わない。同一の source inventory、destination source、coverage matrix revision に対して両方を検査し、どちらかの finding を直す編集が本文、構成、mapping または scope を変えた場合は affected scope の両 gate を再度開く。

- まず source と照合せず成果物だけを読み、想定読者、学習目標、章節ごとの中心的な問い、必要な前提、到達点、最小読解経路が理解できるか確認する。source を知らなければ文章の目的や接続を理解できない unit は major とする。
- 各 Destination Unit に、動機、見通し、定義、概念説明、主張、証明、例・反例、比較、応用、振り返り、navigation 等の教育的役割と、その位置に置く理由があるか確認する。原書の次の paragraph だから続く、という理由を認めない。
- source と照合し、原 chapter、section、paragraph、example、exercise の境界・順序・接続表現が、教育上の検討なしに成果物へ一対一で残っていないか確認する。数学的依存により順序が一致する場合や、statement・定義・不可避な proof structure が対応する場合は、その必然性を記録すればよく、差を作るためだけの変形は求めない。
- 原文の連続 passage を逐次訳した後で見出しや順序だけを変えたもの、単語を置換した paragraph、source ごとの塊を接続しただけの章を再構成済みと判定しない。概念 cluster と dependency graph を単位に統合・分割し、成果物固有の問い、説明、比較、接続、まとめを持たせる。
- source-derived な意味、仮定、modality、proof idea、例の役割、出典上の帰属は保持する一方、直接引用、歴史的見解、著者間の相違を示す箇所等を除き、原著者を成果物の継続的な語り手にしない。本文の基本 voice が講義ノート自身の説明になっているか確認する。
- 再構成の成否を語彙差、文の長短、原文との表面的な非類似度だけで判定しない。教育構造を変えずに言い換えただけのものは不合格とし、意味保存に必要な定型的表現の一致は問題としない。
- identity 改善のための統合、分割、移動、短縮後に、収録対象の各 leaf Source Unit から Destination Unit への forward mapping を全件照合し、仮定、一般性、modality、proof idea、非自明な推論、例・反例・図表・脚注・演習の教育的役割、歴史的・書誌的意味、source 間の差が保存されているか確認する。一つの Destination Unit へ多数の Source Unit が対応していても、mapping edge ごとの意味照合なしに covered と数えない。抜取検査、coverage 率、総数等式だけで完全被覆を認定しない。

## phase 2: definition-before-use audit

記号、用語、略語ごとに first use と definition の索引を作る。
本文だけでなく、タイトル、見出し、定理・定義、表、図、caption、脚注、演習、解答、索引も検査対象とする。

- 形式的な記号、用語、略語は使用前に必ず定義する。first use が definition より前なら blocking とする。
- 定義文を終端とみなさず、その定義に用いた数学的・技術的な語を新たな dependency として再帰的に調べる。日常語風の言い換えの中にある専門語も対象とし、例えば「弧状連結」を「道でつながる」と定義するなら、数学的な「道」が既知の前提でない限り先に定義するか正確に参照する。各 work unit について定義依存 graph の推移閉包を想定読者が既知としてよい前提まで辿り、未定義 node、曖昧な同義語置換、正当化されない循環を blocking とする。
- 新出の専門用語は正式導入の初出で原語を併記し、glossary の原語・採用訳・異綴り・初出・正規索引項目と照合する。独立に読める章節で再導入する場合は原語付き定義へのリンクまたは原語の再掲を確認する。
- 定義には対象の型、domain、codomain、引数、添字範囲、有効範囲を必要なだけ含める。
- motivation で後の概念を自然語により preview してよいが、未定義の形式記号や未導入の推論には依存させない。
- 同じ記号の再定義、重なる scope での別用途、表記だけ異なる重複概念、略語の揺れを検査する。
- 定義環境は原則として一つの概念を定義する。複数概念、補助構成、性質、結論を一つに詰め込まない。

## phase 3: statement と dependency audit

各定義、補題、命題、定理、系、例、演習に対して、直接依存する定義・仮定・先行結果・外部結果を追跡する。

- formal な数学的 assertion を prose に埋め込まず、definition、theorem、lemma、proposition、corollary、example、exercise など意味に合う semantic environment に置く。motivation や直観の prose は許容するが、formal claim の代用にはしない。
- assertion inventory は通常 prose だけでなく、example、remark、caption、脚注、演習、解答、proof 内の補助主張を含む全 content-bearing container を対象に作る。文が特定の対象について性質・関係・存在・一意性・等式・不等式等を真として述べるかを判定し、「実際」「なぜなら」「これを見るには」「したがって」等による論証の開始を強い検出 cue とするが、cue の有無だけで判定しない。主張の一般性、再利用回数、短さ、原資料の environment 名は formal 性を否定する理由にならない。
- example 内で対象の構成・計算・解釈とは別に「この写像は連続である」のような真偽を持つ assertion とその論証がある場合、単一対象に固有でも、内容に合う番号付き theorem-like statement と明示した proof へ抽出する。example には具体的設定と適用を残し、statement と相互参照する。純粋な値の列挙や、別個の assertion を述べない一続きの計算は機械的に theorem 化しない。review artifact には各候補の原 container、判定、抽出先または非 formal とした根拠を残し、未分類候補と未抽出 formal assertion を 0 件にする。
- theorem-like statement には必ず自動生成の番号と一意で安定した `label` を与える。本文から theorem、definition、番号付き proof、equation を指すときは `label` による参照を使い、手書きの番号を使わない。
- domain と codomain、量化子の順序、添字範囲、変数の scope を確認する。
- 定数が何に依存するか、局所的な主張か一様な主張か、点ごとか概収束・ほとんど至る所かを明示する。
- 存在、一意性、well-definedness、境界値、退化ケース、場合分け、極限操作を確認する。
- 必要に応じて可測性、可積分性、連続性、コンパクト性、完備性、閉性など、使用する外部結果の前提を確認する。
- 各仮定が証明で使われているか、使われない仮定が不要か、暗黙に使った仮定が欠けていないかを調べる。
- 結論の各部分がどの仮定・結果に依存するかを確認する。
- 相互・帰納・余帰納定義等の正当な循環は一つの明示した dependency node として well-definedness または基礎付けを検証する。それ以外の循環依存、未宣言の前方依存、存在しない参照、定義と定理の相互依存を禁止する。
- 定理 statement は仮定と結論に集中させ、長い構成、証明、解説は環境外へ置く。

依存台帳がある project では本文と台帳を双方向に照合する。
食い違いを推測で解消せず、証明と定義に基づいて双方を更新する。

## phase 4: proof gap audit

証明を含意ごとに分解し、次を一つずつ検証する。

- すべての proof を明示した proof environment に置く。直前の theorem-like statement を証明する通常の proof は、見出しを原則として「証明」とする bare な無番号 proof environment とし、proof 番号、proof 自体の `label`、対象 statement への明示的な cross-reference を付けない。statement から離れた証明・解答には、対象 statement を `label` で示す無番号の説明見出しを用いてよい。自動生成番号と一意で安定した `label` のある proof environment は、複数の証明・別証明を区別する場合、または proof 自体を他所から参照する場合に限り、対象 statement も `label` で示す。
- 各 proof について、goal、hypotheses と変数の domain、construction または必要な intermediate claim、各 implication の justification、尽くすべき cases、target conclusion を識別する。末尾では得られた結論が target conclusion であることを明示する。
- 各 implication の根拠が定義、先行結果、外部定理、または明示した直接計算に対応するか。
- 暗黙の補題を必要としていないか。原文と合意した依存から妥当に復元できる非自明な中間推論は、読者が追えるよう具体的に展開する。必要なら statement と証明を追加するか、正確な外部依存として記録する。再利用する、または構造上独立した非自明な intermediate claim は、番号と `label` のある lemma として分離する。成立しない gap や原文の誤りを創作によって修正せず、source issue と訳注または review record で追跡する。
- 代数変形、等式・不等式、符号、零除算、根号・対数等の domain、極限操作、写像の well-definedness を、読者が追跡できる step 列として示す。等号・不等号の向きと、極限と演算の交換の根拠も確認する。
- 全 cases が排反かつ尽くされているか、境界・空集合・零・低次元などの退化ケースが落ちていないか。
- 「明らか」「容易」「標準的」「同様」だけを根拠として step を省略しない。これらを使う場合も、直後に具体的な定義、置換、計算、先行結果、または推論を示す。「同様に」では実際の置換、前の議論との差分、追加条件を記す。
- 外部定理は名称、原典の定理番号と位置、正確な statement を示し、適用箇所ですべての条件が成立することを検証する。
- 合意した self-contained scope 内の外部定理は、前項に加えて本文または付録に完全な proof を持たせる。「外部結果である」と記すことや引用だけを proof の代用にしない。原資料にない proof は訳注・編者補足等として由来を区別し、追加した proof 自体とその依存を independent review する。proof を収録しない外部結果は、ユーザーが既知の前提として scope から除くことに明示合意し、読者向けの前提一覧と review artifact に結果、理由、影響範囲を記録した場合だけ許容する。
- 想定読者に routine でも、合意した self-contained scope 上で非自明なら説明または承認済み外部依存が必要である。

proof の十分な長さに任意の語数・page 数の下限を設けない。
想定読者と合意した self-contained scope に対して、すべての非自明な step が明示され、reviewer が statement から conclusion までの proof dependency trace を再構成できることを十分性の基準とする。
再構成できなければ blocking とし、単なる冗長な反復で長さを増やすことは求めない。

番号付き step の採否も proof の長さでは決めない。構成と検証、二方向の含意、場合の切替、置換と結論、補助観察と適用等、局所目標・入力・結論が切り替わる landmark があり、視覚的な区分で dependency trace が明瞭になるなら、短い proof でも step に分けてよい。一つの原子的推論を見かけだけの複数 step に分ける必要はないが、「短いから」という理由だけで有用な区分を退けない。reviewer は step の過不足を、語数ではなく各区分の局所目標、入力、結論と後続依存が説明できるかで判定する。

unit ごとに statement、proof、definition、label、ref の件数を集計し、少なくとも proof を要する statement に対応する proof の欠落、構造上対応先を同定できない proof、番号付き proof に必要な対象 statement 参照と label の欠落、未定義・重複 label、broken ref、orphan proof がすべて 0 件であることを checklist に記録する。

計算機、CAS、proof assistant、検索結果は検証証拠になり得るが、入力、前提、適用範囲、再現方法を記録し、人間が読める論証との関係を示す。

## phase 5: examples、counterexamples、exercises

- 例が定義の全条件を満たし、主張した性質を実際に持つことを計算または証明する。
- 例に埋め込まれた独立した formal assertion とその justification が phase 3 の statement/proof へ抽出され、例の具体的設定・適用・教育的役割と相互参照されているか確認する。「例だから」「単一対象だから」を未抽出の理由にしない。
- 非例・反例が否定対象を正確に破り、より強い別主張だけを破っていないことを確認する。
- 例の特殊性を一般的証拠として扱わず、heuristic と proof を区別する。
- 演習は導入済みの定義・結果だけで解答可能か、問題文の条件が十分か、解が存在するか、曖昧な複数解釈がないかを実際に解いて確認する。
- hint、解答、難度、依存先、本文の使用箇所が互いに整合するか確認する。

## phase 6: notation と LaTeX audit

- 定義、定理、命題、証明、例、演習には意味に合う semantic environment を使い、見た目だけの手動見出しで代用しない。
- 括弧の対応と scope、上付き・下付き、添字範囲、演算子、delimiter、式番号、数式内 punctuation を確認する。
- `label` の一意性、`ref` の存在と参照対象、定理・番号付き proof・式・定義・図表番号、文中の参照名を build 結果と照合する。未定義・重複 label、broken ref、orphan proof を 0 件にする。
- anchor/link inventory を作り、少なくとも work unit の入口から必要な前提・定義・先行結果へ、新出用語から原語付き定義と正規索引項目へ、theorem-like statement から対応 proof・使用する先行結果・主要な利用箇所へ、学習目標から対応本文と最小読解経路へ、本文から対応する学習目標へ辿れる必須 edge を検査する。目次、学習目標、本文内参照、索引を入口とする link graph で全 content-bearing node が到達可能であり、必要 edge の欠落、到達不能 node、誤った向き・移動先を 0 件にする。
- project の house macro は定義、引数、意味、scope を確認し、未定義 macro、意味の衝突、表示だけで意味を担う用法を残さない。
- TeX の compile 成功だけで数式の意味が正しいとは判定しない。PDF 上でも theorem、番号付き proof、equation、definition の番号と参照、無番号 proof と直前 statement の対応、欠落 glyph、改行、式の切断、図表との対応を目視確認する。
- 参照する既存成果物が指定された場合は、判型、実効 font size、baseline、版面、段落 indent、見出し階層、theorem/proof、目次・索引、header/footer を一組として比較する。参照元の内容固有 macro は移植せず、point 数または page への収まりだけで大小を判定せず、actual-size と fit-width の両方の render で本文密度、可読性、階層と柱を確認する。
- upLaTeX 文書で日本語索引を作る場合は、原則として upmendex を用いる。別の索引処理系は、案件要件が必要とする場合、またはユーザーが明示的に決定した場合に限り採用し、その決定を review artifact に記録する。
- 日本語索引の `see` は別名・表記揺れから正規項目への参照とし、project 全体で矢印等の別方式を明記しない限り、全角読点を用いた「別名，正規項目を見よ」の順で組む。「別名, を見よ 正規項目」のような英語順は不可とし、`seename` の訳語だけでなく formatter と語順を検査する。正規項目だけが page 参照を持ち、別名・表記揺れは存在する正規項目への `see` 参照だけを持つことを確認する。`seealso` を採用する場合は関連項目への参照として別に扱い、「項目，関連項目も見よ」等の project 共通表示を定める。参照元項目は自身の page 参照を併記してよく、別名専用の制約を `seealso` に適用しない。
- self-contained な書籍、長編講義ノート、長編数学ノートは、明示的な scope 除外がない限り、収録章節を案内する目次と巻末の用語索引を持つことを確認する。build gate では目次と索引の生成成功、採用した目次処理系の artifact（LaTeX なら `.toc`）と索引処理系の入力・出力・ログ（makeidx/upmendex なら `idx`/`ind`/`ilg`）の存在と非空性、索引処理の warning/error がないこと、索引の目次掲載、ページ参照、PDF bookmark と実際の render の一致を検証する。さらに、生成されたすべての `see` と、採用時にはすべての `seealso` について索引入力・出力と PDF の件数一致、各 formatter の句読点と語順、`see` 別名への page 参照 0 件、未解決 target 0 件、孤立した「を見よ」「も見よ」と不適切な段・page 分割 0 件を確認する。長い多言語項目を含む独立 fixture は採用する formatter ごとに用意し、それぞれの表示と分割防止を再現する。

## phase 6b: 図表の原図照合と render audit

原図がある図表ごとに、原図と再構成 source の要素 inventory を作る。軸、目盛、ラベル、矢印、領域、境界、陰影、色、凡例、caption に加え、投影、視点、曲率、位相、接続、遮蔽、前後関係、曲面・立体面の全面積と可視範囲を要素単位で照合する。球面、半球面等の曲面・立体面を輪郭線だけで代用してはならない。

- 自由な近似、目視だけによる作図、見た目だけの代用を忠実な再現とみなさない。位置合わせが可能な図は、縮尺、向き、基準点を合わせた registered overlay により原図と再構成結果を比較する。位置合わせが困難な場合は、その理由と、control point、寸法、角度、投影、曲面・立体面の全面積と可視範囲等による同等の定量比較を review artifact に記録する。いずれの場合も各要素の presence、placement、geometry、semantics と比較結果を記録する。
- build 後の PDF render を原図および再構成 source と再比較し、縮尺、線幅、文字、切断、重なり、配置によって意味または幾何が変わっていないことを確認する。
- 要素または曲面・立体面の欠落、球面・半球面等を輪郭線だけで代用したもの、意味を変える歪み、投影・曲率・位相・接続・遮蔽・前後関係・全面積・可視範囲の不一致は blocking とし、解消されるまで対象 unit を `reviewed` と表示せず promotion しない。

## 文書種別ごとの追加検査

### 書籍翻訳・原書群に基づく再構成講義ノート

- 数式、量化、否定、条件、modality、定義域、式番号を、記号・意味・論理単位で原文と厳密に照合する。日本語の語順や構文の逐語性より、意味、論理、合意した self-contained 性と文脈上の行間が正確に伝わることを優先する。
- source error の疑いを黙って修正しない。原文、問題、採用判断、根拠を private review record に残し、必要に応じて訳注・正誤注として原著者の主張と区別する。
- 記号統一や節の再構成が原文の依存順と意味を変えていないか確認する。
- 再構成講義ノートでは phase 1b を適用し、source inventory の全 leaf Source Unit の完全被覆と、成果物の章節・説明単位・voice の独立した教育設計が同一 snapshot で両立しているか確認する。source order、paragraph 対応、原著者の語りを保持しただけの成果を、正確な翻訳であっても完成した再構成講義ノートとは判定しない。逆に、教育設計を独立させた結果として内容が欠落、弱化、無根拠に一般化した成果も不合格とする。

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

- [ ] source 群から再構成する unit では、phase 1b の成果物 identity audit が完了し、固有の学習目標、中心的な問い、前提、到達点、教育的役割と説明戦略があり、連続 passage の逐次訳または原 paragraph の言い換えになっていない。
- [ ] source 群から再構成する unit では、収録対象の全 leaf Source Unit に forward mapping と mapping edge ごとの意味照合があり、全 leaf Destination Unit に reverse mapping または明示した編者補足種別がある。統合・分割・移動・短縮による意味、仮定、一般性、proof idea、非自明な段階、教育的役割、source 間の差の欠落が 0 件で、identity gate と coverage gate が同じ revision に対して有効である。
- [ ] source の完全被覆を要求する unit では、登録済み Source Unit から逆算せず raw source snapshot を直接走査した independent inventory audit が完了し、未登録 content-bearing span/region/range、catch-all unit、誤った leaf 粒度、意図しない重複が 0 件である。
- [ ] definition-before-use violation が 0 件である。
- [ ] 定義依存 graph の推移閉包に、未定義の技術語、曖昧な同義語置換、正当化されない循環が 0 件であり、新出専門語の原語と glossary・索引が整合している。
- [ ] unjustified step が 0 件である。
- [ ] unlabeled formal claim が 0 件である。
- [ ] example、remark、caption、脚注、通常 prose を含む assertion inventory に、未分類候補と未抽出 formal assertion が 0 件である。
- [ ] proof の番号付き step の有無が長さではなく論理的 landmark と navigation で判定され、有用な未分割と根拠のない過剰分割が 0 件である。
- [ ] self-contained scope 内で使用する外部結果が完全な proof を持ち、proof のない外部依存はすべてユーザー合意済みの scope 除外として記録されている。
- [ ] unit 内の anchor/link inventory と必須 edge が揃い、unit の入口から必要な前提・定義・先行結果へ到達でき、broken link と到達不能 node が 0 件である。
- [ ] numbering error と reference error が 0 件である。
- [ ] 直前の statement に続く通常の proof が、原則「証明」の見出しを持つ bare な無番号 proof であり、不要な proof 番号、proof label、対象参照が 0 件である。離れた証明・解答の無番号説明見出しには対象参照があり、番号付き proof は複数・別証明または proof 自体が参照対象となる場合に限定され、必要な proof label と対象参照を持つ。

WIP は draft に限り、状態、未完了箇所、依存不能範囲を `STATUS` と本文で明示して完成 unit から隔離できる。
WIP を `reviewed` の本文に混ぜず、`out/` では WIP を 0 件にする。

## phase 8: independent re-review と promotion gate

修正担当と分けた reviewer が、少なくとも変更箇所、直接依存、直接の利用箇所、全 blocking finding を read-only で再確認する。
次をすべて満たした場合だけ `out/` への promotion を許可する。

- [ ] blocking finding が 0 件である。
- [ ] major finding が解消済み、または影響を理解した明示合意と理由が記録されている。
- [ ] source 群から再構成した成果物では、phase 1b の全体 audit と source comparison が完了し、原書順・paragraph 対応・原著者の継続的 voice に依存しない講義ノート固有の教育構造が検証済みである。
- [ ] source 群から再構成した成果物では、採用 source snapshot の全 leaf Source Unit と成果物の全 leaf Destination Unit に対する双方向 coverage と edge ごとの意味保存を全件検証し、未処理、未統合、出典・補足種別不明、弱化、強化、欠落、broken mapping が 0 件である。identity review と coverage review は同一 revision を対象とし、どちらかの後の変更で stale になっていない。
- [ ] source 群から再構成した成果物では、inventory 作成と authoring から独立した reviewer が、採用 raw source snapshot の全 page/logical range/rendered region を直接再走査する raw-inventory audit を完了し、すべての content-bearing 領域と正しい粒度の leaf Source Unit が全件対応している。登録済み inventory、coverage 率、schema の整合だけから完全性を推定していない。
- [ ] definition-before-use violation が 0 件である。
- [ ] 定義依存 graph の推移閉包、専門用語の初出原語、局所読解用の定義・前提リンクが検証済みである。
- [ ] unjustified step と unlabeled formal claim が 0 件である。
- [ ] 全 content-bearing container の assertion inventory に未分類候補と未抽出 formal assertion が 0 件であり、example 内から抽出した statement/proof と元の例の相互参照が検証済みである。
- [ ] self-contained scope 内で使用する外部結果が完全な proof を持ち、proof のない外部依存はすべてユーザー合意済みの scope 除外として記録されている。
- [ ] anchor/link inventory の必須 edge がすべて存在し、目次・学習目標・本文内参照・索引から全 content-bearing node が到達可能で、必要 edge の欠落、broken link、誤った移動先、到達不能 node が 0 件である。
- [ ] statement、proof、definition、label、ref の件数を記録し、statement と proof の構造上の対応、番号付き proof に必要な label・参照、その他の label・参照に欠落がなく、未定義・重複 label、broken ref、orphan proof が 0 件である。
- [ ] 通常の直後 proof、離れた証明・解答、番号付き proof の見出し・番号・label・対象参照が phase 4 の規則に一致している。
- [ ] WIP、unreviewed unit、未完成の proof が 0 件である。
- [ ] 未追跡の仮定と依存が 0 件である。
- [ ] truth status、source fidelity、文書種別ごとの追加検査が完了している。
- [ ] build、参照、数式表示の検査が完了している。
- [ ] 索引の全 `see` と、採用時には全 `seealso` が phase 6 の各意味と日本語表示に従い、`see` の正規項目と別名の page/参照分担、各 target の存在、各 formatter の件数・句読点・語順・段・page 分割、および formatter ごとの独立 fixture の gate を満たしている。
- [ ] 成果物全体について全 phase の再 review と independent re-review が完了している。
- [ ] review artifact の保存先が private local area であり、public 差分に混入していない。

gate を満たさない成果物は `draft/` に留める。
blocking finding は単なる waiver では閉じない。
対象部分を成果物と self-contained scope から除外する場合は、影響、依存不能範囲、責任者、ユーザーの明示合意を private record に残し、残った成果物について gate を再実行する。
