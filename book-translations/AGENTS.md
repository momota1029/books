# 原書群に基づく再構成講義ノート制作ガイド

## 適用範囲と正本

この `AGENTS.md` は `book-translations/` とその配下だけに適用する。原書群に基づく再構成講義ノート制作以外へ固有規則を持ち込まない。上位の指示と衝突する場合は上位を優先し、判断できなければ作業を止めて確認する。

各 project は親の `../AGENTS.md`、`../JAPANESE_WRITING.md`、`../MATH_PROSE_REVIEW.md` と repository root の version 管理された `../.system/` tools を直接使う。これらの private copy を作って正本にしてはならない。案件固有の adapter が必要なら、差分と理由だけを project の private area に記録する。日本語の原稿・訳注・編集文は `../JAPANESE_WRITING.md` に従う。source-derived な数学的内容、条件、modality、proof idea、出典上の帰属は文体上の都合より優先して保存するが、原文の構文、paragraph、章節順、接続、継続的な著者 voice は最終成果の構成原理にしない。直接引用、歴史的見解、著者間の相違等を除き、完成本文の基本 voice は再構成講義ノート自身のものとする。

## 目的

利用権限を確認できる複数の英語書籍を資料群として受け入れ、原書を先頭から順番に翻訳して連結するのではなく、概念、依存関係、教育上の導線に従って全面的に再構成した日本語講義ノートを単一の PDF として作る。合意した収録範囲の内容を過不足なく保持し、非自明な行間を残さず、未解答の演習を置かない。正確な翻訳、要約、再構成、訳注、編者補足を区別し、各記述を原資料まで追跡可能にする。

想定成果物は、統合 PDF、再現可能な編集 source、書誌・版・権利台帳、原資料の全要素 inventory、原資料と再構成先の双方向 coverage matrix、再構成計画、依存・主張・演習の台帳、用語・表記規約、翻訳判断、照合・検証記録である。完成 PDF だけを残して編集根拠を失わず、原資料や中間生成物を公開成果へ無差別に含めない。

## 成果物の identity

- この領域の既定成果は「翻訳本」「抄訳」「複数翻訳の合本」ではなく、原書群を資料として用いた独立の日本語講義ノートである。原書の著者・章・page は provenance であり、読者向け構成の主語ではない。表題、序文、目次、running head、章の導入、本文の語り、まとめにもこの identity を一貫して反映する。
- 制作を source analysis と destination authoring の二層に分ける。source analysis では全要素を精密に解釈・翻訳して台帳化するが、その逐次訳を本文の下書きにしない。destination authoring では、学習目標、中心的な問い、concept index、dependency graph、想定読者から成果物固有の章節と説明戦略を設計し、該当 Source Unit を根拠として統合する。
- 原文の連続 passage を上から訳し、後から見出し、順序、接続語だけを変える workflow を禁止する。各 Destination Unit は、source paragraph ではなく、概念 cluster、問い、定義と結果の依存、比較、応用等の教育単位から書き始める。執筆後に source と照合して意味保存と coverage を確認する。
- 完全被覆は semantic content の完全被覆であって、原文の文数、段落境界、局所順序、修辞、反復、著者の進行実況を保存することではない。定義条件、主張、proof idea、必要な推論、例の役割、歴史的・書誌的内容等は失わない一方、複数箇所の説明を統合し、重複を解消し、成果物固有の説明へ書き直してよい。単なる言い換えや同義語置換は再構成とみなさない。
- 各章節は少なくとも、なぜ読むか、何を前提とするか、何を理解・証明できるようになるか、どの経路で到達するかが分かる学習上の契約を持つ。動機、見通し、定義、概念説明、主張、証明、例・反例、比較、応用、振り返り、次の入口を内容に応じて配置し、原書の「次の節だから続く」という接続を使わない。
- 複数資料が同じ概念を扱う場合は、source ごとの塊を順に紹介せず、共通核、仮定・一般性・記法・proof strategy・例の差を一つの説明へ統合する。単一資料にしか現れない内容も、原章節の翻訳として孤立させず、成果物内の目的と依存に応じた役割を与える。
- source の著者名や「著者は次に述べる」といった語りは、直接引用、歴史、帰属、相違の説明に必要な場合だけ本文へ出す。通常の説明は「本ノートでは」または無標の講義ノート voice で書き、source ごとの narrator の切替を読者へ負わせない。訳注を本文の主要な接続手段にせず、必要な provenance と編集判断は citation、注、巻末資料、private record へ適切に分離する。
- source と同じ構成が数学的・教育的に最適な箇所は一致を機械的に避けなくてよい。ただし、各一致について dependency または pedagogy 上の理由を reconstruction record に残す。差を作るためだけの不自然な並替え、数学的 statement や不可避な proof structure の表面的な言い換えも行わない。
- 再構成性と完全被覆性は独立した二つの hard gate とし、一方の合格、件数、品質を他方の代用または相殺に使わない。すべての Source Unit を mapping した逐次訳も、教育構造だけを独立させて内容を削ったノートも不合格である。同一の source snapshot、canonical source revision、coverage matrix に対して両 gate を通し、一方の finding を直す編集で本文、章節構造、scope または mapping が変わった場合は affected scope の両 gate と dependent unit を再検証する。

## 再構成型・完全被覆講義ノート契約

- ユーザーが資料を `inbox/` に置き、「講義ノートにする」等の短い指示だけを与えた場合も、本節の再構成、完全被覆、行間補完、演習統合、検証の全要件を既定で適用する。指示の短さを、逐次翻訳、要約、抜粋、検査省略または品質 gate 緩和の許可と解釈しない。
- 完全性会計は採用後の source snapshot からではなく、対象 project の `inbox/` で安定検出した全 file/revision から開始する。各候補へ Candidate Source ID を付け、既定では対象資料として採用する。無関係、形式未対応、権利不明、破損等を理由にエージェントだけで会計外へ出さず、`pending` または `blocked` として影響を示す。全内容の強い hash が一致する exact duplicate は採用済み canonical revision へリンクし、版違い等の supersede と成果 scope からの除外には対象、理由、影響とユーザーの明示合意を記録する。
- 「全内容」は、合意した source snapshot の本文、見出し、定義、記号、式、主張、証明、導出、説明、動機、例、反例、注、脚注、演習、解答、図、表、caption、歴史的・書誌的注記、付録その他、読者の理解または数学的意味に寄与する要素をいう。前付・後付、目次、原索引、参考文献等も inventory に登録し、本文へ統合するか、成果物固有のものへ再生成するか、明示的に対象外とするかを記録する。
- 原書の章節順は出典位置として保存するが、無検討に成果物の章節順として採用しない。成果物は definition-before-use、主張と証明の依存、動機から形式化への流れ、基礎から応用への順序に従って独立に設計する。その結果として原書順の一部または全体と一致する場合も、依存上または教育上の理由を記録する。
- 同内容の重複は無表示で反復せず統合してよいが、統合後の一箇所から全 Source Unit へ逆引きでき、各原資料の仮定、一般性、証明法、例、解釈の差が失われないようにする。相違を潰して見かけ上同一にすることは omission とみなす。
- 再構成は要約、抜粋、簡略版への変更ではない。読みやすさ、簡潔さ、紙幅、学習者への負担、章節の均衡、重複整理を理由に content-bearing な Source Unit を落とさない。反復を統合できるのは意味、仮定、一般性、modality、proof strategy、例示対象、教育的役割、歴史的・書誌的文脈まで同一か、相違を統合先で明示的に回収できる場合に限る。単に似ている、同じ用語を使う、結論が同じという理由で duplicate と扱わない。
- 原資料にない接続、補題、説明、例、比較、概観、振り返りまたは計算は、self-contained 性、教育上の導線、複数資料の統合、局所読解のために必要な範囲で追加し、編者補足として出典由来の内容と識別し、根拠と導入理由を記録する。「最小限」を逐語訳へ留まる理由にしない。根拠のない新規主張、過剰な一般化、原資料より強い断定は addition error として禁止する。
- 最終成果には読者へ解答を委ねる「演習」「問題」「課題」を残さない。原資料中の演習は一つ残らず解いて `exercise-ledger` に記録し、その問題が担う定義確認、計算、例、反例、補題、命題または応用と完全解答を、適切な本文、例、導出、定理・証明へ統合する。問題文固有の条件と教育的内容も coverage の対象とし、単に解答だけを載せて問題の意味を失わない。
- 原資料の誤り、不足条件、相互矛盾、解答不能箇所は、完全性を装うために創作で埋めない。該当 Source Unit、影響範囲、根拠、採用した訂正候補または制限を source issue として記録し、本文では原著者の内容と訳者・編者の分析を識別する。解消できない blocking issue があれば完成扱いにしない。
- 内容の省略または scope 除外は、対象 Source Unit、理由、失われる依存と読者への影響、代替策、ユーザーの明示合意を記録した場合に限る。「重複」「自明」「周辺的」「紙幅」等のラベルだけでは除外理由にならない。除外後は残る scope に対して inventory、coverage、依存および review gate を再計算する。

