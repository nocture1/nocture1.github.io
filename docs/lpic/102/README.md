## エイリアス
> コマンドに別名をつけたり、オプションをひとまとめにした新しいコマンドにできる

```bash
# 設定方法
$ alias コマンド=xxx

# 削除方法
$ unalias コマンド

# 一時的に無効化する
$ \ラップされたコマンド

# 登録したエイリアスを表示する
$ alias
```

## 関数
> 頻繁に利用するコマンドの組み合わせを定義し、bashシェル上で利用できる独自の関数

```bash
# 設定方法
$ [function] 関数名() { コマンド }

# 登録した関数を表示する
# （set では変数も表示される）
$ declare -f

# 削除方法
$ unset 関数名
```

## 設定ファイルの実行順序

![image](https://github.com/user-attachments/assets/9a40b1a4-aaaa-4596-bbcf-a3267c798520)

## シェルスクリプト

```bash
# 実行方法
# 読み取り権が必要
$ bash シェルスクリプトファイル名

$ source シェルスクリプトファイル名

$ . シェルスクリプトファイル名

# 実行権が必要
$ ./シェルスクリプトファイル名

# プロセスが現在のシェルに上書きされる
$ exec シェルスクリプトファイル名
```

### スクリプトに関する引数

|変数名|役割|
|:--|:--|
|$0|シェルスクリプトのファイル名（フルパス）
|$1|1番目の引数
|$2|2番目の引数。以下、$3, $4, ... と続く
|$#|引数の数
|$@|すべての引数（スペース区切り）
|$*|すべての引数（デリミタは `IFS` で指定）
|$?|返り値

## ユーザーアカウント
`/etc/passwd` で管理

```
                            ①          ②
lpic:x:1000:1000:LPI Linux :/home/lpic:/bin/bash
```

①：ホームディレクトリ  
②：デフォルトシェル

パスワードはセキュリティのため `x` 固定。
`root` しか読み取れない `/etc/shadow` で管理されている。

## グループアカウント
`/etc/group` で管理

```
staff:x:1002:lpic, linux
```

### useradd
ユーザーアカウントを追加する

|オプション|備考|
|:--|:--|
|-c コメント|comment|
|-d パス|home directory|
|-g グループ名・GID|group|
|-G グループ名・GID|secondary group|
|-s パス|shell|
|-m|make home directory<br>`/etc/skel` の内容をコピーし配置する|

## cron
定期的にジョブを実行する。  
デーモン `crond` とスケジューリングを編集する `crontab` から構成

ユーザーごとのcrontabは `/var/spool/cron` ディレクトリに配置。  
システムのcrontabは `/etc/cron.*` ディレクトリのファイルを参照。

### crontabの書式
```
分 時 日 月 曜日 コマンド
```
※曜日：0,7が日曜

複数の時間帯で実行： `9,12`  
～毎に実行： `*/2`

## at
1回限りの実行スケジュールを扱う

```bash
$ at [-f ファイル名] 日時
```

### atの実行書式

|指定日時|書式|
|:--|:--|
|午後10時|22:00, 10pm|
|正午|noon|
|真夜中|midnight|
|今日|today|
|明日|tomorrow|
|3日後|now + 3 days|
|2週間後の午後10時|10pm + 2 weeks|

## cronのアクセス制御
1. `/etc/cron.allow` があれば、ここに記述されたユーザーだけが `cron` を利用できる
2. `/etc/cron.deny` がなければ、 `/etc/cron.deny` に書いていないユーザーが `cron` を利用できる

## atのアクセス制御
1. `/etc/at.allow` があれば、ここに記述されたユーザーのみが `at` を利用できる
2. `/etc/at.allow` がなければ、 `/etc/at.deny` に書いていないユーザーが `at` を利用できる
3. どちらもなければ `root だけが `at` を利用できる

## iconv
文字コードを変換する

```bash
$ iconv [オプション] [入力ファイル名]
```

|オプション|役割|
|:--|:--|
|-f 文字コード|from<br>変換前の文字コード|
|-t 文字コード|to<br>変換後の文字コード|
|-l|扱える文字コードを表示

## タイムゾーン
`/usr/share/zoneinfo` ディレクトリ以下で管理

システムで利用するタイムゾーンは↑を `/etc/localtime` にコピーする  
環境変数 `TZ` でも設定可能

```bash
$ export TZ="Asia/Tokyo"
```

値を全ユーザーで利用する場合 `/etc/timezone` に "Asia/Tokyo" を設定しておく  
`tzselect` コマンドで一覧から選択してタイムゾーンの設定値を確認できる

## クロック
- ハードウェアクロック
    + ハードウェアとして内蔵された時計
    + コンピュータ内に内蔵した電池で動き続ける
- システムクロック
    + Linuxのカーネル内に存在する時計

```bash
# システムクロックを参照して現在の日時を表示
$ date
```

|書式|役割|
|:--|:--|
|%Y|year|
|%m|month|
|%d|date|
|%H|hour|
|%M|minute|
|%a|day|
|%b|month name|

```bash
# ハードウェアクロックの参照・設定
$ hwclock
```

```bash
# systemdディストリビューションの時刻・タイムゾーン管理
$ timedatectl
```

## NTP
> 正確な時刻はネットワーク経由でクロックを同期するNTPを利用

```bash
$ ntpdate NTPサーバ名
```

## NTPサーバの運用

```bash
# /etc/init.d/ntpd start

# systemd
# systemctl start ntpd.service
```

 NTPサーバは `/etc/ntp.conf` で管理している
