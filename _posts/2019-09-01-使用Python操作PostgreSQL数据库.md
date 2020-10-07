---
title: 使用Python操作PostgreSQL数据库
date: 2019-09-01 20:03:32 +0800
categories: [学习总结, 基础知识]
tags: [Python]
---
使用Python操作PostgreSQL数据库。
***
<h4>建表时增加筛选条件</h4>


```sql
SELECT p.gsn
, case
	when p.enddate > fs.akitime - interval'1' day then ROUND(extract(epoch from (fs.akitime - interval'1' day-p.startdate)): : integer / 60 / 60 / 24)+1
    else ROUND(extract(epoch from (p.enddate-p.startdate))::integer / 60 / 60 / 24)
	end as len
FROM mimiciii.prescriptions p
INNER JOIN mimiciii.focustime fs
ON p.icustay_id = fs.icustay_id AND fs.icustay_id = 200422 AND p.startdate > fs.intime AND p.startdate < fs.akitime


DROP MATERIALIZED VIEW IF EXISTS focustime CASCADE;
CREATE materialized VIEW focustime AS

with foo as
(
    SELECT ks.icustay_id, id.intime, id.outtime   	, case
   	when ks.lowcreat7daytime >= ks.highcreat7daytime then ks.lowcreat7daytime
   	else ks.highcreat7daytime
   	end as akitime
    from mimiciii.kdigo_stages_7day ks
    INNER JOIN mimiciii.icustay_detail id
    ON id.icustay_id=ks.icustay_id
)
select *
from foo
order by foo.icustay_id
```


***
<h4>使用Python操作数据库</h4>

```Python
import psycopg2
conn = psycopg2.connect(dbname="mimic", user="postgres",
                        password="postgres", host="127.0.0.1", port="5432")
cur = conn.cursor()

cur.execute("   SELECT p.GSN  FROM mimiciii.prescriptions p  GROUP BY p.GSN")
combination = cur.fetchall()
```

***
**参考：**
1. https://blog.csdn.net/u011304970/article/details/72771775