完全性は「丁寧に読んだ」等の自己申告で判定せず、以下の再構成ハーネス、件数照合、原文との独立 review によって立証する。自動検査は候補抽出と整合確認に使えるが、意味の保存、数学的正しさ、行間の不存在を単独では保証しない。coverage 率、総数等式、全 ID に mapping があるという構造検査だけで完全被覆と判定せず、収録対象の各 leaf Source Unit と各 mapping edge を抜取でなく全件、原資料と Destination Unit の間で意味照合する。

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

複数案件で有効な source-analysis・再構成 authoring prompt、用語処理、source mapping、build、QA の改善は、案件名、原文、書誌、用語、権利情報、ファイル名等を除去して generic 化し、privacy review と共通の検証を通して repository root の shared system へ upstream する。generic な改善を private project 内だけに残さない。案件固有の用語、権利判断、原文、manifest、config、adapter は private project に置く。

## プライバシーと Git 境界

repository mode は親の規則と `../.system/repository-mode` に従う。

- public mode では、すべての `<project-id>/` とその全内容、project 名、原文、原稿、PDF、manifest、台帳、config、案件固有 tool を stage、commit、push しない。`book-translations/` 内で Git 対象にできるのは privacy review 済みの `AGENTS.md` だけである。
- verified private mode では、configured remote の同一性と GitHub visibility が `PRIVATE` と検証できる場合に限り、inbox、draft、out、`.workspace/` を含む processing document を通常の `git add` で stage、commit する。stable な inbox revision、canonical source、records、draft PDF/`STATUS` を一つの recovery snapshot として、受け入れ確定後、意味のある draft 同期・build 成否の確定後、および親規約の各 checkpoint 境界で小さく commit・push する。互換用の `../.system/repository-mode add -- <paths>` も利用できるが、`git add -f` は使わない。
- private mode でも credential、secret、token、鍵、および configured private remote への複製自体を明示的に禁ずる契約・ライセンス・権利条件がある data は commit しない。翻訳権・公開権・再配布条件が未確認であることだけでは、stable な inbox revision を未採用の `pending` input として private recovery snapshot に保存しない理由にならない。private remote の存在は資料を翻訳対象として採用する権限や公開・配布する権限を与えず、それらは別に確認する。
- `out/` の配布承認と GitHub repository の visibility・commit 可否は別の判定である。配布承認済みでも GitHub に置けるとは限らず、private GitHub に置けても配布できるとは限らない。

`.gitignore`、repository mode、hook を回避せず、`--no-verify` を使わない。status、diff、作業報告にも、私的な原文、タイトル、著者、研究内容、ファイル一覧、hash を必要なく転記しない。この構造と ignore 規則は defense in depth にすぎず、secret store、暗号化、アクセス制御の境界ではない。

## 永続データの配置

- `<project-id>/.workspace/source/`: 再構成講義ノート本文、成果物固有の章構成、注、索引、成果用に作成した図表等の canonical source。逐次訳等の source-analysis 記録を最終本文の正本として置かない。
- `<project-id>/.workspace/records/content-inventory.*`: 採用 source snapshot の全要素を安定した Source Unit ID へ分解した inventory。Source ID、版、原章節、page、logical unit、rendered region、time range 等の形式に応じた安定位置、要素種別、親子関係、短い内容識別子、参照先、図表・脚注等との包含関係、状態を持つ。Source Unit は独立して対応付け・意味照合できる最小の実質要素とし、異なる定義、主張、証明内の実質的論法・中間推論、導出、例、注、脚注、演習の各条件、図表の semantic element、caption を件数削減のため一つに束ねない。原目次・原索引・抽出結果は発見補助として利用できるが、page または page を持たない形式の全 logical unit、rendered region、time range の走査と原資料照合を省略しない。
- `<project-id>/.workspace/records/raw-inventory-review.*`: inventory 作成と authoring から独立した reviewer が、登録済み Source Unit を出発点にせず、採用 raw source snapshot の全 page、logical range、rendered region または time range を先頭から末尾まで直接再走査した記録。各 content-bearing span/region/range と leaf Source Unit の対応、粒度、包含、重複、判読不能箇所、catch-all unit、finding、修正後の再検証、reviewer、対象 snapshot/revision を持つ。downstream mapping、coverage 率、Source Unit 件数が整っていることをこの review の代用にしない。
- `<project-id>/.workspace/records/source-map.*` または `coverage-matrix.*`: 収録対象の各 Source Unit から再構成先の chapter/section、定義、主張、証明、段落、図表、索引項目等への forward mapping と、成果物の各実質要素から Source Unit または明示した編者補足への reverse mapping を持つ双方向 coverage matrix。`excluded-by-agreement` の Source Unit は本文 destination の代わりに有効な除外判断と影響評価へ参照する。成果物側も由来を独立に照合できる leaf Destination Unit に分解し、全 content-bearing span/region が少なくとも一つの leaf Destination Unit に属するよう inventory を作る。同じ段落内で出典由来部分と編者補足が混在する場合は論理的 subunit を分け、leaf 間の重なりは明示した包含または cross-reference 以外に残さない。排他的 disposition を `pending`、`integrated`、`merged`、`exercise-integrated`、`source-issue-resolved`、`excluded-by-agreement`、`blocking-open` で区別し、`expanded-gap` 等の変換種別と、意味照合、mapping 照合、independent review の disposition 別必須 flag、検証者、対象 revision を別 field で記録する。各 mapping edge は、保存対象となる statement/definition、仮定、一般性、modality、proof strategy と非自明な段階、例・反例・図表・脚注・演習の役割、歴史的・書誌的意味、他 source との差、およびそれらの Destination Unit 内の所在を照合できるようにする。多数の Source Unit が同じ Destination Unit を指すだけの many-to-one mapping を、edge ごとの比較なしに covered としない。
- `<project-id>/.workspace/records/reconstruction-plan.*`: 原書順から独立した概念 outline、想定読者、self-contained scope、章節の教育目的、採用する順序と統合・分割の理由、各章節が消費する Source Unit 群を記録する。
- `<project-id>/.workspace/records/lecture-note-identity-review.*`: 成果物固有の学習目標、章節ごとの中心的な問い・前提・到達点・説明戦略、source structure との比較、source と同じ順序を採用した箇所の理由、逐次訳・一対一 paragraph 対応・source narrator の残存 finding、修正と独立再検証を記録する。
- `<project-id>/.workspace/records/learning-objective-map.*`: 成果物冒頭および各章節の学習目標へ一意で安定した Objective ID を付け、各目標の前提、対応する Destination Unit、到達を支える定義・主張・証明・例・応用、最小読解経路、本文からの reverse mapping、対象 revision を記録する。成果物冒頭の目標を宣言だけで終わらせず、部分的に読む学習者が目標から該当箇所と必要な前提へ直接移動できる構造を持たせる。
- `<project-id>/.workspace/records/dependency-graph.*` および必要な claim ledger: 用語・記号の定義と first use、定義文内部の技術語を含む推移的な定義依存、各 formal claim の truth status、原 container、検出 cue、仮定、直接依存、証明、外部依存、利用箇所、semantic environment への抽出先を追跡し、再構成後の順序と本文の参照を双方向に照合する。同時定義、帰納的定義、余帰納的定義等の正当な相互依存は一つの明示した dependency node とし、well-definedness または適切な基礎付けを検証する。その他の循環依存は禁止する。example、remark、caption、脚注、通常 prose を含む全 content-bearing container の assertion 候補を分類し、未分類候補と未抽出 formal assertion を 0 件にする。
- `<project-id>/.workspace/records/claim-audit.json` と `claim-audit-report.json`: repository 共通の `../.system/book-claim-harness` が canonical TeX source の example、remark、caption、footnote、通常 prose を全件走査して管理する assertion 分類台帳と検査 receipt。自動検出した container ごとに `pending`、`no-formal-assertion`、`formal-assertions-extracted`、`formal-assertion-embedded` を排他的に記録する。`no-formal-assertion` には非 formal とする数学的役割と根拠を、`formal-assertions-extracted` には原 Source Unit、mapping edge、原主張の一般性、抽出先の番号付き statement と直後の proof、元 container の anchor、意味保存と一般化回避、および相互参照を記録する。分類、意味保存、相互参照は authoring と分類の双方から独立した reviewer が全件確認する。justification cue が残る否定分類または抽出済み container は cue の非 assertion 的役割と独立 review を伴う明示 waiver がなければ gate を通さない。source 変更後は再走査により変更 container の旧分類を無効化し、同一 content hash の不変 container だけを再利用する。
- `<project-id>/.workspace/tools/book-claim-audit.json`: 前項の project identity と固定 scan profile。新規 project では initializer が生成する。別 project からコピーせず、canonical source を `.tex` 以外へ変更する必要がある場合は未対応形式を黙って検査外にせず shared harness の拡張または明示した blocker として扱う。
- `<project-id>/.workspace/records/exercise-ledger.*`: 原資料中の全演習について Source Unit ID、条件、意図、依存、完全解答、原問の不備、本文への統合先、統合後の semantic environment、`resolved`、`resolution-verified`、`content-integrated` の各状態を記録する。`resolution-verified` は完全解答、または不成立・解答不能であることの根拠を伴う分析の検証をいう。最終成果で `exercise_total = resolved = resolution-verified = content-integrated` を確認できるようにする。
- `<project-id>/.workspace/records/rights-ledger.*`: 書誌、版、出版社、年、ISBN、URL、参照日、入手元、ライセンス・許諾、利用根拠、引用・翻訳・翻案・図版・公開・配布条件、個人情報・秘密・契約制限の判定。
- `<project-id>/.workspace/records/glossary.*`: 原語、候補訳、採用訳、異綴り・別名、例外、定義、初出、独立章節での再導入、正規索引項目、記号、表記規則。新出専門用語は定義または正式導入の初出で原語を併記し、本文・glossary・索引を相互参照させる。
- `<project-id>/.workspace/records/translation-decisions.*`: 要約、再構成、訳注、矛盾、原文の誤り、欠落、未確定事項、照合結果と判断根拠。source issue は少なくとも `blocking-open`、`resolved-by-correction`、`resolved-by-qualified-presentation`、`resolved-invalid-exercise-analysis`、`excluded-by-agreement` を区別し、対象 Source Unit、本文処置、依存への影響、review 結果を持つ。
- `<project-id>/.workspace/records/source-ingestion-manifest.*`: `<project-id>/inbox/` の検出履歴と、採用した入力 snapshot。
- `<project-id>/.workspace/records/` のその他の用途別台帳: style、図表、QA、数学文章 review、公開判定等。台帳名と形式は案件開始時に決める。
- `<project-id>/.workspace/tools/`: 案件固有の build 設定、adapter、依存 lockfile。複数案件に有効な script、template、prompt、QA tool の正本はここに置かず shared system へ upstream する。

