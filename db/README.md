# 環境構築手順

## スキーマ構築手順

### oracle

1. [こちら](https://github.com/oracle-samples/db-sample-schemas/releases/tag/v19c)から`Source code(zip)`をダウンロードする

2. 解凍し、このように配置する

```
Microsoft Windows [Version 10.0.26100.3476]
(c) Microsoft Corporation. All rights reserved.

C:\Users\Syachiku>dir "C:\Oracle\product\19c\dbhome_1\demo"
 ドライブ C のボリューム ラベルがありません。
 ボリューム シリアル番号は 62E9-D9CD です

 C:\Oracle\product\19c\dbhome_1\demo のディレクトリ

2025/03/16  03:55    <DIR>          .
2025/03/16  06:03    <DIR>          ..
2025/03/16  03:55    <DIR>          db-sample-schemas-19c
2025/03/14  23:49        28,882,177 db-sample-schemas-19c.zip
2025/03/16  03:45    <DIR>          schema
               1 個のファイル          28,882,177 バイト
               4 個のディレクトリ  320,692,367,360 バイトの空き領域

C:\Users\Syachiku>
```

3. 表領域と一時表領域を作成する

```
C:\Users\Syachiku>sqlplus sys/PW#SYS as sysdba

SQL*Plus: Release 19.0.0.0.0 - Production on 土 3月 15 17:16:23 2025
Version 19.3.0.0.0

Copyright (c) 1982, 2019, Oracle.  All rights reserved.

Oracle Database 19c Enterprise Edition Release 19.0.0.0.0 - Production
Version 19.3.0.0.0
に接続されました。
SQL> CREATE TABLESPACE DEMO_TS
  2      DATAFILE 'C:\OracleDBF\DB_DEMO\DEMO.dbf' SIZE 500M
  3      AUTOEXTEND ON NEXT 125M MAXSIZE UNLIMITED
  4      EXTENT MANAGEMENT LOCAL
  5  SEGMENT SPACE MANAGEMENT AUTO;

表領域が作成されました。

SQL> CREATE TABLESPACE DEMO_TS_TEMP
  2      DATAFILE 'C:\OracleDBF\DB_DEMO\DEMO_TEMP.dbf' SIZE 50M
  3      AUTOEXTEND ON NEXT 25M MAXSIZE UNLIMITED
  4      EXTENT MANAGEMENT LOCAL
  5  SEGMENT SPACE MANAGEMENT AUTO;

表領域が作成されました。

SQL>
```

4. 下記のようにコマンドを実行する

- ファイル群配置場所へ移動する
- perl で`__SUB__CWD__`の文字列置換を行う
- sqlplus でサンプルスキーマを構築

```
Microsoft Windows [Version 10.0.26100.3476]
(c) Microsoft Corporation. All rights reserved.

C:\Users\Syachiku>cd "C:/Oracle/product/19c/dbhome_1/demo/db-sample-schemas-19c"

C:\Oracle\product\19c\dbhome_1\demo\db-sample-schemas-19c>perl -CSAD -p -i.bak -e "s#__SUB__CWD__#"C:/Oracle/product/19c/dbhome_1/demo/db-sample-schemas-19c"#g" *.sql */*.sql */*.dat

C:\Oracle\product\19c\dbhome_1\demo\db-sample-schemas-19c>sqlplus system/PW#SYSTEM@noodle:1521/orcl

SQL*Plus: Release 19.0.0.0.0 - Production on 日 3月 16 03:55:51 2025
Version 19.3.0.0.0

Copyright (c) 1982, 2019, Oracle.  All rights reserved.

最終正常ログイン時間: 日 3月  16 2025 02:02:16 +09:00


Oracle Database 19c Enterprise Edition Release 19.0.0.0.0 - Production
Version 19.3.0.0.0
に接続されました。
SQL> @"C:/Oracle/product/19c/dbhome_1/demo/db-sample-schemas-19c/mksample.sql" PW#SYS PW#SYSTEM hrPW##1234 oePW##1234 pmPW##1234 ixPW##1234 shPW##1234 biPW##1234 DEMO_TS DEMO_TS_TEMP "C:/Oracle/product/19c/dbhome_1/demo/db-sample-schemas-19c/log/" noodle:1521/orcl

specify password for SYSTEM as parameter 1:

specify password for SYS as parameter 2:

specify password for HR as parameter 3:

～～以下略～～
```
