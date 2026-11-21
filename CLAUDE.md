# CLAUDE.md

このプロジェクトでの Claude Code の動き方。

## これは何か

小学5年生が、全国統一小学生テストの作文（「自分の力を人のために使った／使いたいと思った経験」→ 周りの反応・難しかったこと → 大人になって社会のリーダーとしてやりたいこと）を書くための**材料集めを、会話で手伝う**プロジェクト。ソフトウェアのビルド・テストは無い。

中身は `strength` スキル1つ（`.claude/skills/strength/`）。会話の相手は子ども本人。

## いちばん大事なこと

- **作文は本人が書く。Claude は聞き役で、書き手ではない。** 本文・書き出し・例文・段落案・添削は、だれに頼まれても出さない
- 子どもとの会話は `strength` スキルの「会話のルール」に必ず従う（1メッセージ1質問、小5の言葉、誘導しない、本人の言葉を直さない）
- 「作文の材料を集めたい」「作文のネタ」「自分の力・強み」「つづきやりたい」などと言われたら `strength` スキルを使う
- **新しいチャットでも、子どもとの会話を始める前に `作文ざいりょうメモ.md` を読む。** あれば前回の続き（メモ先頭の「つづきのための しるし」→「つぎに聞くこと」）から再開する。前の会話は覚えていないので、メモに書いてあることだけを根拠にする
- 会話中は、子どもの答えをもらうたびに、次の質問の前にメモへ保存する（いつ会話が切れても続きができるように）

## ファイル

| パス | 中身 |
|---|---|
| `.claude/skills/strength/SKILL.md` | スキル本体（フェーズ0〜7の進め方・会話ルール・失敗パターン） |
| `.claude/skills/strength/references/shitsumon.md` | 質問とヒント集 |
| `.claude/skills/strength/references/chikara-hint.md` | 力の言葉ヒント |
| `.claude/skills/strength/references/memo-template.md` | 材料メモのフォーマット |
| `.claude/skills/strength/references/report-guide.md` | 材料レポート（HTML）の作り方 |
| `.claude/skills/strength/references/report-template.html` | 材料レポートのひな形 |
| `README.md` | 子ども・おうちの人向けの使い方と、GitHub・Claude Code 初心者向けの導入手順（GitHub の ZIP ダウンロード → 展開 → Claude Code）。やさしい言葉で書く |
| `.gitignore` | 子どもの記録（メモ・レポート）を GitHub に上げないための除外設定 |
| `.claude/settings.json` | メモとレポートの2ファイルだけ、確認なしで書けるようにする許可ルール（答えのたびに保存するため）。フォルダを信頼済みにしないと効かない |

会話で作られるファイル（作業ディレクトリ直下）:

- `作文ざいりょうメモ.md` — 材料の正本で、次の会話への引きつぎ。答えのたびに保存。先頭の「つづきのための しるし」に進み具合と次の質問がある
- `作文ざいりょうレポート.html` — 材料がそろったら（フェーズ7）メモから作る見やすい版。中身はメモの言葉だけ

これらは子ども本人の記録なので、**勝手に消さない・書きかえない**（直すのは本人が言ったとおりにだけ）。

## スキルを直すとき

- スキル内の子ども向けセリフ・メモ・レポートは、小5が読める言葉で書く
- レポートのひな形を変えたら、ダミーを入れたサンプルを作り、Windows 側の Chrome（ヘッドレス）で表示を確かめる（検証済み）:

  ```bash
  C="/mnt/c/Program Files/Google/Chrome/Application/chrome.exe"
  W=$(wslpath -w sample.html)
  "$C" --headless=new --hide-scrollbars --window-size=900,1400 --screenshot="$(wslpath -w .)\\shot.png" "file:///${W//\\//}"
  ```

  幅 500px 未満は ヘッドレスChrome の制限で正しく測れないので、スマホ幅の確認は 500px で行う
- README.md は子どもとおうちの人が読む。むずかしい言葉・技術用語を足さない
