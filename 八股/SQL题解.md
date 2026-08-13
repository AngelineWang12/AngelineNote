# SQL题解

## 按月统计交易信息

题目 SQL：

```sql
SELECT DATE_FORMAT(trans_date, '%Y-%m') AS month,
    country,
    COUNT(*) AS trans_count,
    COUNT(IF(state = 'approved', 1, NULL)) AS approved_count,
    SUM(amount) AS trans_total_amount,
    SUM(IF(state = 'approved', amount, 0)) AS approved_total_amount
FROM Transactions
GROUP BY month, country;
```

### 题目思路

1. 先用 `DATE_FORMAT(trans_date, '%Y-%m')` 把交易日期转成“年-月”。
2. 再按 `month` 和 `country` 分组。
3. 每个分组内分别统计总交易数、通过交易数、总金额、通过金额。

### 为什么 `COUNT(*)` 统计的是每个 group 的数量

因为这条 SQL 使用了 `GROUP BY month, country`，所以数据会先按照 “月份 + 国家” 分成多个组。

聚合函数在存在 `GROUP BY` 时，都是对**每个分组**分别计算，而不是对整张表计算。

所以：

- `COUNT(*)`：统计当前分组中的总行数
- `SUM(amount)`：统计当前分组中的总金额
- `COUNT(IF(...))`：统计当前分组中满足条件的行数

如果没有 `GROUP BY`，那么 `COUNT(*)` 统计的才是整张表的总行数。

### 字段解释

- `DATE_FORMAT(trans_date, '%Y-%m') AS month`
  - 将日期格式化为 `2026-03` 这种“年-月”格式。

- `COUNT(*) AS trans_count`
  - 统计当前分组内总共有多少条交易记录。

- `COUNT(IF(state = 'approved', 1, NULL)) AS approved_count`
  - `IF(state = 'approved', 1, NULL)` 表示：
  - 满足条件返回 `1`
  - 不满足条件返回 `NULL`
  - `COUNT(expr)` 只统计非 `NULL` 值，所以它最终统计的是通过的交易条数。

- `SUM(amount) AS trans_total_amount`
  - 当前分组内所有交易金额求和。

- `SUM(IF(state = 'approved', amount, 0)) AS approved_total_amount`
  - 通过的交易取 `amount`
  - 未通过的交易取 `0`
  - 最后求和，得到通过交易的总金额。

### 常见聚合函数总结

#### 1. `COUNT(*)`

- 统计行数
- 有 `GROUP BY` 时，统计每个分组的行数
- 没有 `GROUP BY` 时，统计整张表的总行数

示例：

```sql
SELECT country, COUNT(*)
FROM Transactions
GROUP BY country;
```

#### 2. `COUNT(column)`

- 统计某列中**非 `NULL`** 的数量
- 如果该列为 `NULL`，这一行不会被统计

示例：

```sql
SELECT COUNT(amount)
FROM Transactions;
```

#### 3. `SUM(column)`

- 对某列求和
- 常用于金额、数量、积分等字段

示例：

```sql
SELECT SUM(amount)
FROM Transactions;
```

#### 4. `AVG(column)`

- 求平均值

示例：

```sql
SELECT AVG(amount)
FROM Transactions;
```

#### 5. `MAX(column)` / `MIN(column)`

- 求最大值 / 最小值

示例：

```sql
SELECT MAX(amount), MIN(amount)
FROM Transactions;
```

### 条件聚合写法

条件统计：

```sql
COUNT(IF(state = 'approved', 1, NULL))
```

条件求和：

```sql
SUM(IF(state = 'approved', amount, 0))
```

这类写法本质上都是先通过 `IF` 把数据变成目标值，再交给聚合函数处理。

### 一句话总结

`GROUP BY` 是先分组，`COUNT`、`SUM`、`AVG` 这些聚合函数再对每个分组分别计算。

## 查找员工主部门

题目 SQL：

```sql
WITH t AS (
    SELECT employee_id,
        department_id,
        primary_flag,
        COUNT(*) OVER(PARTITION BY employee_id) AS count_over
    FROM employee
)
SELECT employee_id, department_id
FROM t
WHERE count_over = 1 OR primary_flag = 'Y';
```

### 题目思路

- `COUNT(*) OVER(PARTITION BY employee_id)`：统计每个员工有几条部门记录，但**不合并行**。
- `count_over = 1`：说明该员工只属于一个部门，直接返回。
- `primary_flag = 'Y'`：如果员工属于多个部门，返回主部门。
 **窗口函数这句怎么拆开理解**

`count(*) over(partition by employee_id)`

可以拆成：

- count(*)：统计数量
- over(...)：说明这是窗口函数
- partition by employee_id：按员工编号分窗口

### 一句话总结

- 窗口函数 `OVER(PARTITION BY ...)` 是“分组统计但保留明细行”。
- `GROUP BY` 是“分组后聚合，明细行会被合并”。

## CASE WHEN 用法

示例：

```sql
SELECT x, y, z,
    CASE
        WHEN x + y > z AND x + z > y AND y + z > x THEN 'Yes'
        ELSE 'No'
    END AS triangle
FROM triangle;
```

### 一句话理解

- `CASE WHEN` 就是 SQL 里的条件判断，类似 `if ... else`。
- 条件满足走 `THEN`，否则走 `ELSE`，最后用 `END` 结束。

### 常见写法

条件判断型：

```sql
CASE
    WHEN 条件1 THEN 结果1
    WHEN 条件2 THEN 结果2
    ELSE 默认结果
END
```

等值匹配型：

```sql
CASE 表达式
    WHEN 值1 THEN 结果1
    WHEN 值2 THEN 结果2
    ELSE 默认结果
END
```

### 常见用途

- 字段映射：状态码转文本
- 条件统计：`SUM(CASE WHEN 条件 THEN 1 ELSE 0 END)`
- 条件求和：`SUM(CASE WHEN 条件 THEN amount ELSE 0 END)`
- 自定义排序：`ORDER BY CASE WHEN ... END`
