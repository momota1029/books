# Books

翻訳・執筆 project を、共通規約と versioned quality gates で運用する公開 system です。

## 1. clone、bootstrap、mode 確認

[GitHub の clone 手順](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository)に従って公開版を clone し、制作を始める前に bootstrap、doctor、mode 確認を実行します。

```sh
git clone https://github.com/momota1029/books.git
cd books
.system/bootstrap && .system/doctor
.system/repository-mode status
```

`bootstrap` は repository local の Git 設定、hook、mode 別の `.git/info/exclude` を準備し、`doctor` は system を検査します。どちらかが失敗した場合は、原因を解消して再実行するまで制作を始めません。system 更新後にも両方を再実行します。

共通 shell tool と offline test は、GNU/Linux の Bash と macOS 標準の Bash 3.2・BSD userland の双方を対象とします。GNU 固有の `sed -i`、`sha256sum`、`realpath`、`stat -c` や Bash 4 以降だけの構文を前提にしません。evidence harness は Python 3.10 以降を必要とし、図表・PDF の検査には各 harness が確認する外部 decoder も必要です。

repository mode は clone ごとの local Git config `books.repositoryMode` に記録されます。未設定の clone は `public` として扱われ、`bootstrap` も既定値として `public` を設定します。この時点の通常の出力は `mode=public` です。

## 2. public mode

public mode で Git 管理できるのは、共有する structure、rules、generic code など、`.public-files` allowlist に一致し privacy review を通過したファイルだけです。通常の `git add`、`git commit`、`git push` を使えますが、pre-commit hook は index を、pre-push hook は送信対象の commit tree を検査します。

案件名、原資料、原稿、翻訳、ノート、PDF、manifest、ログ、画像、案件固有の設定や tool、および `inbox/`、`draft/`、`out/`、`.workspace/`、直接の book project、`papers/writing/`・`papers/submitted/` 直下の論文 project は、public mode では stage、commit、push できません。

現在の公開 remote に対して private mode への切替を試みると失敗します。public repository を private 作業用 remote として承認することはありません。

## 3. 公開版を自分の private repository で運用する

以下の `YOUR-GITHUB-USER` は自分の GitHub user または organization 名、`YOUR-PRIVATE-REPOSITORY` は新しく作る private repository 名に置き換えます。

### 3.1 空の独立 repository を作る

GitHub 上で `YOUR-GITHUB-USER/YOUR-PRIVATE-REPOSITORY` を `Private` として作ります。[新しい repository の作成手順](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)にある README、`.gitignore`、license の初期化はすべて選ばず、空の repository にしてください。GitHub CLI を使う場合は、次も任意の作成例です。

```sh
gh repo create YOUR-GITHUB-USER/YOUR-PRIVATE-REPOSITORY --private
```

GitHub の Fork ではなく独立 repository を作るのは、公開元の fork network とその visibility 制約から private 作業履歴を切り離すためです。repository の visibility と fork への影響については、[GitHub の visibility の説明](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/managing-repository-settings/setting-repository-visibility)も確認してください。

作成先を README などで初期化すると、clone 済みの公開版とは履歴が分岐し、初回 push が拒否されることがあります。force push や `--allow-unrelated-histories` で回避せず、作成先にすでに内容がある場合はここで停止し、その内容を保持する方法を個別に判断してください。

### 3.2 公開元と非公開先の remote を分離する