## 再構成ハーネスと完全性会計

まず source-set ledger として、安定検出した Candidate Source revision の総数と ID 一覧を固定し、`detected = adopted + exact-duplicate-linked + superseded-by-agreement + excluded-by-agreement + pending + blocked` を満たすことを検査する。同一 revision を複数 disposition へ重複計上せず、未分類を 0 件にする。`exact-duplicate-linked` は全内容の強い hash が同一で、採用済み canonical revision への参照を持つ場合だけ使う。版違い等の `superseded-by-agreement` と scope 除外はユーザー合意を必須とする。`pending`、合意のない除外、`blocked`、manifest と実在 file/revision の不一致が一つでもあれば完成候補に進めない。

1. 安定した採用 snapshot ごとに、page を持つ形式は先頭 page から末尾 page まで、page を持たない形式は全 logical unit、rendered region または time range を始端から終端まで走査し、`content-inventory` に Source Unit を登録する。PDF text 抽出、OCR、原目次、原索引、見出し検出、数式・図表検出、transcript 等を併用してよいが、いずれか一つの抽出結果を正本にせず、原 render または原データと照合する。各 content-bearing span/region/range は少なくとも一つの leaf Source Unit に属し、leaf 間の重なりは明示した包含または cross-reference 以外に残さない。parent unit は全 child の disposition と検証 flag が解決するまで covered としない。処理済み範囲を記録し、未分類領域、空白、判読不能、欄外、脚注、継続図表等を未確認のまま飛ばさない。
   inventory 作成後、prose authoring の大量開始前に、作成担当から独立した reviewer が raw snapshot 自体を先頭から末尾まで再走査する。reviewer は登録済み Source Unit の一覧を母集団として確認を始めず、raw page/logical range/rendered region/time range から content-bearing 領域を独立に列挙して inventory と突き合わせる。未登録領域、異なる実質要素を束ねた catch-all unit、誤った leaf 粒度、意図しない重複、位置不明・判読不能箇所を 0 件にし、`raw-inventory-review` を固定するまで inventory gate を通過させない。
2. prose authoring 前に、全 Source Unit を概念別に分類した provisional concept index、用語・記号 registry、claim/dependency graph、exercise ledger を作る。複数資料間の同義、包含、一般化、矛盾、記法差、証明差を明示し、機械的な文字列一致だけで同一概念と判定しない。
3. provisional concept index と dependency graph から、source の見出しを写さず `reconstruction-plan` を作る。各章節の中心的な問い、目的、前提、到達点、最小読解経路、説明戦略、必要な concept cluster を先に決め、その後で Source Unit 群を割り当てる。全 Source Unit に予定統合先、合意済み除外判断への参照、または承認待ちの disposition を与えるが、source ごと・chapter ごと・page ごとの逐次翻訳を work order にしない。
4. authoring は各 Destination Unit の問い、教育的役割、入力となる定義・結果、得る結論、隣接 unit との接続を記した brief から開始する。原文 passage を原稿欄へ置いて順に訳さず、concept/dependency records を根拠に成果物固有の説明を執筆してから、該当 source snapshot に戻って意味、modality、proof idea、例の役割、coverage を照合する。Source Unit を統合するたびに forward mapping を、成果物へ実質的な記述を加えるたびに reverse mapping を更新する。分割は one-to-many、重複統合は many-to-one として表現し、保存した意味と追加した説明を区別する。
5. work unit の checkpoint ごとに、Source Unit 総数、各 disposition、意味照合 flag、合意除外、source issue、統合先不明、Destination Unit 総数、成果物側の未分類領域・出典不明要素、assertion 候補・分類済み・formal claim 抽出済み・未抽出、定義依存の未定義 node、未追跡依存、外部結果・本文内 proof・合意済み scope 除外、concept 総数と配置済み数、演習総数、解決済み、resolution 検証済み、本文統合済みの件数を snapshot と共に記録する。Source Unit の排他的 disposition は `pending`、`integrated`、`merged`、`exercise-integrated`、`source-issue-resolved`、`excluded-by-agreement`、`blocking-open` とし、意味照合、mapping 照合、independent review は disposition ごとに schema が定める別の必須 flag とする。`source_unit_total` が全 disposition の和に等しいこと、`destination_unit_total = source-mapped + identified-editorial-addition`、`exercise_total = resolved = resolution-verified = content-integrated` を検査する。不成立演習は、根拠を検証した分析と本文統合が済んだ場合だけ resolved に数える。集計値だけでなく該当 ID 一覧へ辿れるようにする。
6. 各再構成章節について、成果物の各要素から原資料へ戻る destination-to-source audit と、収録対象の各 Source Unit から成果物へ進む source-to-destination audit の両方向を、抜取でなく leaf unit と mapping edge の全件について行う。合意除外した Source Unit は除外判断と影響評価を別に照合する。章節を跨ぐ仮定、例、図表、脚注、演習由来内容が脱落していないか、統合で意味が弱化・強化・重複していないかを原文と照合する。件数、coverage 率、同じ destination の存在だけでは意味照合を通過したことにしない。
7. authoring の進行に合わせて provisional concept index を更新し、完成候補では final concept index として固定する。各 concept は定義または正式導入、成果物内 destination、glossary/正規索引項目、関連 Source Unit、dependency graph node を持つ。未配置 concept、索引未登録の正式用語、存在しない destination、相互に不一致な concept/glossary/index/dependency record を 0 件にする。
8. 再構成で追加した全ての formal claim、補題、証明、説明、例について reverse mapping と種別を持たせる。出典由来でない formal claim は proof または正確な外部依存と truth status を必須とし、接続説明もどの gap、依存または教育上の必要を満たすか記録する。
9. checkpoint と完成候補で `lecture-note-identity-review` を行う。まず source を見ずに成果物だけを読み、学習目標、問い、前提、到達点、説明の連続性を確認する。次に各 source と比較し、連続 passage の逐次訳、原 paragraph・見出し・接続の一対一対応、source ごとの未統合 block、原著者の継続的 narrator を finding として記録する。数学的・教育的必然性のある一致を除き、これらが 0 件になるまで再構成済みとしない。
10. coverage review と lecture-note identity review は、同じ source snapshot、canonical source revision、coverage matrix revision を対象に別々に合否を記録する。一方の finding を修正した後は affected Source Unit、Destination Unit、mapping edge、dependent unit の両 review flag を stale にし、再照合する。source の追加・差替え、scope 変更、章節の統合・分割・移動、本文の短縮・書換え、例・図表・脚注・演習の再配置後に、変更前の完全被覆または identity の合格を流用しない。
11. 採用 source revision の追加・差替え・supersede、source scope の変更、または記録済み hash との不一致を検出した時点で、影響する raw range とその leaf Source Unit だけでなく、そこから推移的に到達する concept、用語・記号、claim、dependency、exercise、figure、mapping edge、Destination Unit、学習目標、索引・link、review flag、build、draft `STATUS`、promotion receipt を stale にする。既存 `out/` artifact は対応 snapshot の historical artifact として不変に保てるが、新 source に対する current/final と表示しない。影響なしとして再利用する unit は、raw content と unit boundary、意味、依存が不変である証拠を記録したものに限り、それ以外は raw inventory から coverage、identity、数学 review、build/render まで affected graph を推移的に再実行する。revision field または hash だけを更新して従前の合格 flag を引き継がない。
12. 完成候補では、`pending`、`blocking-open`、source と destination の未処理・未分類範囲、統合先不明 Source Unit、出典または補足種別が不明な Destination Unit、数学的意味・coverage・権利に影響する未解決 source issue、未定義語、未配置 concept、未追跡依存、未証明の要証明 claim、未解答演習、本文未統合演習、dangling ID、到達不能または broken mapping、stale snapshot/revision、stale raw-inventory review、stale coverage review、stale identity review、stale build/promotion receipt をすべて 0 件にする。`excluded-by-agreement` は 0 件条件の例外だが、ユーザー合意と影響評価が有効なものだけを別集計する。schema validator は、source/destination の全 content-bearing span/region の leaf unit 被覆、排他的 disposition、disposition 別必須 flag、総数等式、全 ID の参照整合、forward/reverse mapping の到達可能性、ledger・source・draft snapshot の一致に加え、anchor/link inventory の必須 edge と、目次・学習目標・本文内参照・索引から全 content-bearing node への到達可能性を検査し、必要 edge の欠落、broken link、誤った移動先、到達不能 node を 0 件にする。対象 snapshot、validator version、実行結果を記録し、該当 snapshot に対する validator 成功記録がなければ promotion しない。

