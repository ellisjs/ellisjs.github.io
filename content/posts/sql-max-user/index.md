+++
date = '2024-10-09T21:44:09+08:00'
draft = false
title = 'SQL面试题：统计直播间最大用户数'
categories = ["面试"]
tags = ["SQL"]
+++
要统计直播间同时最多用户数，可以通过以下步骤实现：

## 1.数据准备
假设表名为 live_visits，包含以下字段：

user_id：用户ID

enter_time：用户进入时间

leave_time：用户离开时间

## 2.思路
统计同时在线用户数的峰值，可以通过以下步骤：

将进入时间和离开时间合并为一个时间线。

对每个时间点，统计在线用户数。

找出最大值。

## 3.SQL实现

```SQL
WITH time_points AS (
    SELECT enter_time AS time_point, 1 AS change
    FROM live_visits
    UNION ALL
    SELECT leave_time AS time_point, -1 AS change
    FROM live_visits
),
online_users AS (
    SELECT time_point, 
           SUM(change) OVER (ORDER BY time_point) AS current_users
    FROM time_points
)
SELECT MAX(current_users) AS max_concurrent_users
FROM online_users;
```

## 4.解释
time_points：将进入和离开时间合并，change为1表示用户进入，-1表示用户离开。

online_users：通过窗口函数 SUM(change) OVER (ORDER BY time_point) 计算每个时间点的在线用户数。

MAX(current_users)：找出在线用户数的最大值。

## 5.结果
该查询返回直播间同时在线用户数的峰值。