clone 時の `origin` を、公開元からの取得専用の `upstream` に改名し、自分の非公開先を新しい `origin` として登録します。[remote の管理方法](https://docs.github.com/en/get-started/git-basics/managing-remote-repositories)も参照してください。

```sh
git remote rename origin upstream
git remote add origin https://github.com/YOUR-GITHUB-USER/YOUR-PRIVATE-REPOSITORY.git
git remote -v
gh auth status
git remote get-url --push --all origin
```

`gh auth status` が対象 GitHub host への認証成功を示すことを確認します。最後のコマンドの出力は、作成した private repository の URL 1 行だけでなければなりません。別の URL や複数行が表示された場合は先へ進みません。

この構成では remote の役割を次のように固定します。

- `upstream`: 公開元 `momota1029/books`。更新の取得にだけ使う
- `origin`: 自分の private repository。private history の push 先に使う

### 3.3 private mode に切り替えて初回 push する

GitHub 上の `Private` visibility と local repository mode は別の状態です。remote を作るだけでは private data の gate は有効にならないため、次の切替を必ず実行します。

```sh
.system/repository-mode private origin
.system/repository-mode status
git push -u origin main
```

`status` では次の設定を確認します。

```text
mode=private
privateRemote=origin
```

切替コマンドは、切替時点の HEAD と index が public allowlist に適合することに加え、GitHub CLI の認証、`origin` の単一の push URL、repository identity、GitHub API 上の `PRIVATE` visibility を検証します。確認できない項目があれば fail closed で停止し、private mode を有効にしません。成功時には切替時の HEAD を `books.privateBase` として clone の local Git config に記録し、public mode 専用の local ignore を外します。

## 4. private data の通常運用

たとえば `book-translations/PROJECT-ID` を stage、commit、push する手順は次のとおりです。

```sh
git add -- book-translations/PROJECT-ID
git diff --cached --name-status
git diff --cached --
git commit -m "Describe the private work"
git push origin main
```

verified private mode では、通常の `git add`、`git commit`、`git push` を使えます。既存手順との互換性のため `.system/repository-mode add -- <paths>` も checked wrapper として残しますが、必須ではありません。ignore を強制的に迂回する `git add -f` と hook を飛ばす `--no-verify` は使いません。いずれの stage 方法も credential、secret、token、鍵や、契約・ライセンス・権利上 Git に保存できない資料を自動判定しないため、commit 前に staged diff の対象と内容を必ず確認します。binary file は通常の diff に内容が表示されないため、別途内容を確認します。private remote の存在は、資料の取得、保存、翻訳、配布の権限を与えません。

mode 切替、pre-commit、pre-push の各 gate は、その都度 online で remote の repository identity と `PRIVATE` visibility を再検証します。checked wrapper を使う場合は stage 時にも検証します。pre-push は、実際の送信先が local config に記録した `origin` と完全に一致することも確認します。network、認証、identity、visibility の確認に失敗した場合、mode 切替、commit、push は停止します。

検証済み private mode では、処理中の文書を `inbox/`、`draft/`、`out/`、`.workspace/`、または直接の book project 内に置いて commit できます。ただし、credential・secret・権利上保存できない data の禁止は変わりません。

## 5. 公開 upstream の更新を取り込む

取り込み前に private 側の作業を commit し、`git status --short` の出力が空になることを確認します。その後、[remote から変更を取得する GitHub の手順](https://docs.github.com/en/get-started/using-git/getting-changes-from-a-remote-repository)に沿って次を実行します。

```sh
git status --short
git fetch upstream
git merge --no-edit upstream/main
.system/bootstrap && .system/doctor
git push origin main
```

merge conflict が発生した場合は、内容を解決した各 path を通常の Git 操作で stage してから merge commit を完了します。

```sh
git add -- RESOLVED-PATH
git diff --cached --name-status
git diff --cached --
git commit
```

更新後の `bootstrap` または `doctor` が失敗した場合は push せず、原因を解消して再実行します。

`AGENTS.md`、文章・レビュー規範、routing 規範が merge で変わると、post-merge hook はその場で通知します。実行中の各 Codex thread はスレッド別の既読世代を保持し、checkpoint、task 切替、長時間処理、進捗・最終報告等の action boundary で `.system/instruction-refresh` により再確認します。変更時は現在適用される規約を読み直してから計画と作業路線を更新するため、ある thread の確認が別 thread の通知を消すことはありません。hook から既存会話へ直接割り込みを注入するものではなく、実行中の command が終わった次の boundary で反映されます。この確認 command も通常はエージェント自身が実行し、ユーザーが操作する必要はありません。ただし、この規則を取り込む前から動いていて checker を一度も読み込んでいない thread だけは自力で polling を開始できないため、一度だけ再読を指示するか新しい thread へ引き継ぎます。

## 6. public mode へ戻す

```sh
.system/repository-mode public
.system/repository-mode status
```

切替コマンドは `privateBase..HEAD` の全 commit tree を `.public-files` allowlist と照合し、現在の HEAD と index も検査します。private file を後の commit で削除していても、それを含む過去の commit tree が範囲内にあれば切替を拒否します。

拒否された場合、command は reset や履歴書換えを自動実行しません。private history を public remote に送信しないでください。履歴の修正は影響を確認したうえでユーザーが明示的に決定する操作です。

private 作業から得た generic な system 改善は、clean な public clone、または private history を含まない public baseline の branch で再構成し、private identifier と案件内容を除去して privacy review を通したうえで public 側へ upstream することを推奨します。mode の設定と `privateBase`、`privateRemote` は clone local であり、別 clone へは引き継がれません。

## 7. safeguard と限界

- GitHub 側で private repository を `Public` に変更すると local gate を通らず履歴が公開されるため、visibility を変更しません。
- private mode の pre-push は設定済みの private remote 以外への push を拒否します。local hook は事故防止の gate であり、`bootstrap`、hook、`repository-mode` の迂回は禁止です。詳細は[運用・公開境界](AGENTS.md)を参照してください。
- private mode から public mode へ戻るときは履歴も検査されます。後から private file を削除しただけでは切替を通過しない場合があります。
- collaborator の権限を後から外しても、その人が取得済みの clone までは回収できません。[personal repository の権限](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/repository-access-and-collaboration/permission-levels-for-a-personal-account-repository)を確認し、共有先は必要最小限にします。
- verifier が受け付ける push URL は `https://<host>/<owner>/<repo>`、`git@<host>:<owner>/<repo>`、`ssh://git@<host>/<owner>/<repo>` の対応形式だけです。末尾の `.git` は使用できます。
- push URL が複数ある remote、GitHub 以外、未知の URL 形式、query・fragment 付き URL は fail closed で拒否します。
- `out/` の配布承認と GitHub repository の visibility は別の判定です。private repository に置けることは、読者や第三者へ配布できることを意味しません。
- 複数案件に有効な規約、prompt、template、script、build・QA tool、test、修正は generic 化し、privacy review 後に公開共有領域へ upstream します。

## 8. 自然言語から制作を始める

ユーザーは `.system/` command や directory 名を覚える必要はありません。たとえば次のように、欲しい成果物を自然言語で依頼できます。

- 「○○について新しい論文を書きたい」
- 「この本を日本語に翻訳したい」
- 「この論文を読み、証明を補った解説ノートを作りたい」
- 「今の日本語論文を英訳したい」

エージェントは[自然言語リクエストの制作ルーティング規範](WORKFLOW_ROUTING.md)に従い、最終成果物、source 種別、既存 project の有無から route を選び、必要な初期化 command を内部で実行します。route を変える曖昧さがある場合だけ、投稿・改訂する論文、忠実な翻訳、永続的な解説ノート、self-contained な数学ノート、会話内だけの読解支援のどれが必要かを確認し、翻訳なら source が書籍、他者論文、自分の論文のどれかも同じ一問で確認します。

以下の command は手動運用と system の再現性のために残す公開 interface であり、通常のユーザー依頼ではエージェントが実行します。

## 9. 書籍 project の作成

書籍翻訳 project は手作業で構造を複製せず、repository root から次の command で `book-translations/` の直接の子として作成します。

```sh
.system/new-book-project <project-id>
```

配置、権利管理、制作、review、promotion の詳細は、次の文書を参照してください。

- [書籍翻訳・統合 PDF 制作ガイド](book-translations/AGENTS.md)
- [運用・公開境界](AGENTS.md)
- [日本語文章規範](JAPANESE_WRITING.md)
- [数学文章レビュー規範](MATH_PROSE_REVIEW.md)

## 10. 論文講義ノート project の作成

既存論文を理解・検証する講義ノートは、手作業で singleton workspace に混在させず、repository root から次の command で `paper-lecture-notes/` の直接の子として作成します。

```sh
.system/new-paper-note-project <project-id>
```

一つの project は一つの講義ノート成果物と lifecycle を表します。複数の原論文を同じ読者・scope の一つの統合 PDF にまとめる場合は同じ project で管理し、独立 PDF や独立した公開判断を持つ案件は別 project に分離します。既存の collection-root 直下の private data は一つの legacy project として保持し、新規案件を追加しません。詳しい project 選択、Paper ID、evidence、review、promotion の条件は、[論文理解・講義ノート PDF 制作ガイド](paper-lecture-notes/AGENTS.md)に従います。

## 11. 論文 project の作成

数学論文の新規 project は、手作業で構造を複製せず、repository root から次の command で `papers/writing/` の直接の子として作成します。

```sh
.system/new-paper-project <paper-id>
```

各 project には `inbox/`、`draft/`、`out/` と、canonical source・台帳・build 領域を分離した `.workspace/` が作られます。証明・例集、日本語論文、英語論文、投稿 bundle の制作、レビュー、投稿後に `papers/submitted/` へ移す条件は、[数学論文執筆・投稿ガイド](papers/AGENTS.md)に従います。

ここに記載した `.system/` scripts は system の公開 interface です。