台帳の形式は CSV、YAML、JSON、SQLite 等から案件に適したものを選べるが、安定 ID、双方向参照、差分、件数検査、snapshot 固定、review 記録を再現できなければならない。汎用化できる schema、validator、report generator、fixture は privacy review 後に repository root の shared system へ upstream し、案件固有 data は private area に残す。

## inbox 受け入れプロトコル

ingestion と source analysis・destination authoring を分離し、受け入れが確定した snapshot だけを制作入力にする。

1. 作業開始時、各処理 batch 開始時、PDF を `draft/` へ公開する直前、`out/` へ昇格する直前に、対象 project の `inbox/` を再走査する。
2. 検出結果を `.workspace/records/source-ingestion-manifest.*` に追記する。各 revision について、少なくとも相対 path、byte size、更新時刻、強い content hash の方式と値、初回検出時刻、処理状態、採用・保留・除外理由を保持する。採用する revision は全内容を読み取った強い content hash を必須とする。
3. 同一性を path、ファイル名、時刻だけで判断しない。採用前に時間を置いて 2 回以上検査し、各回で hash 計算の直前と直後の size、mtime 等の利用可能な stat が一致し、全内容の読み取りが成功し、全回の強い content hash が一致することを要求する。同名、同 size/mtime の内容変更も hash で新 revision として検出し、翻訳、引用、ページ対応、権利、既存 PDF への影響を評価する。
   変更 revision を採用または旧 revision の supersede として確定した場合は、再構成ハーネスの推移的 invalidation 規則を直ちに適用する。影響評価だけで従前の inventory、coverage、review、build、draft/out status を有効なまま残さない。
4. 書き込み・同期・lock 中、一時拡張子、stat/hash の不一致、読み取り・parse 失敗は `pending` として処理しない。ユーザーの完了合図は安定確認の代替ではなく再検査の trigger に限り、長い固定 sleep を前提にしない。採用後は権利・機密上許される場合だけ immutable snapshot を `.workspace/` 内の案件に適した永続・保護領域へ保存し、その所在と hash を manifest に記録する。保存しない場合は、入力を使用するたびと `draft/`・`out/` への公開直前に記録済み hash を再検証する。
5. archive は自動展開しない。形式、path traversal、symlink、巨大展開、入れ子、暗号化の有無を先に検査し、安全と判断したものだけを `.workspace/tmp/` の隔離先へ展開する。展開物も個別に受け入れ判定する。
6. PDF、Office、画像等を信頼して実行せず、macro、埋め込みファイル、外部 link、偽装形式に注意する。必要なら内容を検証し、関連性、版、権利、重複、既存成果への影響を記録する。
7. ユーザーが入力を削除しても manifest の検出・採用・削除履歴は消さず `missing` として影響を確認する。制作中の追加・更新を見つけても、既存ユーザー入力を移動・削除せず、作業を黙って巻き戻さない。
8. 並行処理では担当 snapshot を分け、同じ manifest や canonical source を競合更新しない。更新の所有者を一つにするか、直列に統合する。
9. `draft/` への公開時に、採用した manifest の snapshot/revision を `STATUS` または変更要約へ記録する。
10. `out/` 昇格直前の再走査で、未評価の安定ファイル、変更 revision、`pending`、新規 `missing`、採用 revision の欠落、読み取り失敗、hash の計算・再検証失敗、または記録済み hash との不一致があれば停止して報告する。例外進行は、影響評価とユーザー合意を manifest に記録した場合だけ認める。

## draft と out

- `draft/` の PDF は途中成果であり final と呼ばない。`out/` への移動・複製は、完了条件を確認した明示的な promotion とする。
- draft PDF は閲覧者や copy の可否にかかわらず WIP artifact とし、変更後も affected work unit を `WIP` または `blocked` に戻して速やかに再生成する。`reviewed` と表示する範囲だけは affected work unit の再 review と変更後 snapshot の quality gate を完了して `draft/STATUS.*` を更新する。外部への送信・公開・配布の可否は draft/out の所在と別に判断する。
- 各制作 cycle で、完了済み work unit と進行中 work unit を含む可読かつ追跡可能な PDF を `draft/` へ公開する。未完成部分は章節等の境界で隔離し、本文と `draft/STATUS.*` の双方で `reviewed` と WIP、その範囲を明示する。WIP を無表示で完成済み本文へ混在させない。
- WIP preview は学習と早期フィードバックのための必須 live view である。build が成功する限り、raw inventory、coverage、mapping、claim audit、independent review、索引、権利・配布判定の未完了を `STATUS` に `pending`、`unknown`、`WIP` または `blocked` として記録して PDF を出す。これらの全件完了は該当範囲を `reviewed` と表示する条件または `out/` promotion 条件であって、WIP PDF を初めて生成する条件ではない。
- 公開状態を `draft/STATUS.*` 等へ記録し、少なくとも build 日時、成功・失敗・陳腐化、source revision または commit、採用した inbox source snapshot、work unit ごとの `reviewed` / WIP 状態、通過済み・未通過の quality gate、残件、対応する PDF 名を含める。
- `draft/STATUS.*` には再構成ハーネスの最新 snapshot と、Candidate Source の検出・採用・保留・合意除外・阻害、Source Unit、coverage、依存、claim、演習に関する完全性会計を含める。未処理または未検証の件数がある draft は `unreviewed WIP` とし、0 件でない項目と影響範囲を明示する。
- `draft/STATUS.*` では `raw_inventory_review`、`coverage_gate`、`mapping_edge_semantic_review`、`lecture_note_identity_gate` を別項目とし、それぞれの対象 source snapshot、canonical source revision、coverage matrix revision、reviewer、合否、全件/未検証件数、stale の有無、open finding/ID 一覧への参照を示す。百分率または「ほぼ完了」だけを表示せず、いずれか一つでも未通過または stale の draft を完全、reviewed、final と表示しない。WIP ではどの gate が開いているかと影響範囲を明示する。
- `draft/STATUS.*` には `claim_audit_gate` を独立項目として追加し、`claim-audit-report.json` の canonical source snapshot、gate 種別、container 総数、各分類件数、抽出件数、reviewer、合否、stale の有無を記録する。`pending`、`formal-assertion-embedded`、未検証分類、未解決 error が一つでもある場合は open ID と影響範囲を示し、他の数学 review や build 成功で相殺しない。
- 新しい build を開始した時、inbox revision や canonical source が変わった時、または build が失敗した時は、古い成功 PDF を最新版と誤認しないよう `STATUS` を先に `building`、`failed` または `stale` に更新する。
- `out/` の既存成果を直接上書きする前に、対象名、版、検証結果、承認を確認する。可能なら成果物と共に build 日時、source revision/commit、採用 inbox snapshot、検証状態を記録する。
- `out/` に WIP、未完表示、未通過 gate のある work unit を含めない。promotion 対象 snapshot に含まれる全 work unit が完成し、必要な review と gate を通過していることを確認する。
- build 失敗時は `out/` を変更しない。`draft/` と `out/` の公開物を clean 対象にしない。

