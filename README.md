# zmk-config-Pyuron

Pyuron のファームウェアです。

- main ブランチ = PMW3610
- paw3220 ブランチ = PAW3222 / PAW3220

## キー割り当て一覧

各レイヤーのキー割り当ては [KEYMAP.html](KEYMAP.html) にまとめています。ブラウザでレンダリング表示する場合は以下のリンクから閲覧できます。

https://htmlpreview.github.io/?https://github.com/ryo-aoki-pc/zmk-config-Pyuron/blob/custom/KEYMAP.html

キーマップを変更すると GitHub Actions（`keymap-docs.yml`）が `KEYMAP.html` / `KEYMAP.xlsx` を自動で再生成します。

## 命名規則（カスタムビヘイビア）

`config/Pyuron.keymap` の Macro / Tap Dance / Mod Morph は以下の規則で命名します。

- **構造**：`<prefix>_vim_<id>` 形式。`<prefix>` は `macro_`（マクロ）/ `td_`（タップダンス）/ `mm_`（モッドモーフ）。ノードラベル・ノード名・`label` を一致させ、`label` はラベルの大文字にする（例：`mm_vim_g` → `label = "MM_VIM_G"`）。
- **Mod Morph の `<id>`**：キーに直接割り当てるモーフは無修飾時の vim キーで命名（`mm_vim_d` `mm_vim_g` など）。ベースが `&none`（修飾時のみ動作）またはネスト用ヘルパーは、修飾＋キーストロークで命名する（`mm_vim_shift_4` `mm_vim_ctrl_r` `mm_vim_shift_d`）。

# ファームウェア更新手順とキーマップの編集

## ZMK Studioによるキーマップの編集
### 手順1. 有線接続でキーマップを変更する場合は[ZMK Studio](https://zmk.studio/)にアクセス
### 手順2. 中央にある"USB"をクリックしてデバイスを選択することでキーマップの編集が可能になります。
### 手順3. 無線接続でキーマップを変更する場合は[ZMK Studioネイティブアプリ](https://zmk.studio/download)にアクセスしてファイルをダウンロード
### 手順4. インストール後に起動、接続中のデバイスが表示されるのでデバイスを選択するとキーマップの編集が可能になります。

## Keymap-Editorによるキーマップの編集 (Githubアカウントが必要になります)
### 手順1. [Pyuronリポジトリ](https://github.com/ryo-aoki-pc/zmk-config-Pyuron) リポジトリをFork
### 手順2. Actionsタブを選択して"I understand my workflows, go ahead and enable them"をクリック
### 手順3. [Keymap-Editor](https://nickcoutsos.github.io/keymap-editor/)にアクセス
### 手順4. Githubでログインをしたあと"Only select repositories"を選択、"Add Repository"をクリック
### 手順5. 再度"Only select repositories"を選択、"Install"をクリック
### 手順6. キーマップが表示され、編集可能になります。
### 手順7. 編集が完了したら左上の"Save"をクリック、ビルドが完了すると"Latest"からダウンロードできます。
### ダウンロードしたファームウェアは以下の手順で書き込んでください。

## ファームウェアの書き込み

### 手順1. PCと右側ボードを接続します。
### 手順2. 表にあるリセットスイッチ用のボタンを2回素早く押すとブートローダーが起動して"XIAO SENSE"として認識されます。
### 手順3. まずは"XIAO SENSE"ドライブに"settings_reset-seeeduino_xiao_ble-zmk.uf2"を書き込みます。
### 手順4. 次に同じ手順でブートローダーを起動し`Pyuron_right_central.uf2`を書き込みます。
### 手順5. 右側が終わったら左側も同じように"settings_reset-seeeduino_xiao_ble-zmk.uf2"から`Pyuron_left_peripheral.uf2`を書き込みます。
### 手順6. 書き込みが完了したら念の為1回リセットボタンを押してください。
### 手順7. 左右どちらも電源をオンにしたあとにBlutoothデバイスの追加から"Pyuron"を選択し、接続ができたらファームウェアの書き込みは完了です。


#### ・ファームウェアの書き込みの際エラーが出ていても実際には正常で書き込めています。手順を進めて問題ありません。
#### ・ファームウェアを更新する際は一度PC側のデバイスから削除して再度ペアリングをしてください。

## 生成されるファームウェア一覧

| ファームウェア名 | 説明 |
| --- | --- |
| `Pyuron_left_peripheral.uf2` | 左側 ペリフェラル |
| `Pyuron_right_central.uf2` | 右側 セントラル |
| `Pyuron_right_central_studio.uf2` | 右側 セントラル (ZMK Studio 対応) |
| `settings_reset-seeeduino_xiao_ble-zmk.uf2` | 設定リセット用 |

## ローカルビルド手順

GitHub Actions でのビルドは毎回 2〜3 分かかりますが、ローカル環境では 40 秒〜1 分で完了します (PC スペックによって前後します)。
キーマップを少し試したいだけでもローカルビルドなら素早く試行錯誤ができます。

### 必要なもの

- [Visual Studio Code](https://code.visualstudio.com/)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- VS Code 拡張機能: [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

### 手順

1. **準備**
   1. このリポジトリを PC に clone します。
   2. Docker Desktop を起動します。
   3. VS Code でこのフォルダを開きます。
   4. 右下に表示される「Reopen in Container (コンテナーで再度開く)」をクリックします (初回は環境構築に時間がかかります)。

2. **ビルド**

   VS Code のターミナルで以下のいずれかを実行します。

   > [!TIP]
   > ビルドは CPU コアを使って並列実行できます。並列数は自動で CPU コア数になりますが、
   > 環境変数 `PARALLEL` で指定することもできます (例: `PARALLEL=4 make all_p`)。

   | コマンド | 内容 |
   | --- | --- |
   | `make` | 全ファームウェアを並列ビルド (ZMK Studio 版を除く) |
   | `make all` | 全ファームウェアを逐次ビルド (ZMK Studio 版を除く) |
   | `make all_studio_p` | ZMK Studio 版も含めて並列ビルド |
   | `make all_studio` | ZMK Studio 版も含めて逐次ビルド |
   | `make single` | 一覧から番号を選んで 1 つだけビルド |
   | `make clean` | `firmware_builds/` を削除 |

   キーマップ変更だけを試すなら `Pyuron_right_central` のみで十分です。

3. **完成**

   `firmware_builds/` に `.uf2` が生成されます。これをキーボードに書き込みます。
