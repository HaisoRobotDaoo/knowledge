### 構成概要
[Windows 11 Home（アクセス元）]
Tailscale + Moonlight
|
Tailscale VPN（ポート開放不要）
|
[Ubuntu（リモート先）]
Tailscale + Sunshine

### Step 1: Tailscale のインストール（Ubuntu側）

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

インストール後、以下で起動・ログイン：

```bash
sudo tailscale up
# → ブラウザが開くのでアカウントでログイン
```

Tailscale IPの確認：

```bash
tailscale status
# → 100.x.x.x が自分のTailscale IP
```

### Step 2: Sunshine のインストール（Ubuntu側）

```bash
# 一時ディレクトリ作成
mkdir -p ~/sunshine_install_tmp && cd ~/sunshine_install_tmp

# .debパッケージのダウンロード（Ubuntu 24.04 amd64）
wget https://github.com/LizardByte/Sunshine/releases/latest/download/sunshine-ubuntu-24.04-amd64.deb

# インストール
sudo apt-get update
sudo apt-get install -y ./sunshine-ubuntu-24.04-amd64.deb

# 後片付け
rm -rf ~/sunshine_install_tmp
```

注記: Ubuntu 22.04 の場合はファイル名を sunshine-ubuntu-22.04-amd64.deb に変更してください。

### Step 3: Tailscale のインストール（Windows 11 Home側）

1. https://tailscale.com/download からWindows用インストーラーをダウンロード
2. インストール後、タスクトレイのTailscaleアイコンからログイン
3. Ubuntu側と同じTailscaleアカウントでログインする

### Step 4: Moonlight のインストール（Windows 11 Home側）

1. https://moonlight-stream.org からWindows用インストーラーをダウンロード
2. インストールして起動

### Step 5: Sunshine の起動（Ubuntu側）

```bash
# サービス起動
systemctl --user start sunshine

# (任意) ログイン時に自動起動させる場合
systemctl --user enable sunshine
```

管理UIをブラウザで開く：
https://localhost:47990
初回はユーザー名・パスワードを設定してログイン。

### Step 6: 初回ペアリング

1. Windows側: Moonlightを起動 → ホスト追加で UbuntuのTailscale IP（100.x.x.x）を入力
2. 接続すると4桁のPINが表示される
3. Ubuntu側: https://localhost:47990 の「PIN」タブにPINを入力
4. ペアリング完了

注意: LAN内にいる場合は自動検出されることがあるが、外部ネットワークからは必ずTailscale IP（100.x.x.x）を使うこと。

### 接続時の毎回の手順

| 操作 | Ubuntu側 | Windows側 |
| :--- | :--- | :--- |
| 起動 | systemctl --user start sunshine | Tailscale ON → Moonlight起動 |
| 停止 | systemctl --user stop sunshine | Moonlight終了 → Tailscale任意 |

### （推奨）トラブル回避用設定
リモート操作時の認証ポップアップ等を回避するため、ユーザーを適切なグループに追加します。

```bash
sudo usermod -aG video,input $USER
```

※実行後、一度ログアウト・再ログインが必要です。

### （補足）アンインストール手順（Ubuntu側）

```bash
# サービスの停止と無効化
systemctl --user stop sunshine
systemctl --user disable sunshine

# パッケージの削除
sudo apt-get remove --purge sunshine
sudo apt-get autoremove

# 設定ファイルの削除
rm -rf ~/.config/sunshine
```