## ツールチェーンと latexmk 契約

- 既存案件では合意済みの toolchain を優先し、engine や toolchain を無断で変更しない。合意前に大量の template や依存を追加しない。
- 長編日本語数学書の組版は、別要件またはユーザーの明示合意がなければ親 `AGENTS.md` の `bxjsbook` / upLaTeX + dvipdfmx の house-style 基準に従う。参照成果物が指定された場合は同基準の比較項目を一組として検証し、内容固有 macro を移植せず、actual-size と fit-width の render を確認する。
- upLaTeX 文書で日本語索引を作る場合は、原則として upmendex を用いる。別の索引処理系は、案件要件が必要とする場合、またはユーザーが明示的に決定した場合に限り採用する。
- LaTeX/latexmk を採用する案件では、local latexmk 4.83 の `-outdir`、`-auxdir`、`-emulate-aux-dir` 相当の分離機能を利用できる。`out_dir` 相当を `.workspace/build/out`、`aux_dir` 相当を `.workspace/build/aux` とする。
- latexmk が生成する PDF、DVI、SyncTeX、log、aux、fls、fdb 等は、まず全て `.workspace/build/` 配下へ置く。`draft/` や `out/` を latexmk の直接 outdir にしない。
- latexmk の終了 code 成功、生成 PDF の存在と今回の更新、log、未解決参照、引用、font、page を確認する。その後 PDF だけを `draft/` 内の一時名へ copy し、同一 filesystem 上の rename 等で原子的に公開する。`STATUS` は公開 PDF と同じ検証結果を指すよう更新する。
- 失敗時は古い PDF を最新とみなさず `STATUS` を `failed` または `stale` にし、`out/` は不変とする。完了条件を満たした draft PDF だけを明示的に `out/` へ promotion する。
- clean は `.workspace/build/` 等の再生成領域に限定する。相対 path、`-cd`、BibTeX、MakeIndex、glossary、画像参照は案件の実文書で検証する。
- 実文書がなく未検証なら latexmkrc や build script を先回りして作らない。案件開始時に採用方式を確定し、`.workspace/tools/` に案件固有の最小設定を作って実 build で検証する。汎用化できる改善は root shared system へ upstream する。

## 標準ワークフロー

1. 対象 project の `inbox/` を受け入れプロトコルどおり走査し、安定検出した全 file/revision に Candidate Source ID と状態を与える。全候補を既定の対象資料として、版、収録範囲、想定読者、self-contained scope、公開範囲、納期、既存 toolchain を確認する。短い依頼から安全に確定できない事項は provisional と明記し、権利、scope、資料除外、読者水準等の結果を左右する事項だけをユーザーへ確認する。
2. 資料ごとに一意な Source ID を付け、書誌・版・権利・参照方式を rights ledger に登録する。版違いは別 revision/record とし、制作に使う安定 snapshot を固定する。
3. prose authoring を始める前に、採用 snapshot の全 page、または page を持たない形式の全 logical unit、rendered region、time range を走査して `content-inventory` を作り、全 Source Unit の境界、種別、原位置、包含関係を原 render または原データと照合する。未処理範囲、未分類領域、判読不能領域、未登録図表・脚注・演習を 0 件にするまで inventory phase を完了扱いにしない。
4. 全 Source Unit から provisional concept index、glossary、symbol registry、claim ledger、dependency graph、exercise ledger を作る。複数資料間の重複、包含、一般化、矛盾、記法差、証明差を分類し、解消方針を translation decisions に記録する。
5. concept index と dependency graph に基づき、source の目次や見出しを雛形にせず `reconstruction-plan` を作る。章節ごとに中心的な問い、目的、前提、到達点、最小読解経路、説明戦略を先に定めてから Source Unit 群、統合・分割・順序の理由を割り当てる。成果物冒頭と各章節の学習目標に Objective ID を与えて `learning-objective-map` を作り、各目標を具体的な Destination Unit、必要な前提、最小読解経路へ対応付ける。
6. 成果物側の concept cluster、定義と結果の cluster、一続きの導出等を追跡可能な work unit とする。各 unit に source から独立した問い、教育的役割、入力、到達点、説明の流れを持つ authoring brief を作り、原書 chapter、source batch、連続 page をそのまま逐次翻訳する単位にはしない。各 unit の source snapshot、依存先、coverage 範囲、review 状態を記録する。
7. 各 unit は authoring brief と concept/dependency records から講義ノート voice で執筆し、その後で原資料へ戻って意味、論理関係、条件、否定、数量、modality、数式、proof idea、例の役割を照合する。定義前使用と正当化されない循環依存をなくし、動機、見通し、定義、概念説明、主張、完全な証明・導出、例・反例、比較、応用、振り返りを教育的に接続する。正当な相互・帰納・余帰納定義は一つの明示した dependency node として扱い、不確かな箇所は推測で確定せず検索可能な未確定表示と根拠を残す。
8. Source Unit を本文へ統合した時点で coverage matrix の forward mapping を、本文へ要素を追加した時点で reverse mapping と由来種別を更新する。配列変更、統合、分割、重複整理、行間補足を翻訳と区別し、採用元、意味保存の根拠、追加理由を記録する。
9. 原資料中の全演習を実際に解き、問題文、依存、解答、source issue、本文統合先を exercise ledger に記録する。最終成果では未解答の exercise environment にせず、数学的内容と完全解答を本文、例、導出、命題・証明等へ統合する。
10. checkpoint ごとに完全性会計、双方向 coverage audit、definition-before-use、claim/proof 対応、依存、用語・記号、原文対応、図表要素、演習統合に加え、lecture-note identity を検査する。source を見ない読解と source comparison の両方を行い、逐次訳・一対一 paragraph 対応・source narrator の残存を探す。細かな編集は checkpoint まで batch し、semantic、structural、editorial の影響範囲は親 `AGENTS.md` の分類に従う。
   canonical TeX source の作成または変更後は `../.system/book-claim-harness scan <project-root>` を実行し、`claim-audit.json` の全 current container を数学的役割で分類する。cue の有無だけで formal 性を決めず、単一対象、短い主張、局所的な定義確認、後で再利用しない主張も独立した真偽判定と論証を持つなら `formal-assertions-extracted` とする。抽出先の statement、bare proof、Source Unit と mapping edge、元 example 等の anchor と双方向参照を記録し、原資料より強い一般化をしていないことを照合する。分類担当と authoring 担当から独立した reviewer の全件確認後に `../.system/book-claim-harness check <project-root> --gate checkpoint` を通し、その source snapshot と receipt を checkpoint record と `draft/STATUS.*` に反映する。`scan` の候補抽出、件数、cue 不在、agent の自己申告は checkpoint gate の代用にしない。
11. 数学を含む書籍では、各 affected work unit の完成時と semantic change 後の checkpoint で `../MATH_PROSE_REVIEW.md` に従う independent review phase を実施し、private records に結果を保存する。raw inventory、coverage matrix、mapping edge の意味保存を独立 review する担当は、authoring だけでなく、対象 inventory/mapping の作成・編集、semantic verification flag の付与、source-analysis 上の採否・同一視・統合判断を行った担当からも分ける。reviewer は数学だけでなく、該当 Source Unit と再構成先の双方向照合、統合・分割による意味の保存、行間補完、演習統合、および同規範 phase 1b の成果物 identity を確認する。`reviewed` と表示する draft 全体または `out/` 候補について、成果物全体と採用 source snapshot の独立した全件 coverage・lecture-note identity review も行う。artifact 作成者による自己検査は WIP の一次確認には使えるが、independent review flag または promotion gate の証拠にしない。
12. WIP 制作中は変更箇所を incremental build し、checkpoint で受け入れ snapshot とハーネス snapshot を再確認して必要範囲の build と render を検証する。`reviewed` と表示する draft または `out/` 候補では log、全 page、全図表、目次、索引と内部リンクを検証し、work unit の review 状態と WIP 範囲を明示した readable/traceable な PDF を公開する。
13. 成果物の生成・保存に必要な権利・license、不要物混入、直前の inbox 状態、全 work unit の gate、完全性会計の全 0 件条件、全体 coverage review、lecture-note identity review、対象 snapshot に対する schema validator 成功、WIP 0 件を確認し、承認後にだけ `out/` へ昇格する。外部送信・公開・配布の条件は、その操作を実際に行う時点で別に確認する。
   promotion 対象と同一の canonical source snapshot に対して `../.system/book-claim-harness check <project-root> --gate promotion` を再実行し、全 container の `pending` と `formal-assertion-embedded`、未検証の否定分類、未記録の cue waiver、抽出先でない statement、proof 欠落、原 container 内に残る statement、provenance/mapping 欠落、意味を強めた一般化、片方向だけの example 相互参照、stale scan を 0 件にする。成功した `claim-audit-report.json` の snapshot と promotion candidate を一致させる。

