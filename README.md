# Books

翻訳・執筆 project を、共通規約と versioned quality gates で運用する公開 system です。

## 1. clone、bootstrap、mode 確認

clone 直後と system 更新後は、制作を始める前に次を実行します。

```sh
.system/bootstrap
.system/doctor
.system/repository-mode status
```

`bootstrap` は repository local の Git 設定と hook を準備し、`doctor` は system を検査します。どちらかが失敗した場合は、原因を解消して再実行するまで制作を始めません。

repository mode は clone ごとの local Git config `books.repositoryMode` に記録されます。未設定の clone は `public` として扱われ、`bootstrap` も既定値として `public` を設定します。`status` の通常の出力は `mode=public` または `mode=private` です。private mode では設定済みの `privateRemote` も表示されます。

## 2. public mode

public mode で Git 管理できるのは、共有する structure、rules、generic code など、`.public-files` allowlist に一致し privacy review を通過したファイルだけです。通常の `git add`、`git commit`、`git push` を使えますが、pre-commit hook は index を、pre-push hook は送信対象の commit tree を検査します。

案件名、原資料、原稿、翻訳、ノート、PDF、manifest、ログ、画像、案件固有の設定や tool、および `inbox/`、`draft/`、`out/`、`.workspace/` や直接の book project は、public mode では stage、commit、push できません。

現在の public remote に対して次の private mode 切替が失敗するのは正常です。public repository を private 作業用 remote として承認することはありません。

## 3. private mode の前提

private mode を使う前に、次をすべて満たす必要があります。

- remote が GitHub repository を指している
- GitHub CLI `gh` がインストールされ、対象 host に認証済みである
- remote に設定された push URL が正確に 1 個である
- GitHub API で repository の visibility を `PRIVATE` と確認できる

認証状態は、たとえば次で確認できます。

```sh
gh auth status
git remote get-url --push --all <remote>
```

private repository であっても、credential、secret、token、鍵、および契約・ライセンス・権利上 Git に保存できない data は commit できません。private remote の存在は、資料の取得、保存、翻訳、配布の権限を与えません。

## 4. private mode への切替

```sh
.system/repository-mode private <remote>
.system/repository-mode status
```

切替コマンドは、public mode から切り替える時点の HEAD と index が public allowlist に適合することを確認し、指定 remote の push URL、GitHub 上の repository identity、`PRIVATE` visibility を検証します。成功すると、切替時の HEAD を `books.privateBase`、remote 名を `books.privateRemote`、mode を `books.repositoryMode` として clone の local Git config に記録します。

remote の解決、GitHub CLI、認証、network、応答の解析、repository identity、visibility のいずれかを確認できなければ fail closed で停止し、private mode を有効にしません。

## 5. private data の add、commit、push

```sh
.system/repository-mode add -- <private-path> [more-private-paths...]
git commit -m "..."
git push <remote> <branch>
```

private data の stage には必ず `repository-mode add --` を使います。直接の `git add`、`git add -f`、hook を飛ばす `--no-verify` は使いません。

mode 切替、`add`、pre-commit、pre-push の各 gate は、その都度 online で remote の repository identity と `PRIVATE` visibility を再検証します。pre-push は、実際に指定された remote 名と push URL が local config に記録した private remote と完全に一致することも確認します。network、認証、identity、visibility の確認に失敗した場合、stage、commit、push は停止します。

検証済み private mode では、処理中の文書を `inbox/`、`draft/`、`out/`、`.workspace/`、または直接の book project 内に置いて commit できます。ただし、前節の credential・secret・権利上保存できない data の禁止は変わりません。

## 6. public mode へ戻す

```sh
.system/repository-mode public
.system/repository-mode status
```

切替コマンドは `privateBase..HEAD` の全 commit tree を `.public-files` allowlist と照合し、現在の HEAD と index も検査します。private file を後の commit で削除していても、それを含む過去の commit tree が範囲内にあれば切替を拒否します。

拒否された場合、command は reset や履歴書換えを自動実行しません。private history を public remote に送信しないでください。履歴の修正は影響を確認したうえでユーザーが明示的に決定する操作です。

private 作業から得た generic な system 改善は、clean な public clone、または private history を含まない public baseline の branch で再構成し、private identifier と案件内容を除去して privacy review を通したうえで public 側へ upstream することを推奨します。mode の設定と `privateBase`、`privateRemote` は clone local であり、別 clone へは引き継がれません。

## 7. safeguard と限界

- verifier が受け付ける push URL は `https://<host>/<owner>/<repo>`、`git@<host>:<owner>/<repo>`、`ssh://git@<host>/<owner>/<repo>` の対応形式だけです。末尾の `.git` は使用できます。
- push URL が複数ある remote、GitHub 以外、未知の URL 形式、query・fragment 付き URL は fail closed で拒否します。
- local hook は事故防止の gate であり、意図的な迂回まで絶対に防ぐものではありません。[運用・公開境界](AGENTS.md) は bootstrap、repository-mode、hook の迂回を禁止しています。
- `out/` の配布承認と GitHub repository の visibility は別の判定です。private repository に置けることは、読者や第三者へ配布できることを意味しません。
- 複数案件に有効な規約、prompt、template、script、build・QA tool、test、修正は generic 化し、privacy review 後に公開共有領域へ upstream します。

## 8. 書籍 project の作成

書籍翻訳 project は手作業で構造を複製せず、repository root から次の command で `book-translations/` の直接の子として作成します。

```sh
.system/new-book-project <project-id>
```

配置、権利管理、制作、review、promotion の詳細は、次の文書を参照してください。

- [書籍翻訳・統合 PDF 制作ガイド](book-translations/AGENTS.md)
- [運用・公開境界](AGENTS.md)
- [日本語文章規範](JAPANESE_WRITING.md)
- [数学文章レビュー規範](MATH_PROSE_REVIEW.md)

ここに記載した `.system/` scripts は system の公開 interface です。
