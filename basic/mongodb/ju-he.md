# 聚合

Aggregation operations process **data records** and return **computed results**。

Aggregation operations group values from multiple documents together, and can perform a variety of operations on the grouped data to return a single result.

MongoDB provides three ways to perform aggregation: **the aggregation pipeline**, **the map-reduce function**, and **single purpose aggregation methods**.

## SQL to Aggregation Mapping

| SQL | mongo shell |
| :--- | :--- |
| WHERE | $match |
| GROUP BY | $group |
| HAVING | $match |
| SELECT | $project |
| ORDER BY | $sort |
| LIMIT | $limit |
| SUM\(\) | $sum |
| COUNT\(\) | $sum |
| join | $lookup |