## 翻訳・再構成・出典規約

- 翻訳、要約、再構成、直接引用、訳注、編者補足を識別可能にする。原文にない説明、例、接続、評価、推論は合意した表示で明示し、原著者の主張に見せない。
- 翻訳は source analysis の手段であり、完成本文の既定形式ではない。正確な逐語寄りの working translation が必要な場合も private record または中間資料として扱い、それを paragraph 単位で磨いて最終本文にしない。最終本文は reconstruction plan と authoring brief から講義ノートとして書き、working translation との対応は coverage matrix で保持する。
- 再構成は原書の章節順、説明順、演習配置を保存する作業ではなく、内容と出典対応を保存して教育的・依存的に最適な順序へ組み直す作業とする。順序変更で文脈、指示対象、仮定、modality、適用範囲が失われないよう、必要な接続と再導入を明示する。
- 成果物全体と各章節について、想定読者、既知としてよい前提、本文内で定義・証明する範囲、許容する外部依存を読者向けに明示し、self-contained 性の境界を検証可能にする。合意した境界の内側では definition-before-use を定義文内部の技術語へ再帰的に適用し、定義依存 graph の推移閉包に未定義 node を残さない。例えば「弧状連結とは道でつながっていることである」とするなら、数学的な「道」を先に定義するか同じ箇所で定義し、日常語風の言い換えを定義の代用にしない。主張と証明の依存、記号、非自明な推論を本文または正確な参照で追跡可能にし、「前に述べた」等の曖昧な案内で済ませない。章節を途中から読む場合に必要な前提、定義、先行結果には、本文内の正確な名称とクリック可能なリンクを付ける。
- self-contained scope 内で外部結果を使う場合、名称・出典・正確な statement と適用条件だけでなく、本文または付録へ完全な proof を追加する。「外部結果である」という断りだけで proof としない。原資料にない proof は訳注・編者補足として由来と導入理由を記録し、数学的内容を独立 review する。proof を収録できない結果は、ユーザーが既知の外部依存として scope から除くことに明示合意した場合だけ許容し、読者向け前提一覧、dependency graph、translation decisions に結果と影響範囲を記録する。
- 新出専門用語は、定義または正式導入の初出で採用訳と原語を併記する。独立に読める章節で再導入する場合は原語を再掲するか原語付き定義へリンクし、glossary、正規索引項目、本文の first use を一致させる。
- Source Unit を短い要約へ置き換えただけで covered と判定しない。定義の全条件、主張の全仮定と結論、証明の各実質的論法、説明の因果、例・反例の役割、図表の semantics が再構成先に保存されていることを要素単位で確認する。複数箇所への分散と複数資料からの統合は coverage matrix で明示する。
- 直接引用は原文と照合して正確性を保ち、脱落・省略、原文にない強調、訳文の併記を明示する。引用と翻訳の表示・照合結果を source map または translation decisions に残す。
- 欠落・判読不能箇所を推測で埋めない。原資料間の矛盾は安易に解消せず、版、定義、文脈を確認して両論と編集判断を記録する。
- source analysis 用の working translation では語順や構文の逐語的対応より、原文の意味、論理、modality、著者の声を正確に把握する。最終の再構成本文は、その分析を根拠として意味、条件、modality、出典上の帰属を保存しつつ、講義ノート自身の voice と教育構造で書く。原文の構文、paragraph、章節順、接続、継続的な著者 voice を fidelity の名の下に持ち込まない。文脈上省略された主語・指示対象・接続関係や、原文から妥当に復元できる非自明な中間推論は、読者が追える自然な日本語として明示し、実質的な補足は訳注または translation decisions で追跡する。
- 原文に成立しない論証の gap、欠落、または誤りがあると判断した場合は、訳文で黙って補完・訂正したり、もっともらしい内容を創作したりしない。source issue として原文どおりの内容、疑義、影響、確認根拠、採用した扱いを translation decisions に記録し、本文では原文の問題と訳者注を識別可能に示す。訂正候補や補足を載せる場合も訳者によるものと明示する。`reviewed` と表示する draft の範囲に、無表示の gap や未記録の補完を残さない。
- 意味を変える意訳、説明追加、節移動は追跡可能にする。AI 生成の補足、候補訳、要約は、原文・出典との照合なしに確定本文へ入れない。
- 引用・翻訳・図表には可能な限り Source ID、章節、版、page または安定位置を付ける。統合先から原資料へ、原資料の使用範囲から統合先へ双方向に追跡できるようにする。
- 定義、列挙、比較、条件、例外、否定、数量、専門用語、引用、数値、数式を重点照合する。訳語変更時は既存箇所を検索し、文脈依存の例外には理由を残す。
- 「明らか」「容易」「標準的」「同様」等を理由に、原資料または再構成上必要な非自明な推論を削らない。原資料が省略した中間推論も、合意した前提から妥当に復元できる場合は具体的に展開し、復元不能または不成立なら source issue として扱う。
- 文体、句読点、全半角、固有名詞、原語併記、敬体・常体、数字・単位、脚注形式を glossary または style 台帳で統一する。要約・簡略化で条件、例外、論理関係を落として誤解を招かないよう translation decisions と照合する。
- 翻訳対象として採用するのは利用権限を確認できる資料だけとする。直接引用は必要最小限とし、rights ledger の URL、参照日、license・許諾、翻訳権、翻案・編集、図版、公開・配布条件を確認する。個人情報、秘密情報、購入資格情報、契約制限素材、DRM 回避物を commit・公開せず、権利未確認素材を翻訳済み本文や公開物へ入れない。verified private mode では、保存先への複製自体が禁止されていない stable な inbox input を、権利確認前にも `pending` と明示して recovery snapshot へ含めてよいが、採用・翻訳・配布の許可とは扱わない。

## 数式・図表・PDF 品質

