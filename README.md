# research-theme-template

ふと浮かんだ提案や疑問を、エージェントと議論しながら研究テーマに仕上げるためのテンプレート。
1レポジトリ = 1テーマ。気になることができたらこのテンプレートをクローンして始める。

エージェント向けの指示は `AGENTS.md` にすべて書いてある。ここは人向けの説明。
`CLAUDE.md` は `AGENTS.md` を取り込むだけの一行ファイルで、Claude Code と Codex のどちらで起動しても同じ指示書が自動で読まれる。

## 使い方

```bash
git clone <this-template> my-new-theme
cd my-new-theme
rm -rf .git && git init
claude   # または codex。AGENTS.md / CLAUDE.md が起動時に自動で読まれる
```

Codex は指示ファイルの合計が既定 32 KiB を超えると、警告なしに末尾を切り捨てる。`AGENTS.md` は約 25 KB に収めてあるので、追記するときは `wc -c AGENTS.md` で上限を確認する。超えそうなら `~/.codex/config.toml` に `project_doc_max_bytes = 65536` を入れる。

最初に話しかけると、エージェントは「受け入れモード」で思いつきを聞き、`seed.md` に原文のまま記録した上で、動機を引き出す質問をしてくる。
答え終わると第1ラウンドの議論が走り、`rounds/01/proposal.md` が返ってくる。

## 人がやること

ラウンドごとに、`proposal.md` を読んで次のどちらか。

- **質問する**: 分からないこと、引っかかることを聞く。「案Eをもう少し詳しく」「なぜ案Aは見送り？」など。`qa.md` に残る
- **判断する**: 推薦から選ぶ、その他や見送りから拾い上げる、制約を足す、要望を出す。エージェント（書記）が `decision.md` に清書して確認を求めてくる

判断が確定すると次ラウンドが始まる。「これで良い」と言うと、まず過程を見ていない別のエージェントが提案書の草稿を査読し、その結果を見た上で最終判断する。それでも良ければ `PROPOSAL.md` が生成されて終了、「やめる」と言えば理由とともに終了。

`AGENTS.md` や `templates/` を直したいときは、エージェントにそう頼めば通常のコーディング作業として扱う（テーマの議論には入らない）。

## どこに何があるか

| 見たいもの | 場所 |
|---|---|
| 今のテーマの姿 | `theme.md` |
| 最初の思いつき（原文） | `seed.md` |
| 今回の選択肢 | `rounds/NN/proposal.md` |
| 各案の検討の詳細 | `rounds/NN/discussion.md` |
| 批評家の指摘 | `rounds/NN/critique.md` |
| 自分が過去に下した判断と理由 | `rounds/NN/decision.md` |
| これまでの判断の一覧（採用・却下・保留） | `theme.md` の「判断台帳」 |
| finalize 前の査読結果 | `rounds/NN/review.md` |
| 考えの変遷 | `git diff round-02 round-03 -- theme.md` |
| 読んだ論文・データの確認状況 | `literature/notes.md` |
| 別テーマになりそうな種 | `spinoffs.md` |
| 最終成果物 | `PROPOSAL.md`（finalized 後） |

## 状態

`theme.md` の先頭を見れば、今どこにいるか分かる。

```yaml
round: 3
status: awaiting_decision   # 判断待ち
last_decision: rounds/02/decision.md
```

`awaiting_decision` のときは、エージェントは人の判断なしに次へ進まない。
