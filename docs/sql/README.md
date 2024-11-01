## 実行計画
```sql
# Oracle
SET AUTOTRACE TRACEONLY

# PostgreSQL
EXPLAIN <SQL文>

# MySQL
EXPLAIN ANALYZE <SQL文>
```

# CASE式
## 基本情報
- SQLにおける条件分岐
- 検索CASE式を使う

```sql
CASE WHEN CONDITION
     WHEN CONDITION
     ...
     ELSE NULL
```

- 常に `ELSE` を書くこと
  + 明示的に `NULL` を返すことを表明する

## CASEのユースケース
- ラベルの張替え
- 行列の変換（PIVOT）
- ad hocなソートキー・集約キーを作る
- 値をスワップする