- 原文の記号、添字、上付き、演算子、式番号、定義域、量化範囲を照合する。複数書籍の記号を統一する場合は原記号、統一記号、範囲、理由を記録し、原文引用は勝手に改変しない。
- 数学上の formal claim は、内容に合う theorem、lemma、proposition、corollary 等の番号付き semantic environment に置き、自動生成番号と一意で安定した label を付ける。正式な主張を通常の prose だけで済ませない。
- 原資料で example と表示された単位も、その表示を機械的に保存せず、内部の各文を数学的役割で分類する。特に「○○は連続である。実際，……」のように、特定の対象について性質・関係・存在・一意性・等式・不等式等を真として assertion し、その根拠を続ける部分は、一般化できるか、後で再利用するか、証明が長いかを問わず formal claim と proof として扱う。内容に合う proposition、lemma、theorem、corollary 等の番号付き environment と直後の proof environment へ抽出し、元の具体的設定、計算、解釈、適用は example として残して相互参照する。
- 単一の対象が定義を満たすことの局所的な確認も、それ自体を独立した assertion として掲げて論証するなら前項に従う。純粋な値の列挙、別個の assertion を掲げない一続きの計算、例示そのものだけが目的の記述、反例・非例の対象導入は機械的に定理化しないが、内部に真偽を持つ assertion と justification があれば必ず抽出する。この分割は Source Unit から複数の Destination Unit への mapping として記録し、原資料の仮定、一般性、出典上の example という位置付け、教育的役割を保存する。抽出によって原資料より強い主張または根拠のない一般化を導入しない。
- 前二項は文面上の注意だけで完了としない。canonical source の全変更後に `book-claim-harness scan` が列挙した example、remark、caption、footnote、通常 prose を、各 container の全体を読んで `claim-audit.json` に全件分類する。`実際`、`なぜなら` 等は検出 cue であって十分条件でも必要条件でもない。cue がない、主張が短い、対象が一つだけ、証明が一計算で終わる、source の見出しが Example である、再利用予定がない、という理由だけで `no-formal-assertion` にしてはならない。否定分類と抽出記録は独立 reviewer が原 Source Unit、最終 container、statement、proof、coverage edge を直接照合し、checkpoint と promotion の machine gate を通す。
- 直前の theorem-like statement を証明する通常の proof は、見出しを原則として「証明」とする bare な無番号 proof environment に置き、proof 番号、proof 自体の label、証明対象への明示的な cross-reference を付けない。statement から離れた証明・解答には証明対象を label で示す無番号の説明見出しを用いてよい。番号付き proof は、複数の証明・別証明を区別する場合、または proof 自体を他所から参照する場合に限り、一意で安定した label と証明対象への cross-reference を持たせる。必要な statement、番号付き proof、definition、equation 間の参照は、手入力番号ではなく label による cross-reference とする。
- proof に非自明な gap を残さない。十分性は任意の語数や page 数ではなく、主張に必要な依存関係と推論が明示され、読者が各段階を追跡できる長さと内容を備えるかで判定する。原文由来の gap は創作して埋めず、source issue と訳者注として扱う。
- proof は長さではなく論理的 landmark と読者の navigation によって番号付き step へ分ける。複数の構成、場合分け、補助主張、長い計算に限らず、短い proof でも構成と検証、二方向の含意、置換と結論、補助観察と適用等を区別すると明瞭になるなら step 化してよい。各 step に「局所目標」「使用する仮定・先行結果」「得られる結論」が分かる短い説明見出しを付け、後続 step は使用する直前の step または結果を明示する。一つの原子的推論を見かけだけの step に細分しないが、短いことだけを未分割の理由にしない。
- 翻訳・収録範囲に含まれる演習問題は、原著に解答が掲載されているか否かを問わず、すべて解く。これは proof と本文に非自明な gap を残さず、読者が必要な推論を追跡できるようにする規則の一部である。解答は問題ごとに対応を明示し、必要な中間推論を省略しない。原問が不成立、不足条件を含む、または解答不能であると判断した場合も未解答のまま残さず、その事実と根拠を示し、原文の問題と訳者・編者による分析または補足を識別可能にして translation decisions に記録する。完成講義ノートでは演習として残さず、問題が担う全内容と解答を意味に合う本文、例、導出、定理・証明へ統合する。
- 再構成後の各 formal claim、proof、definition、equation、example、figure、table、footnote と実質的な説明について、coverage matrix の reverse mapping または明示した編者補足種別を確認する。収録対象の全 Source Unit について forward mapping と意味保存を、合意除外した Source Unit について除外判断と影響評価を確認し、孤立した成果物要素、未統合 Source Unit、broken mapping を 0 件にする。
- 数学を含む場合、work unit 完成時と semantic change 後の checkpoint で `../MATH_PROSE_REVIEW.md` の該当 phase と unit gate を affected scope に適用する。structural change と editorial change は親 `AGENTS.md` の分類に従い、変更のない unit の review を機械的にやり直さない。必要な review は原則として authoring 担当とは別の read-only phase とし、記録を private records に保存する。さらに promotion 対象全体に同文書の全 phase と promotion 条件を適用し、blocking finding の解消または scope 除外へのユーザー合意を記録して open blocking を 0 件にするまで `out/` へ promotion しない。
- 図表ごとに出典、版、元番号、page、権利、引用・翻訳・改変・新規作成の別を図表台帳へ記録する。改変範囲を明示し、番号、caption、凡例、単位、本文参照、白黒・縮小時の可読性、accessibility、画像解像度を確認する。
- build 成功だけで完成としない。self-contained な書籍には、明示的な scope 除外がない限り、収録章節を案内する目次と巻末の用語索引を設ける。error、warning、未解決参照、重複 label、欠落引用、式番号、図表位置に加え、目次と索引の生成成功、採用した目次処理系の artifact（LaTeX なら `.toc`）と索引処理系の入力・出力・ログ（makeidx/upmendex なら `idx`/`ind`/`ilg`）の存在と非空性、索引処理の warning/error、索引の目次掲載、ページ参照、PDF bookmark を確認する。
- PDF 内の目次、学習目標、相互参照、引用、脚注、URL、索引の page 参照、`see`、`seealso` その他のクリック可能なリンクは、リンク文字列自体を一貫して青字かつ下線付きで表示する。色だけに依存せず印刷時にもリンク範囲を識別できるようにし、通常本文との十分な contrast、改行時の下線、数式・日本語・欧文を含むリンク範囲、PDF bookmark との対応を実際の render で確認する。
- PDF の索引には内部リンクを付ける。各ページ参照は対応する本文ページへ移動できるようにし、`see` および採用時の `seealso` 参照も対象の索引項目または合意した適切な移動先へリンクする。生成された全リンクについて、リンク切れ、誤った移動先、リンク範囲の欠落がないことを実際の PDF で確認する。
- 日本語・欧文・数式 font、埋め込み、代替、文字化け、禁則、行末、脚注、余白、見開き、空白 page、page 番号、はみ出しを確認する。最終候補は全 page を目視または rendering 検査する。
- 検証記録には実施日、対象 source revision、採用 inbox snapshot、引用・文体・権利・図表を含む実施項目、結果、残件を残す。Git 差分に原資料、中間物、秘密情報、権利不明素材、無関係な変更がないことを確認し、台帳または QA に未解決の不備があれば `out/` へ promotion しない。

## 禁止事項

- 権利未確認の書籍を翻訳対象または公開物へ加えること。public Git へは加えず、verified private mode の recovery snapshot に保存する場合も、保存先への複製自体が禁止されていない stable な inbox input を未採用の `pending` として記録する場合に限る。
- 原書を先頭から chapter 順に翻訳して連結しただけのものを、再構成済み講義ノートとして扱うこと。
- 原文の連続 passage を逐次翻訳し、後から見出し、順序、接続語、用語だけを変更したものを再構成済み講義ノートとして扱うこと。
- 原 chapter・section・paragraph・example・exercise の境界と順序を、数学的・教育的理由の記録なしに Destination Unit へ一対一で移すこと、または source ごとの block を順に接続して一冊にすること。
- 直接引用、歴史的帰属、source 間の相違等を除き、原著者を完成本文の継続的 narrator とし、source 固有の「次に見る」「前章で述べた」等を講義ノートの接続として残すこと。
- `inbox/` で検出した file/revision を Candidate Source として登録せず、またはユーザー合意なしに不採用・無関係・重複として完全性会計から除くこと。
- 採用資料の全 page、または page を持たない形式の全 logical unit、rendered region、time range に対する Source Unit inventory、provisional concept index、dependency graph、coverage matrix を作る前に、完成本文の大量 authoring を始めること。
- 原目次、原索引、OCR、text 抽出、検索 hit、LLM の記憶のいずれか一つだけを根拠に、原資料の全内容を把握したとみなすこと。
- Source Unit を対応先なしに省略し、または仮定、一般性、証明法、例の役割、図表 semantics を失う短い要約だけで covered とすること。
- 再構成性、読みやすさ、簡潔さ、紙幅、学習者への負担、重複整理を、content-bearing な Source Unit の省略、弱化、未表示の一般化、資料間の差の消去の根拠にすること。
- 多数の Source Unit を一つの Destination Unit へ結び、各 mapping edge の意味保存を照合せずに covered とすること、または抜取検査、coverage 率、総数等式だけで完全被覆を認定すること。
- 登録済み Source Unit の集合だけを完全性 review の母集団とし、採用 raw source snapshot の全 page/logical range/rendered region/time range から独立に inventory を再構成・照合しないこと、または異なる実質要素を catch-all Source Unit へ束ねて未登録領域を隠すこと。
- 出典不明の記述、根拠のない一般化、未証明の formal claim、目的不明の編者補足を加えること。
- 原資料の演習を未解答で残すこと、演習内容を削ること、または完成講義ノートに読者へ解答を委ねる演習・問題・課題を置くこと。
- `pending`、未処理範囲、未統合 Source Unit、未追跡依存、未解答・未統合演習、dangling ID、broken mapping、stale snapshot、open blocking のいずれかが残る snapshot、または schema validator 未成功の snapshot を完全、reviewed、final と表示すること。
- `book-claim-harness` の current scan に `pending` または `formal-assertion-embedded` がある、否定分類・cue waiver・抽出記録の独立 review がない、抽出先の番号付き statement と直後の bare proof がない、元 example 等との相互参照または Source Unit/mapping edge がない、あるいは claim audit snapshot が canonical source と一致しない状態を checkpoint 合格、reviewed、final と表示すること。
- 翻訳、要約、再構成、補足を無表示で混ぜ、原著者の主張に見せること。
- 出典対応、版、page、訳注、矛盾、原文の誤り、版変更を記録せず統合すること。
- inbox の入力をエージェント都合で変更し、不安定・未評価の入力を採用すること。
- project 間で source、records、tool state、build、cache、lock を共有すること。
- 合意なく toolchain や engine を固定・置換すること。
- build 成功だけを根拠に PDF を完成扱いし、draft を無検証で out に置くこと。

## 完了条件

