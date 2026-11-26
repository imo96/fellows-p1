# 研修用プログラム生成手順

Nov 26, 2026

## 1. 目的と全体像

受講生は、Claude(Free プランのチャット)に「指示書」と画面 HTML・テーブル定義書を添付して、Python(Django)のアプリを生成し、ログイン認証と情報表示が仕様どおりに動くことを確認します。講義の手順を減らすため、生成する言語は **Python のみ**、データベースは Python に組み込まれている **SQLite** です(データベースのインストールは不要)。題材は、商品・注文を扱うアプリ(作業フォルダ C:\fellowsapp)です。

| フェーズ | 場所 | 生成・確認するもの |
| --- | --- | --- |
| ① スタンドアロン | 受講生のパソコン | C:\fellowsapp に配置した SQLite のファイル(myappdb.sqlite3)を使い、VSCode(Python / Django)で、ログイン認証と情報表示を確認 |
| ② WEB | AWS(講師が配布する AMI) | ①で作った Python を Django(`http://IP:8081/`)にデプロイして動作確認(AWS は PostgreSQL。7 章を参照) |

アプリの機能は「ログイン → 商品一覧(商品ごとの注文件数) → 注文情報詳細」と、各画面のログアウトです。ユーザー登録画面は作りません。DB の初期データにある 1 ユーザー(user@example.com / 1234)でログインできることが、この研修の到達点です。

**全体の流れ**

1. 事前準備(受講生は、講義の前に「①前準備リポジトリ」の ①環境構築 と ②Claude利用開始 を済ませておく。このリポジトリの教材の受け取り)
2. 指示書を Claude にチェックさせて手直しする(完成できない場合は、講師が用意した指示書を使う)
3. Claude のチャットに指示書と資材を添付し、Python のアプリを生成して各自で動かす
4. フェーズ②で AWS にデプロイする(別手順書)

## 2. 事前準備

### 2.1 環境構築(講義の前に完了させる)

