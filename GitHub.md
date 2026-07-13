# GitHubの使い方

## リモートをローカルへclone
```bash
git clone https://github.com/TAISEIETO/iputokAI_repo.git
```

---

## ① ターミナルでプロジェクトフォルダに移動
```bash
cd ~/catkin_ws_iputok_test/src/iputok_visions
```

## ② Git 初期化
```bash
git init
```

## ③ GitHub のリポジトリを origin に設定
GitHubリポジトリ画面右上のcodeからURLを取得します。
```bash
git remote add origin https://github.com/TAISEIETO/iputokAI_repo.git  # repo_name=originとして登録
```

## ④ ファイルを追加してローカルの初回commit
```bash
git add .  # 2回目以降は特定のファイル名.拡張子を指定
git commit -m "Initial commit"  # ""内にメッセージを追加
```

## ⑤ ローカルをリモートへ push
最初だけブランチ名指定が必要です。
```bash
git branch -M main  # GitHubのデフォルトがmainの場合
git push -u origin main # ローカルのmainをoriginへpush（-uをつけると二回目以降は git push のみで可能になる）
git remote -v
```

## ⑥ リモートの変更をローカルへpull
```bash
git pull origin main
```

---

## 補足（初回のみ）
```bash
git config --global user.name "Your Name"  # --globalはPC全体のリポジトリで使う設定
git config --global user.email "ok230xxx@oks.iput.ac.jp"
```