- 同一の source snapshot、canonical source revision、coverage matrix revision に対して、完全被覆 gate と lecture-note identity gate が独立に合格している。一方の合格を他方の代用にせず、いずれかの review 後に source、scope、本文、構成、mapping が変わっておらず、stale な coverage/identity flag と未再検証の dependent unit が 0 件である。
- 表題、序文、目次、章節、本文 voice、まとめが、翻訳本または source 別合本ではなく、原書群を資料として用いた独立の再構成講義ノートとして一貫している。各章節に固有の中心的な問い、学習目標、前提、到達点、最小読解経路、説明戦略があり、source を知らない読者がその目的と論理的接続を理解できる。
- `lecture-note-identity-review` が source を見ない読解と source comparison の両方で完了し、数学的・教育的必然性を記録した一致を除いて、連続 passage の逐次訳、原見出し・paragraph・接続の一対一対応、source ごとの未統合 block、原著者の継続的 narrator が 0 件である。語彙の言い換えだけではなく、concept cluster と dependency に基づく統合・分割・再配置、および成果物固有の説明が確認されている。
- 合意範囲が、原書順の連結ではなく概念・依存・教育上の導線に基づいて再構成された単一 PDF に収録され、構成、本文、scope 内の目次・巻末用語索引、相互参照が整合している。目次または索引を scope から除外する場合は、対象、理由、読者と quality gate への影響、代替する案内手段、ユーザーの明示合意を private record に記録し、残る成果物について gate を再実行する。
- `inbox/` で安定検出した全 Candidate Source revision が source-set ledger にあり、`detected = adopted + exact-duplicate-linked + superseded-by-agreement + excluded-by-agreement + pending + blocked` が成立し、未分類、`pending`、合意のない supersede/除外、`blocked`、manifest 不一致が 0 件である。exact duplicate は同一 hash と canonical target を持ち、supersede/除外は対象、理由、影響、ユーザー合意を持ち、採用した全 revision が固定 snapshot と一致する。
- 採用 source snapshot の全 page または page を持たない形式の全 logical unit、rendered region、time range を照合した `content-inventory` が存在し、独立した実質要素が leaf Source Unit に分解されている。未処理範囲、未分類 content-bearing span/region/range、意図しない重複、未解決 child、位置不明 Source Unit、未登録の本文要素・脚注・図表・caption・演習が 0 件である。
- inventory 作成と authoring から独立した reviewer が、登録済み Source Unit から逆算せず採用 raw source snapshot の全 page/logical range/rendered region/time range を直接再走査し、content-bearing な全領域、leaf 粒度、包含、重複を inventory と全件照合した `raw-inventory-review` が同一 snapshot に対して有効である。未登録領域、catch-all unit、誤った粒度、判読不能・位置不明の未解決箇所が 0 件である。
- 収録対象の全 Source Unit と成果物の全 leaf Destination Unit について双方向 coverage mapping があり、成果物の全 content-bearing span/region が Destination Unit に属している。destination の未処理・未分類領域、意図しない重複、`pending`、統合先不明、出典・補足種別不明、broken mapping が 0 件である。`excluded-by-agreement` の Source Unit は本文 mapping の代わりに対象、理由、影響、ユーザー合意への有効な参照を持ち、残る scope の gate が再実行されている。
- 全章節を Source ID、版、原章節、page または形式に応じた安定位置まで追跡でき、翻訳・要約・再構成・補足の区別が明瞭である。
- final concept index の全 concept が本文 destination、glossary/正規索引項目、Source Unit、dependency graph と整合し、未配置 concept と正式用語の索引漏れが 0 件である。
- 成果物全体と各章節の self-contained scope、想定読者、既知としてよい前提、許容する外部依存が読者向けに明示され、境界内の definition-before-use、主張・証明・記号・非自明な推論を追跡できる。途中の章節から読む学習者に必要な前提と先行結果が正確な本文リンクを持ち、曖昧または到達不能な案内が 0 件である。
- すべての Objective ID が少なくとも一つの実在する Destination Unit と、その理解に必要な前提・最小読解経路へ対応し、対応先本文からも Objective ID を逆引きできる。未対応目標、本文と一致しない目標、到達不能・broken link、stale revision が 0 件であり、学習目標から該当本文と前提へ PDF 内で直接移動できる。
- anchor/link inventory に、work unit の入口から必要な前提・定義・先行結果、新出用語から原語付き定義と正規索引項目、theorem-like statement から対応 proof・使用する先行結果・主要な利用箇所、学習目標から対応本文と最小読解経路、および本文から学習目標への必須 edge が揃っている。目次、学習目標、本文内参照、索引を入口とする link graph で全 content-bearing node が到達可能であり、必要 edge の欠落、broken link、誤った移動先、到達不能 node が 0 件である。
- definition-before-use、claim/proof 対応、dependency graph と本文が一致し、定義文内部を含む未定義語、正当化されない循環・未宣言依存、未証明の要証明 claim、行間として残る unjustified step が 0 件である。定義依存の推移閉包が想定読者の既知前提まで閉じ、新出専門語の初出原語、glossary、索引が整合している。正当な相互・帰納・余帰納定義は一つの明示した node として well-definedness または基礎付けが検証されている。
- example、remark、caption、脚注、通常 prose を含む全 content-bearing container の assertion inventory があり、未分類候補と未抽出 formal assertion が 0 件である。例内から抽出した statement/proof と元の例が相互参照され、外部結果は本文内の完全な proof またはユーザー合意済みの scope 除外に対応している。
- promotion 対象と同一の canonical source snapshot に対する `book-claim-harness check <project-root> --gate promotion` の成功 receipt があり、全 current container の分類と独立 review、全抽出の Source Unit/mapping provenance、instance-specific/general の保存、番号付き statement、直後の bare proof、元 example/remark の anchor と双方向参照が検証済みである。`pending`、`formal-assertion-embedded`、未承認 cue waiver、stale container、抽出先に残った埋込み statement、根拠のない一般化、片方向参照が 0 件である。
- step 化によって dependency trace が明瞭になる proof は、論理構造に応じた番号付き step と説明見出しを持ち、各 step の局所目標、入力、結論および後続 step との依存を追跡できる。proof の長短だけを理由にした有用な未分割と、論理的 landmark のない過剰分割が 0 件である。
- 原文照合、用語統一、重複・矛盾・原文の誤りの処理、引用・権利確認が完了し、`blocking-open` source issue と数学的意味・coverage・権利に影響する未確定事項が 0 件である。本文の完全性に影響しない non-blocking metadata だけは、影響がない根拠と共に残件として明示できる。
- 合意した手順で再生成でき、warning、参照、数式、font、全 page の layout 検証を通過し、参照成果物がある場合は house style を一組として actual-size と fit-width の両方で比較済みである。
- 数学を含む場合は `../MATH_PROSE_REVIEW.md` の独立 review phase が完了し、open blocking が 0 件である。
- 翻訳・収録範囲に含まれるすべての演習問題に、行間を残さず検証された解答、または問題が不成立・解答不能であることの根拠を伴う検証済み分析が対応し、その全内容が本文へ統合され、`exercise_total = resolved = resolution-verified = content-integrated` が成立している。未解答演習、本文未統合演習、読者へ解答を委ねる exercise/problem/task environment が 0 件である。
- authoring、inventory/mapping の作成・編集、semantic verification flag の付与、source-analysis 上の採否・同一視・統合判断の各担当と分けた reviewer が、採用 source snapshot と完成候補について収録対象の全 leaf Source Unit の forward coverage、成果物の全 leaf Destination Unit の reverse coverage、全 mapping edge の意味保存、合意除外した Source Unit の除外記録・影響評価、統合・分割後の意味保存、行間補完、演習統合、および source を見ない読解と source comparison による lecture-note identity を抜取でなく全件再確認し、blocking finding が 0 件である。
- 採用 source revision の追加・差替え・supersede または scope 変更後に、影響 graph の raw inventory、mapping、semantic review、identity review、数学 review、build/render、draft/promotion receipt が推移的に invalidation・再検証され、stale flag と未再検証 dependent unit が 0 件である。再利用した unaffected unit には raw content、unit boundary、意味、依存が不変である証拠がある。
- 対象 snapshot に対する schema validator が、source/destination の全 content-bearing span/region の leaf unit 被覆、排他的 disposition、disposition 別必須 verification flag、全総数等式、dangling ID 0 件、全 mapping の到達可能性、source・ledger・draft revision の一致を検証して成功し、その version と結果が記録されている。
- PDF の全リンクが青字かつ下線付きで一貫して表示され、目次、学習目標、相互参照、引用、脚注、URL、索引のすべてのリンクが正しい移動先へ到達し、リンク範囲、contrast、改行、数式・日本語・欧文表示を実際の PDF で検証済みである。
- 採用 inbox snapshot、source revision、work unit ごとの review 状態、quality gate、検証結果、承認が記録され、WIP が 0 件の最終成果だけが `out/` に明示的に昇格されている。
- 実際に外部送信・公開・配布する場合は、対象に原資料、中間物、秘密情報、権利不明素材が混在せず、その操作に必要な条件が確認済みである。