講義で使う開発環境(Python、VSCode)と、アプリを置く作業フォルダ `C:\fellowsapp`(データベースのファイル myappdb.sqlite3 入り)は、**①前準備リポジトリ**(https://github.com/imo96/fellows-p0)の環境構築ツール(setup.bat)で準備します。**講義の前に、①前準備リポジトリの README の手順で環境構築を済ませておいてください。**

- 環境構築の手順: ①前準備リポジトリの `README.md` と `①環境構築\②一括セットアップの使い方.txt`
- 環境構築で問題が発生した場合は、講義の中で講師が対応します。

環境構築が済んでいると、次の状態になっています。

| 用途 | 準備されているもの | 補足 |
| --- | --- | --- |
| Python | VSCode、Python 3.12 以上 | Django 6.1 は、生成したアプリの venv に入れる |
| DB | (インストール不要)Python に組み込みの SQLite | 作業フォルダ C:\fellowsapp(setup.bat が作成)に、テーブルと初期データ入りの myappdb.sqlite3 が置かれる |

### 2.2 Claude の利用開始(講義の前に完了させる)

Claude のアカウント(無料プラン)を、講義の前に作成しておいてください。登録に**電話番号の認証が必要になることがある**ため、講義の当日には間に合わないことがあります。

- 手順: ①前準備リポジトリの `②Claude利用開始\claude開始手順.md`

### 2.3 講義で使うもの

| 用途 | ソフト | 注意点 |
| --- | --- | --- |
| 生成 | Claude(Free プランのチャット) | ②Claude利用開始(①前準備リポジトリ)を済ませ、ログイン済みであること(手順は `②Claude利用開始\claude開始手順.md`)。Free は 5 時間単位の使用量制限があるので、チャットを新しく開いて生成する |

**講師から渡すもの**

- このリポジトリ(②講義用リポジトリ)の教材フォルダ「②講義内容」(下の構成)
- データベースのファイル myappdb.sqlite3 は、①前準備リポジトリの `①環境構築\商品関連テーブル` と、このリポジトリの `⑥生成アプリ動作検証方法` に入っている(Claude に読み込ませる資材と混ざらないよう、②アプリ作成に必要な資材 には入れない)(受講生の PC には、setup.bat が `C:\fellowsapp` に配置する。setup.bat を実行していない場合は、後者を `C:\fellowsapp` にコピーする)

**フォルダ構成**

①前準備リポジトリ(受講生が講義の前に実施)

```
①環境構築\                 setup.bat, setup.ps1, ①手順.txt など
  商品関連テーブル\        テーブル定義書（myappdb）.sql, myappdb.sqlite3, create_db.py, テーブル定義書（myappdb）SQLite用.xlsx, テーブル定義書（myappdb）AWS用.xlsx
②Claude利用開始\           claude開始手順.md, images\
```

②講義用リポジトリ(講義で使う)

```
①Claudeについて\           ClaudeDesktopアプリの位置づけ.pdf, Claudeの提供形態とライセンス.md
②講義内容\
  研修内容.txt              講義の進め方
  ①HTML素材からhtml作成\    画面定義.xlsx, ロゴ, 生成したhtml\
  ②アプリ作成に必要な資材(HTML.テーブル定義書)\   login.html, product_list.html, order_detail.html, style.css, logo.svg, テーブル定義書（myappdb）.sql
  ③指示書（原本）\          指示書（原本Free版）.xlsx
  ④指示書（完成版）\        指示書.md
  ⑤成果物\                  myapp_Python.zip(講師が生成した Python アプリ)
  ⑥生成アプリ動作検証方法\   アプリ実行手順_Python.md, myappdb.sqlite3(DB の予備。②の資材とは別にしてある)
```

講義では、Claude のチャットにファイルを添付し、zip で受け取ります。

## 3. 題材とテーブル定義

### 3.1 接続先(フェーズ①)

| 項目 | 値 |
| --- | --- |
| DBMS | SQLite(Python 組み込みの sqlite3。追加のライブラリは不要) |
| DB ファイル | `C:\fellowsapp\myappdb.sqlite3`(生成したプロジェクトの manage.py と同じフォルダ。①前準備リポジトリの setup.bat が配置する) |
| ユーザー・パスワード | なし(ファイルを開くだけ) |

作業フォルダ `C:\fellowsapp` と myappdb.sqlite3 は、環境構築の `setup.bat` が作ります(OneDrive の同期対象外で、ユーザー名に依存しない場所)。Claude が生成した zip は `C:\fellowsapp` に展開して上書きします(zip に myappdb.sqlite3 は含めないので、DB は消えません)。講義後は `C:\fellowsapp` を削除すれば、アプリ・venv・DB がまとめて消えます。環境変数の設定は不要です(`DJANGO_SECRET_KEY` は、無ければ開発用の値が使われます)。

### 3.2 テーブル定義書(sql)

テーブル定義は Excel ではなく、DDL と初期データ(INSERT)を書いた `テーブル定義書（myappdb）.sql`(UTF-8、SQLite 用)です。myappdb.sqlite3 は、この SQL を `create_db.py`(Python 標準ライブラリだけで動く。①前準備リポジトリの `①環境構築\商品関連テーブル` にある)で実行して作ったものです。生成したプログラムは SELECT だけを行い、テーブルの作成やデータの投入はしません。

| テーブル | 主な列 | 備考 |
| --- | --- | --- |
| 商品 | 商品コード(PK)、商品名、単価、商品区分、関連商品コード | 42 件 |
| 注文 | 注文日・注文番号・注文枝番(複合 PK)、商品コード、数量、クーポン割引料 | 68 件。クーポン割引料は NULL あり |
| ユーザー情報 | ユーザーid(連番 PK)、eメール、パスワード | 1 件(user@example.com / パスワード 1234) |
| 廃番商品 | 商品コード(PK)、廃番日、売上個数 など | 9 件。本アプリでは使わない |

> **注意**: この SQL は先頭で各テーブルを `DROP TABLE` してから作り直します。①前準備リポジトリの `①環境構築\商品関連テーブル` で `python create_db.py` を実行すると、同じフォルダの myappdb.sqlite3 は初期状態に作り直されます。SQLite では、PostgreSQL 版から次の 2 点だけ変えています: `DROP TABLE … CASCADE` の `CASCADE` を削除、`SERIAL` を `INTEGER PRIMARY KEY AUTOINCREMENT` に変更。日付は 'YYYY-MM-DD' の文字列で保存されます。

パスワード列には、入力値を UTF-8 の SHA-256 でハッシュ化した小文字 16 進 64 桁(ソルトなし)を入れます。1234 のハッシュは `03ac674216f3e15c761ee1a5e255f067953623c8b388b4459e13f978d7c846f4` です。

### 3.3 画面仕様

| 画面 | URL | 内容 |
| --- | --- | --- |
| ログイン | `/login` | eメールとパスワードで認証。失敗時は「ユーザ名またはパスワードが正しくありません。」 |
| 商品一覧 | `/products` | 商品コード順。注文状況に「注文(n 件)」を表示し、注文情報詳細へリンク |
| 注文情報詳細 | `/orders?code=商品コード` | その商品の注文を、注文日降順・注文番号降順・注文枝番昇順で表示 |
| ログアウト | `POST /logout` | セッションを破棄してログイン画面へ |

- 未ログインで商品一覧・注文情報詳細を開いたら、ログイン画面へ戻す
- 注文件数は `COUNT(DISTINCT o.注文日 || '|' || o.注文番号)` で数え(SQLite の COUNT(DISTINCT) には列を 2 つ並べて書けないため、連結する)、0 件は「注文なし」(リンクなし)
- 関連商品コードが無い商品は「―」、クーポン割引料が NULL の注文は空欄
- 画面の項目・並び・表示形式は `②アプリ作成に必要な資材(HTML.テーブル定義書)` の HTML に合わせる

## 4. 指示書の構成

生成ルールは、指示書(`指示書.md`)に書いてあります。Claude には、この md(または受講生が手直しして完成させた指示書)を添付します。

| 指示内容 | 中身 |
| --- | --- |
| １ 作業フォルダ | 添付するファイルと zip の作り方 |
| ２ 生成物の条件 | 言語・バージョン、プロジェクト構成、URL、DB の指定、テーブル定義の参照 |
| ３ ログイン画面 | SHA-256 による認証方法 |
| ４ 商品一覧画面 | 注文件数の数え方、表示形式 |
| ５ 注文情報詳細画面 | 抽出条件と並び順 |
| ６ 動作確認 | 生成物の起動手順 |

指示書の要点は次のとおりです。

- **Python**: Django 6.1(追加のライブラリなし)。`INSTALLED_APPS` は staticfiles・app のみで、セッションは署名付き Cookie に保存するため、`migrate` は不要。モデルは `managed = False` で、注文は `CompositePrimaryKey`。DB は `BASE_DIR / 'myappdb.sqlite3'`。`DJANGO_SECRET_KEY` は環境変数があればそれを使う

## 5. 生成手順(Free プランのチャット)

### 5.1 進め方

1. Claude のチャットを新しく開く
2. 添付ファイルを付ける: 指示書.md、login.html、product\_list.html、order\_detail.html、style.css、logo.svg、テーブル定義書（myappdb）.sql(「\_1」などが付いても同じファイルとして扱われる)
3. 次の文を送る
4. 出力された zip をダウンロードし、`C:\fellowsapp` に展開する(講師が生成した見本は `⑤成果物` の myapp_Python.zip)
5. 動かしてエラーが出たら、エラー全文を貼って修正させる
6. 動いたら 6 章の確認項目を通す

```
添付の指示書.md に従って、Python(Django)のアプリを生成してください。
添付の html・style.css・logo.svg・テーブル定義書(sql)を使い、完成したら zip にまとめて出力してください。
```

### 5.2 Python(VSCode / Django)

1. zip を `C:\fellowsapp` に展開する(直下に manage.py がある。同名のファイルは上書きする。「すべて展開」でサブフォルダができないよう、展開先を `C:\fellowsapp` にする)
2. そのフォルダで venv を作り、依存を入れる

```
python -m venv venv
venv\Scripts\activate.bat
pip install -r requirements.txt
```

(VS Code のターミナルは標準で PowerShell です。PowerShell では `.\venv\Scripts\Activate.ps1` を使います。詳しくは `アプリ実行手順_Python.md` の注意を参照)

3. `C:\fellowsapp` に `myappdb.sqlite3` があることを確認する(setup.bat が配置済み)
4. `python manage.py runserver` で `http://127.0.0.1:8000/login` を開く

## 6. 動作確認

講師は研修前に同じ手順を一度通しておきます。

### 6.1 確認項目

- [ ] 初期データのユーザー(user@example.com / 1234)でログインできる
- [ ] 間違ったパスワードではログインできず、エラー文言が出る
- [ ] 未ログインで商品一覧・注文情報詳細の URL を直接開くと、ログイン画面に戻る
- [ ] 商品一覧の「注文(n 件)」が、DB で集計した件数と一致する。注文の無い商品は「注文なし」でリンクが無い(12 商品)
- [ ] 注文情報詳細が注文日降順・注文番号降順・注文枝番昇順で、クーポン割引料が NULL の行は空欄
- [ ] 「← 商品一覧に戻る」とログアウトが動き、ログアウト後に商品一覧へ戻れない

### 6.2 件数の確認(任意)

ログインできないとき、または件数を確認したいときは、Python だけで DB を確認できます(`cd /d C:\fellowsapp` で移動してから `python` を起動して実行)。

```python
import hashlib, sqlite3
con = sqlite3.connect('myappdb.sqlite3')
print(con.execute('SELECT eメール, length(パスワード) FROM ユーザー情報').fetchall())   # 64 であること
print(hashlib.sha256('1234'.encode('utf-8')).hexdigest())                                 # DB のパスワード列と一致すること
print(con.execute("SELECT COUNT(DISTINCT 注文日 || '|' || 注文番号) FROM 注文").fetchone())
```

## 7. フェーズ② AWS への接続

AWS(AMI)側は現状のままです。ローカルは SQLite ですが、AWS では **PostgreSQL** で動かします(2026/11/26 決定)。①で動いたコードは、アプリのコードを直さずに AWS に載せられるのが目標です。DB の違いは、資材をアップロードするときの設定で吸収します。

| 項目 | 内容 |
| --- | --- |
| 公開 URL | `http://IP:8081/` |
| 送るもの | `myapp.tar.gz`(config、app、static、templates、manage.py、requirements.txt。`manage.py` はアーカイブの直下に置く)。**myappdb.sqlite3 と venv は含めない** |
| 反映方法 | FileZilla で `/home/ec2-user/` へ送り `bash deploy.sh`(`/opt/myapp/` へ同期 → `pip install -r requirements.txt` → `migrate` → `collectstatic` → gunicorn 再起動) |
| 本番設定 | `/opt/myapp-conf/settings_prod.py` と `/etc/myapp/env` が上書き(`DATABASES` を PostgreSQL にし、`DJANGO_SECRET_KEY` と DB 接続の環境変数を `/etc/myapp/env` から読む) |

**tar.gz の作り方**(PowerShell またはコマンドプロンプトで、`manage.py` があるフォルダ `C:\fellowsapp` に移動して実行します)

```
cd C:\fellowsapp
tar -czf myapp.tar.gz --exclude=__pycache__ --exclude=myappdb.sqlite3 config app static templates manage.py requirements.txt
```

- 含めるフォルダ・ファイルをコマンドで明示しているため、`venv` と `myappdb.sqlite3` は自動的に入りません(手作業で外す必要はありません)。`--exclude` は念のための保険です
- `C:\fellowsapp` で実行すると、`manage.py` がアーカイブの直下に入ります(`deploy.sh` はこれを確認します)
- `myapp_Python.zip` や `venv` フォルダを tar に追加しないでください

資材をアップロードするときに必要な変更(DB 定義まわり):

- `requirements.txt`: AWS の `deploy.sh` は `pip install -r requirements.txt` を実行するため、ファイル自体は必須(Claude が生成した `Django>=6.1,<6.2` のままで動く)。バージョンを固定したい場合だけ、アプリのフォルダで `python -m pip freeze > requirements.txt`(PowerShell は `| Out-File -Encoding ascii requirements.txt`。`>` は UTF-16 になるため使わない)で作り直す(任意)。ローカルの SQLite 用には `psycopg` が入っていないが、AMI の venv に `psycopg[binary]` が導入済みのため、追加は不要
- `settings.py` の `DATABASES`(SQLite)は、`settings_prod.py` が PostgreSQL の設定で上書きする(`ENGINE`: `django.db.backends.postgresql`、`HOST`・`PORT`・`NAME`・`USER`・`PASSWORD` は環境変数)。アプリのコードは変更しない
- AWS の PostgreSQL には、従来どおり PostgreSQL 版の `テーブル定義書（myappdb）.sql`(`DROP TABLE … CASCADE`、`SERIAL`。元の `AI生成プログラム` フォルダにある Shift_JIS のファイル)でテーブルと初期データを作る(今回の SQLite 版 sql は SQLite 専用)
- アプリの SQL(`COUNT(DISTINCT o.注文日 || '|' || o.注文番号)`)は、PostgreSQL でも使える書き方にしてある(AWS での動作は未確認)

> AMI の `settings_prod.py` は、`config.settings` を読み込んだうえで `DATABASES` を PostgreSQL に上書きし、`DEBUG`・`SECRET_KEY`・`ALLOWED_HOSTS`・`CSRF_TRUSTED_ORIGINS`・`STATIC_ROOT` も上書きする(環境変数は `/etc/myapp/env`)。上の方針と一致している。

## 8. よくある落とし穴

検証で実際に踏んだものです。受講生が詰まったらまずこの表を見ます。

| 症状 | 原因 | 対処 |
| --- | --- | --- |
| ログイン時に `no such table` | C:\fellowsapp に myappdb.sqlite3 が無いまま起動し、Django が空の DB を作った | 起動を止め、できた空の myappdb.sqlite3 を削除して、`⑥生成アプリ動作検証方法` の myappdb.sqlite3 を `C:\fellowsapp` にコピーする(または環境構築の `setup.bat` をもう一度実行する) |
| ログインできない(パスワードが違う扱いになる) | ハッシュ形式の不一致 | 6.2 で確認し、指示書のハッシュ仕様を示して修正させる |
| 注文の無い商品が「注文(1 件)」になる | 注文日・注文番号を連結せず、LEFT JOIN の NULL を数えている | `COUNT(DISTINCT o.注文日 \|\| '\|' \|\| o.注文番号)` にする(NULL は数えられない) |
| `COUNT(DISTINCT (a, b))` で SQL エラー | SQLite は COUNT(DISTINCT) に 2 列を指定できない | 2 列を `\|\|` で連結する |
| Django で makemigrations しても何も起きない / 実行してはいけない | 業務テーブルは既存(managed = False) | 実行しない。テーブルは myappdb.sqlite3 にある |
| ログイン後にリダイレクトがループする | `request.path` で比較している | `request.path_info` で比較する |
| requirements.txt が作れない / pip install が失敗 | UTF-16 になっている、または pip を実行できない環境で作った | `Out-File -Encoding ascii`。無ければ Django の 1 行を手で書く |
| データを初期状態に戻したい | | `⑥生成アプリ動作検証方法` の myappdb.sqlite3 を `C:\fellowsapp` に上書きコピーする(または DB を削除して `setup.bat` をもう一度実行する) |
| コマンドが通らない | PowerShell / cmd の取り違え | 本書のコマンドは cmd(コマンドプロンプト)用。PowerShell では `Activate.ps1` などに読み替える(アプリ実行手順_Python.md の比較表を参照) |

## 9. 未決事項

- [x] テーブル構成と初期データ(`テーブル定義書（myappdb）.sql` で確定。SQLite 用に変更)
- [x] 研修の対象は Python のみ、DB は SQLite にする(2026/11/26)
- [x] 作業フォルダは C:\fellowsapp(setup.bat が作成し、myappdb.sqlite3 を配置)。デスクトップは OneDrive 同期の可能性があるため使わない(2026/11/26)
- [x] 講師による事前生成と動作確認(myapp_Python.zip を SQLite 用に作り直し、Django 6.1 で確認済み)
- [x] フェーズ② AWS は現状の AMI のまま。ローカルは SQLite、AWS は PostgreSQL で動かす(2026/11/26)
- [x] AMI 側の設定を確認(settings_prod.py が PostgreSQL 設定、psycopg 導入済み、環境変数は /etc/myapp/env)
- [ ] AWS 上での動作確認(PostgreSQL 版 sql の用意、このアプリを載せて動かす)
- [x] `テーブル定義書（myappdb）.xlsx` は、`SQLite用` と `AWS用`(PostgreSQL 時点の内容)の 2 種類にする(2026/11/26)
