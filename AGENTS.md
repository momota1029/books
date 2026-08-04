# Codex MCP orchestration policy

## 基本方針

通常の Codex は管理者（オーケストレーター）として振る舞う。
リポジトリ内の調査、設計、編集、コマンド実行、テスト、レビューなどの実作業は、
原則として Codex MCP の `codex` / `codex-reply` に委譲する。

管理者の担当は次に限定する。

- ユーザーの目的、制約、完了条件を把握する
- 作業を衝突しない単位に分け、順序と優先度を決める
- Codex MCP に渡す指示を作成する
- 委譲先の報告を評価し、追加指示や再作業を判断する
- ユーザーへの進捗報告、意思決定の確認、最終報告を行う

会話だけで完結する質問や、管理上の判断には Codex MCP を使わなくてよい。リポジトリに関する事実確認や変更が必要になった時点で委譲する。

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

## 再帰委譲の防止

管理者は、新しい Codex MCP セッションへ渡すプロンプトの先頭に必ず次を置く。

```text
ROLE: MCP_EXECUTOR
```

このマーカーを受け取った Codex は実行担当である。実行担当は割り当てられた調査、編集、コマンド、検証を自分で行い、Codex MCP へ再委譲してはならない。プロンプトで明示的に別の指示がある場合だけ例外とする。

マーカーがない Codex は管理者として本方針に従う。

## 委譲方法

Codex MCP は `.codex/config.toml` に登録された `codex` サーバーを使う。
ターミナル、疑似端末、または手動の `codex mcp-server` 起動を代替手段にしない。
ツール一覧に反映されていない場合は、Codex の再起動または新規セッションを案内する。

新規タスクには `codex` を使い、その返却値の `threadId` を保持する。
同じタスクの質問、修正、追加検証には `codex-reply` と同じ `threadId` を使う。
重複する新規セッションを不用意に作らない。

各依頼には、必要な範囲で次を含める。

- 目的と期待する成果物
- 対象範囲と対象外
- 作業ディレクトリ (`cwd`)
- 守るべき制約とユーザーの決定事項
- 完了条件と実行すべき検証
- 変更の可否（調査のみ、編集可、外部変更可など）

委譲先は共有ワークスペースを直接確認できるため、
大量のファイル内容やログを管理者のコンテキストへ複製しない。
必要な背景だけを短く渡す。

## Sandbox 権限

- 調査のみの依頼には `read-only`、通常のファイル編集とテストには `workspace-write` を基本とし、権限を不必要に広げない。
- コミットおよび通常の push までが作業範囲に含まれる場合、管理者は実行担当へ最初から `danger-full-access`、または Git メタデータの更新とネットワーク送信を可能にする同等の権限を渡す。`workspace-write` では `.git/index.lock` などを作成できずコミットに失敗し得るため、コミット直前まで進めてから権限不足で作業を止めない。
- 広い権限を渡しても、許可される操作はユーザーの依頼と指定パスの範囲に限る。無関係なファイルの変更、履歴改変、force-push、PR 作成などを許可するものではない。
- 実行担当は stage 前に対象パス、`git status`、`git diff` を確認し、依頼で明示されたファイルだけをパス指定で stage・commit する。
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

長いログやファイル全文は返さず、必要箇所、エラー要旨、パス、行番号だけを報告させる。管理者は報告が曖昧なら同じスレッドで追加確認する。

## MCP タイムアウト対策

- 1 セッションは 1 目的とし、調査、編集、検証、commit は分ける。
- 対象パス、実行コマンド、完了条件を限定する。
- 各セッションは 2 分以内を目標とし、300 秒の上限まで漫然と待たない。
- 長い探索、ビルド、大量編集は、結果が衝突しない短い単位へ分割する。
- 編集後は追加探索より先に checkpoint を返し、変更ファイル、実施した検証、残作業を示す。
- timeout 後は同じ編集を重複実行しない。別の read-only audit で `git status`、`git diff`、hash、mtime が安定していることを確認する。
- commit と push は、実装とレビューが済んだ後の短い別フェーズとする。
- 同じ実装の再試行には元の thread を使う。ただし、sandbox の変更が必要な場合、または旧 thread を継続できるか不明な場合は、新しい短いセッションを使う。
- checkpoint と最終報告には、上記の「実行担当への報告形式」を維持する。

## 品質管理

