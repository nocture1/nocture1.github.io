# ウインドウ関数

## ウインドウ関数とは
行数を減らさず、グルーピングだけする機能。  
サブクエリよりも高効率な行間比較を行える。

## ミニマル - 平均値よりも低いデータを取得する

```sql
select student_id,
       weight
  from (select student_id,
               weight,
               avg(weight) over() as avg_weight
          from students) TMP
 where weight < avg_weight;
```

> 💡  
> `OVER` に何も指定しない場合、全行に渡って集約を行う。

### サブクエリとの比較
- テーブルスキャンの回数が減る
- コードが煩雑になる
- from 区内のクエリを抜き出すことでデバッグがやりやすい

## PARTITION BY - グループごとの平均値より低いデータを取得する

上の例について、 `dpt` ごとの平均値より低いデータを取得する。

```sql
select student_id,
       weight
  from (select student_id,
               weight,
               avg(weight) over(partition by dpt) as avg_weight
          from students) TMP
 where weight < avg_weight;
```

> 💡  
> `PARTITION BY` で指定したキーごとに集約を行う。

## 合わせ技- グループ内の最小の枝番を持つデータを取得する

```sql
select student_id,
       index
  from (select student_id,
               row_number() over(partition by dpt order by submit_date) as index
          from students) TMP
 where index = 1
```

## ORDER BY - 移動累積を求める

```sql
select sales_date,
       sales_amt,
       acm_amt
  from (select sales_date,
               sales_amt,
               sum(sales_amt) over(order by sales_date) as acm_amt
          from sales) TMP;
```

> 💡  
> `ORDER BY` で指定したキーごとにソートをかける。  
> これにより、 `SUM` の対象を制御できる。

## フレーム句 - 移動平均を求める

```sql
select sales_date,
       sales_amt,
       avg_amt
  from (select sales_date,
               sales_amt,
               avg(sales_amt) over(order by sales_date rows between 2 preceding and current rows) as avg_amt
          from sales) TMP;
```

> 💡  
> `rows` 以降で集約対象の行を制御できる。

### 補足：集約行に満たない場合はNULLにする

```sql
select sales_date,
       sales_amt,
       avg_amt
  from (select sales_date,
               sales_amt,
               case when count(*) over(order by sales_date) < 3 then null
                    else avg(sales_amt) over(order by sales_date rows between 2 preceding and current row) as avg_amt
          from sales) TMP;
```

> 💡  
> `rows between n preceding and current row` は `LAG(列名, n)` で簡潔に記述できる。

## 合わせ技 - グループごとに移動平均を求める

```sql
select sales_date,
       sales_amt,
       avg_amt
  from (select sales_date,
               sales_amt,
               avg(sales_amt) over(partition by dept order by sales_date rows between 2 preceding and current row) as avg_amt
          from sales) TMP;
```

## 合わせ技 - 直前行との増減を比較する

```sql
select sales_date,
       sales_amt,
       case when diff > 0 then '↑'
            when diff = 0 then '→'
            when diff < 0 then '↓'
            else null end as trend
  from (select sales_date,
               sales_amt,
               coalesce(sign(sales_amt - max(sales_amt) over(order by sales_date) rows between 1 preceding and 1 preceding), 0) as diff
          from sales) TMP;
```

## 合わせ技 - 行間比較のNULLを無視する
各列の値のうち、 `level` が最も高い行にある値を取得する。（列の折り畳み）

```sql
select max(color_max),
       max(length_max)
  from (select first_value(color) ignore nulls over(order by level desc) as color_max,
               first_value(length) ignore nulls over(order by level desc) as length_max
          from elements) TMP;
```

> 💡  
> `IGNORE NULLS` を有効にした場合、行間比較中のNULLは無視する。