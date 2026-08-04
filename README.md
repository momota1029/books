# Books

翻訳・執筆 project を、共通規約と versioned quality gates で運用する公開 system です。

## clone 後の最短手順

clone 直後と system 更新後に次を実行し、両方が成功するまで制作を開始しません。

```sh
.system/bootstrap && .system/doctor
```

repository mode の既定値は `public` です。
public mode で commit・push できるのは、`.public-files` allowlist に含まれ、privacy review 済みの structure、rules、generic code だけです。

## project の作成と private mode

書籍 project は、local の `book-translations` 直下に直接の子として作成します。

```sh
.system/new-book-project <project-id>
```

案件内容を Git 管理する必要がある場合は、GitHub visibility が `PRIVATE` の remote を指定して private mode に切り替え、専用コマンド経由でだけ stage します。

```sh
.system/repository-mode private <remote>
.system/repository-mode add -- <paths>
```

private mode は mode 切り替えと stage の各 gate で remote と `PRIVATE` visibility を検証し、commit・push 時にも再検証します。
認証、network、URL 解決、visibility 確認に失敗した場合は処理を拒否します。
`inbox/`、`draft/`、`out/`、`.workspace/` や案件 directory は private mode では commit できますが、secret、credential、または権利上 commit できない data は含められません。
private history を public remote へ push してはいけません。
`out/` の配布承認と GitHub repository の visibility は別の判定です。
public mode と private mode の対象や stage 手順を混同しないでください。

## 共通規範

- [運用・公開境界](AGENTS.md)
- [日本語文章規範](JAPANESE_WRITING.md)
- [数学文章レビュー規範](MATH_PROSE_REVIEW.md)

ここに記載した `.system/` scripts は system の最終的な公開 interface です。
clone で再利用できる共通改善は案件情報を除去し、privacy review 後に公開側へ upstream します。