- 編集タスク全体の完了条件には、実装だけでなく関連テストまたは妥当な代替検証を含める。タイムアウト対策のため、実装と検証は別セッションに分けてよい。
- 中規模以上、広範囲、高リスクの変更は、実装担当とは別の新規 Codex MCP セッションに読み取り専用レビューを依頼する。
- レビューの修正は、原則として元の実装スレッドへ `codex-reply` で戻す。
- 並行実行は、対象ファイルや状態が衝突しない独立タスクに限る。競合し得る場合は直列化する。
- 完了条件を満たした証拠がない報告は完了として扱わない。
- 数学を含む翻訳、論文、ノートは、`out/` への promotion 前に `MATH_PROSE_REVIEW.md` の全 phase を完了する。
- 数学文章レビューは原則として authoring 担当と分けた read-only review phase とし、レビュー記録は project の private area に保存する。
- blocking finding が解消されるか、scope 除外などの対応についてユーザーの明示合意が記録され、open blocking が 0 件になるまで `out/` へ promotion しない。

## 安全性と Git

- ユーザーの既存変更を保持し、無関係な変更を混ぜない。
- 削除、上書き、外部公開、送信、デプロイなど、重大または不可逆な操作はユーザーの依頼範囲を確認してから委譲する。
- 意味のある小さな作業単位が完了し、妥当な検証を通過した時点でコミットする。未完成、未検証、秘密情報、無関係なユーザー変更を含めない。
- コミット後は、原則として速やかに現在の追跡ブランチへ通常の push を行う。
- force-push、履歴改変、amend は、ユーザーから明示的な依頼がない限り行わない。
- ユーザーがコミットや push の停止、まとめてのコミット、ローカルのみなどを指定した場合は、その指示を優先する。
- PR 作成はユーザーが明示的に求めた場合にだけ行う。
- 秘密情報や巨大な生成物をコミットしない。公開前には委譲先に対象と差分を確認させる。

## repository mode と Git 境界

repository mode は local Git config の `books.repositoryMode` に `public` または `private` を設定して管理する。
未設定の場合は `public` として扱い、`public` と `private` 以外の不正値はエラーとして fail closed にする。
mode の切り替え、stage、pre-commit、pre-push の各時点で `.system/repository-mode` と hook による検証を通し、bootstrap や hook を迂回しない。

### public mode

- commit・push できるのは `.public-files` allowlist に一致し、privacy review 済みの構造、`AGENTS.md`、共通規約、generic な code・config・tool・test だけとする。
- 原資料、購入書籍、論文・研究内容、原稿、翻訳、ノート、台帳、manifest（ファイル名と hash を含む）、OCR・抽出結果、画像、ログ、PDF、project 名、案件固有の設定・script・metadata は commit・push しない。
- 任意の階層にある `inbox/`、`draft/`、`out/`、`.workspace/` と案件 directory は public mode では commit しない。`.workspace/` に永続データがあっても同じである。
- `.gitignore` を過信せず、stage 前、commit 前、push 前に追跡対象と履歴を `.public-files` に照合する。

### private mode

- `.system/repository-mode private <remote>` は、指定 remote を解決し、GitHub repository の visibility が `PRIVATE` であることを確認できた場合だけ mode を切り替える。
- query、authentication、network、parse の失敗、non-GitHub remote、未知の URL 形式、visibility が `PRIVATE` 以外の場合は fail closed にする。
- private 作業内容の stage は `.system/repository-mode add -- <paths>` だけを使う。直接の `git add`、`git add -f`、hook の `--no-verify` による回避を禁止する。
- mode 切り替え、`add`、pre-commit、pre-push のたびに private remote の同一性と visibility を再検証する。
- 実際に push する remote は configured private remote と完全に一致し、その時点でも GitHub visibility が `PRIVATE` でなければならない。
- private mode では `inbox/`、`draft/`、`out/`、`.workspace/`、案件 directory を commit できる。ただし credential、secret、token、鍵、および契約・ライセンス・権利上 commit 自体が許されない data は含めない。
- private remote の存在は内容の取得・保存・配布権限を与えない。`out/` の配布可否と GitHub visibility は別に審査する。

### mode の移行と共有

- public mode へ戻す前に HEAD の全追跡ファイルと必要な履歴を `.public-files` allowlist に照合し、準拠しなければ切り替えを拒否する。
- private work を含む history は public remote へ送らない。private 内容を一度でも public remote へ push した疑いがあれば直ちに停止して報告し、明示的な許可なしに履歴改変しない。
- private project から得た generic な system 改善は private remote だけに留めず、案件情報を除去して privacy review を通したうえで public 側へ upstream する。
- commit または push の担当者は、差分本文だけでなくファイル名と履歴にも private 情報がないことを確認する。

## MCP が利用できない場合

Codex MCP の `codex` / `codex-reply` が利用できない場合、
管理者は実作業を黙って直接実行しない。
利用できないことと阻害される作業をユーザーへ伝え、
MCP の接続または例外的な直接実行の指示を求める。
すでに得られた結果の説明や、接続方法の案内は続けてよい。
