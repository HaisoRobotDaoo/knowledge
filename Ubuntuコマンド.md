# Bashとは
Linux または macOS を使用しているものの、シェルにまだ慣れていない場合は、このチュートリアルが役立ちます。

## シェルの起動スクリプト ~/.bashrc への追加
```bash
echo "<setting_value>" >> ~/.bashrc
```
`echo` は `""`（ダブルクォート）内を文字列として返すので、`"setting_value"` として `~/.bashrc` に書き込むという意味になります。

## update と upgrade の違い

```bash
# aptリポジトリから最新バージョンを取得
sudo apt update

# 最後に取得したaptリポジトリのバージョンに更新
sudo apt upgrade
```

- **`sudo apt update`（情報更新）**
  - **役割**: サーバーから最新のパッケージデータベース（パッケージリスト）をダウンロードし、ローカルの情報を更新する。
  - **影響**: 実際のインストール済みパッケージには影響を与えない。
  - **目的**: 何がアップデートできるかを把握する。
- **`sudo apt upgrade`（実更新）**
  - **役割**: `update` で取得した情報に基づいて、インストール済みのパッケージを新しいバージョンにアップグレードする。
  - **影響**: パッケージが更新される。ただし、システムを破壊するような大きな変更や、他のパッケージを削除しない限りで行われる。
  - **目的**: 既存のアプリケーションを最新にする。 

## grep の基本テンプレート
最も頻繁に使う 「特定のフォルダ内をまるごと検索する」 形です。

```bash
grep -r "<keyword>" <directory_path>
```

- `<keyword>`: 探したい文字列（例: `cmd_vel`, `ERROR`, `turtlebot3`）
- `<directory_path>`: 検索対象のフォルダ（例: `.`（現在地）, `~/turtlebot3_ws/src`）

### パイプ (|) を使った応用テンプレート
「出力結果が多すぎるもの」を絞り込むときに使います。

```bash
<command> | grep -i "<keyword>"
```

- `<command>`: 実行したいROSコマンドなど（例: `ros2 topic list`, `printenv`）
- `-i`: 大文字小文字を無視（Ignore case）
- `<keyword>`: 抽出したいワード

## find の基本テンプレート
カレントディレクトリ構造の検索

```bash
find .
```

## cp の基本テンプレート
ファイルのコピー

```bash
cp <source_path> <destination_path>
```

ディレクトリのコピー

```bash
cp -r <source_directory> <destination_directory>

cp -a <source_directory> <destination_directory>
```

## 圧縮ファイルの展開

```bash
unzip "isaac-sim-standalone-5.1.0-linux-x86_64.zip" -d ~/isaacsim
```
