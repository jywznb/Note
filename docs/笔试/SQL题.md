# 最近七天连续三天登陆用户数量

```
select
	mid_id
from(
	select
		mid_id
	from(
		select
			mid_id,date_sub(dt,rank) date_dif
		from(
			select 
				mid_id, dt,
				rank() over(partition by mid_id order by dt) rank
			from dws_uv_detail_day
			where dt>=date_add('2021-03-26',-6) and dt<='2021-03-26' 
			) t1
		) t2
	group by mid_id,date_dif
	having count(*) > 3
	) t3
group by mid_id;
```

https://blog.csdn.net/he_wen_jie/article/details/104839346

