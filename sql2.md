# CASE式

## GROUP BY句に使用 - コード体系の変換

```sql
select case when pref_name = '愛媛' then '四国'
            when pref_name = '高知' then '四国'
            when pref_name = '鹿児島' then '九州'
            else 'その他' end as district,
       pop
  from populations
 group by case when pref_name = '愛媛' then '四国'
               when pref_name = '高知' then '四国'
               when pref_name = '鹿児島' then '九州'
               else 'その他' end as district;
```

> ⚡  
> SQL標準の書き方では正しいが、  
> DBによってはCASE式はSELECT句だけに書いても動作する。

## ORDER句に使用 - その場限りのソートキー
テーブルにソート用のキー値がない状態での独自ソートを行う。

```sql
-- 愛媛⇒高知⇒順不同　の順番で表示する
select pref_name,
       pop
  from populations
 order by case when pref_name = '愛媛' then 1
               when pref_name = '高知' then 2
               else 0 end;
```

## 集計関数に使用 - 表側・表頭レイアウト
クロス集計表のようにデータを集計する。

```sql
select pref_name,
       sum(case when sex = '1' then pop else 0 end) as pop_1,
       sum(case when sex = '2' then pop else 0 end) as pop_2,
  from populations
 group by pref_name;
```

## 集約関数に使用 - 集約したデータに適用
1つだけのクラブに所属している学生はそのクラブIDを、  
掛け持ちしている場合は主なクラブIDを求める。

```sql
select club_id,
       case when count(*) = 1 then max(club_id)
            else max(case when main_club_flg = 'Y'
                          then club_id
                          else NULL end)
  from students
 group by student_id;
```

> 💡  
> `count(*)` は `HAVING` 句で表現するが、  
> このように `SELECT` 句に記述できる。

## 集約関数に使用 - 全称量子化として
生徒全員がレポートを提出した学部を求める。

```sql
select *
  from students
 group by dept
having count(*) = sum(case when submit_date is not null
                           then 1
                           else 0 end);
```
