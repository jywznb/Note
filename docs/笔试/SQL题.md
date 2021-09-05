### 最近连续三周活跃用户数

1）建表语句

```
hive (gmall)>
drop table if exists ads_continuity_wk_count;
create external table ads_continuity_wk_count( 
    `dt` string COMMENT '统计日期,一般用结束周周日日期,如果每天计算一次,可用当天日期',
    `wk_dt` string COMMENT '持续时间',
    `continuity_count` bigint COMMENT '活跃用户数'
) COMMENT '最近连续三周活跃用户数'
row format delimited fields terminated by '\t'
location '/warehouse/gmall/ads/ads_continuity_wk_count';
```

2）导入2020-06-25所在周的数据

```
hive (gmall)>
insert into table ads_continuity_wk_count
select
    '2020-06-25',
    concat(date_add(next_day('2020-06-25','MO'),-7*3),'_',date_add(next_day('2020-06-25','MO'),-1)),
    count(*)
from
(
    select
        mid_id
    from
    (
        select
            mid_id
        from dws_uv_detail_daycount
        where dt>=date_add(next_day('2020-06-25','monday'),-7)
        and dt<=date_add(next_day('2020-06-25','monday'),-1)
        group by mid_id

        union all

        select
            mid_id
        from dws_uv_detail_daycount
        where dt>=date_add(next_day('2020-06-25','monday'),-7*2)
        and dt<=date_add(next_day('2020-06-25','monday'),-7-1)
        group by mid_id

        union all

        select
            mid_id
        from dws_uv_detail_daycount
        where dt>=date_add(next_day('2020-06-25','monday'),-7*3)
        and dt<=date_add(next_day('2020-06-25','monday'),-7*2-1)
        group by mid_id
    )t1
    group by mid_id
    having count(*)=3
)t2;
```

3）查询

```
hive (gmall)> select * from ads_continuity_wk_count;
```



### 7.2.8 最近七天内连续三天活跃用户数

1）建表语句

```
hive (gmall)>
drop table if exists ads_continuity_uv_count;
create external table ads_continuity_uv_count( 
    `dt` string COMMENT '统计日期',
    `wk_dt` string COMMENT '最近7天日期',
    `continuity_count` bigint
) COMMENT '最近七天内连续三天活跃用户数'
row format delimited fields terminated by '\t'
location '/warehouse/gmall/ads/ads_continuity_uv_count';
```

2）写出导入数据的SQL语句

```
hive (gmall)>
insert into table ads_continuity_uv_count
select
    '2020-06-16',
    concat(date_add('2020-06-16',-6),'_','2020-06-16'),
    count(*)
from
(
    select mid_id
    from
    (
        select mid_id
        from
        (
            select 
                mid_id,
                date_sub(dt,rank) date_dif
            from
            (
                select
                    mid_id,
                    dt,
                    rank() over(partition by mid_id order by dt) rank
                from dws_uv_detail_daycount
                where dt>=date_add('2020-06-16',-6) and dt<='2020-06-16'
            )t1
        )t2 
        group by mid_id,date_dif
        having count(*)>=3
    )t3 
    group by mid_id
)t4;
```

3）查询

```
hive (gmall)> select * from ads_continuity_uv_count;
```

https://blog.csdn.net/he_wen_jie/article/details/104839346

