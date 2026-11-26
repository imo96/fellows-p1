# アプリ実行手順（Python）

Python アプリの生成から動作確認までの手順です。データベースは、Python に組み込まれている SQLite を使います。

## 起動方法

1. **プログラム用資材をアップロード** する
2. **Claude でプログラミング** する
3. Claude が、作成したプログラムを **zip で圧縮** する
4. ダウンロード後、**C:\fellowsapp に解凍** する(環境構築で作成済みのフォルダ。同名のファイルは上書きする。myappdb.sqlite3 は zip に入っていないので消えない)
   - 「すべて展開」の展開先欄は `C:\fellowsapp` にし、`C:\fellowsapp\zip名` のようなサブフォルダができていないこと(`C:\fellowsapp` の直下に manage.py があること)を確認する

### 参考: cmd と PowerShell で使えるコマンドの比較

| やりたいこと | cmd | PowerShell |
|---|---|---|
| ファイルの中身を表示 | `type` | `Get-Content` |
| フォルダを移動 | `cd /d C:\fellowsapp` | `cd C:\fellowsapp` |
| venv を有効化 | `venv\Scripts\activate.bat` | `.\venv\Scripts\Activate.ps1` |
| ファイルに書き出す | `> file`(そのまま使える) | `Out-File -Encoding ascii`(`>` は UTF-16 になる) |

以降は cmd で説明します。

> **注意**: VS Code のターミナルは、標準で **PowerShell** です。cmd のコマンド(`venv\Scripts\activate.bat` など)を PowerShell に入力しても、仮想環境は有効になりません(`activate.bat` は別の cmd の中で動くため)。次のどちらかにしてください。
> - ターミナル右上の「＋」の横の「▼」から **Command Prompt** を選び、cmd のターミナルを開く
> - PowerShell のまま、上の表の PowerShell 列のコマンドを使う(`Activate.ps1` が「スクリプトの実行が無効」と拒否されたら、`Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass` を実行してから、もう一度実行する)
>
> 有効になると、行の先頭に `(venv)` と表示されます。表示されていないときは、まだ有効になっていません(`deactivate` も使えません)。

### 1. 仮想環境の作成

ターミナルを起動し、`C:\fellowsapp`(manage.py があるフォルダ)に移動します。

```
cd /d C:\fellowsapp
```

- 初回のみ: 仮想環境を作成する

  ```
  python -m venv venv
  ```

- 毎回実施: 仮想環境を有効化する(行の先頭に `(venv)` と出れば成功)

  ```
  venv\Scripts\activate.bat
  ```

  PowerShell の場合: `.\venv\Scripts\Activate.ps1`

- 作業終了時に実施: 仮想環境を終了(無効化)する

  ```
  deactivate
  ```

### 2. インストール

```
pip install -r requirements.txt
```

### 3. データベース(SQLite)のファイルの確認

`C:\fellowsapp` に **myappdb.sqlite3** があることを確認します(環境構築の `setup.bat` が配置しています)。

```
C:\fellowsapp\
  manage.py
  myappdb.sqlite3   ← 環境構築で配置済み(Claude の zip には含まれない)
  config\
  app\
  venv\
  ...
```

- テーブルと初期データは、このファイルの中に入っています。環境変数の設定や migrate は不要です。
- ファイルが無いまま起動すると、Django が空のデータベースを新しく作ってしまい、ログイン時に `no such table` というエラーになります。その場合は、起動を止め(Ctrl+C)、できた空の myappdb.sqlite3 を削除して、このフォルダ(`⑥生成アプリ動作検証方法`)の `myappdb.sqlite3` を `C:\fellowsapp` にコピーしてください(環境構築の `setup.bat` をもう一度実行しても、DB が無ければ配置されます)。
- データを初期状態に戻したいときは、このフォルダ(`⑥生成アプリ動作検証方法`)の `myappdb.sqlite3` を `C:\fellowsapp` に上書きコピーします。
- 講義のあと環境を消したいときは、`C:\fellowsapp` フォルダを削除します(アプリ、venv、データベースがまとめて消えます)。

### 4. Django 製の内蔵 Web サーバーを起動

```
python manage.py runserver
```

### 5. URL へアクセス

- http://127.0.0.1:8000/

| 項目 | 値 |
|---|---|
| ユーザ名 | `user@example.com` |
| パスワード | `1234` |
