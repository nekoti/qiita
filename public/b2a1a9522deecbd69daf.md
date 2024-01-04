---
title: 'Oracle実験室 / Oracle Database 19c (19.3) 各種環境設定 '
tags:
  - Linux
  - CentOS
  - oracle
  - VirtualBox
private: false
updated_at: '2024-01-03T13:03:09+09:00'
id: b2a1a9522deecbd69daf
organization_url_name: null
slide: false
ignorePublish: false
---
# 前書き
- 記事一覧 [Oracle実験室 / Oracle データベース各種操作](https://qiita.com/Que-sera-sera/items/862c77963b841a95ba95)

# プラガブルデータベース操作
- 状態表示
<font color="blue">OPEN MODEがMONTEDで停止している.</font>
```SQL
$ sqlplus / as sysdba
SQL> show pdbs

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         2 PDB$SEED                       READ ONLY  NO
         3 ORCLPDB1                       MOUNTED
```
- 起動
```SQL
SQL> alter pluggable database ORCLPDB1 open;

プラガブル・データベースが変更されました。
```
- 状態表示
<font color="blue">OPEN MODEがREAD WRITEとなり起動完了.</font>
```SQL
SQL> show pdbs

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         2 PDB$SEED                       READ ONLY  NO
         3 ORCLPDB1                       READ WRITE NO
```
- 永続設定
```SQL
SQL> alter pluggable database ORCLPDB1 save state;

プラガブル・データベースが変更されました。
```

# HRスキーマ有効化
[【参考文献】2 サンプル・スキーマのインストール](https://docs.oracle.com/cd/E82638_01/comsc/installing-sample-schemas.html#GUID-CB945E4C-D08A-4B26-A12D-3D6D688467EA)
- スキーマ作成
```SQL
SQL> @?/demo/schema/human_resources/hr_main.sql

specify password for HR as parameter 1:
1に値を入力してください: ********

specify default tablespeace for HR as parameter 2:
2に値を入力してください: users

specify temporary tablespace for HR as parameter 3:
3に値を入力してください: temp

specify log path as parameter 4:
4に値を入力してください: $ORACLE_HOME/demo/schema/log/


PL/SQLプロシージャが正常に完了しました。


ユーザーが作成されました。

...


Commit complete.


PL/SQL procedure successfully completed.
```
- 確認
```SQL
$ sqlplus hr/PASSWORD@localhost:1521/ORCLPDB1
SQL> select table_name from user_tables;

TABLE_NAME
--------------------------------------------------------------------------------
COUNTRIES
REGIONS
LOCATIONS
DEPARTMENTS
JOBS
EMPLOYEES
JOB_HISTORY

7行が選択されました。
```
