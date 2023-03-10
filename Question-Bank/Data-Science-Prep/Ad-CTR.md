## Problem
This problem was asked by Facebook.

Assume you have the below events table on app analytics. Write a query to get the click-through rate per app in 2019.

**events**

| column_name |              type              |
|:-----------:|:------------------------------:|
|    app_id   |             integer            |
|   event_id  | string ("impression", "click") |
|  timestamp  |            datetime            |

## Solution
To get the click-through rate, we can calculate the total sum of clicks and the total number of impressions, and then divide the two. The CASE statement comes in handy here, so we can use the following query:

```
SELECT app_id,
    SUM(CASE WHEN event_id = ‘click’ THEN 1 ELSE 0 END) /
    SUM(CASE WHEN event_id = ‘impression’ THEN 1 ELSE 0 END) AS ctr
FROM events
    WHERE timestamp >= ‘2019-01-01’
GROUP BY 1
```


###MS SQL
```
with cte as 
    (select 
        app_id, 
        CAST(SUM(case when event_id='click' then 1 end) AS decimal(4,2)) as click,
        count(event_id) as total
    from events
    group by app_id)
select app_id, round((click/total),2) as click_through_rate from cte;
```