# 查詢

Read operations retrieves documents from a collection.

## 1. 查詢操作符

### 比較

按照 偏序關係 + 包含 + 非

| Name | Description |
| :--- | :--- |
| $eq | Matches values that are equal to a specified value. |
| $gt | Matches values that are greater than a specified value. |
| $gte | Matches values that are greater than or equal to a specified value. |
| $in | Matches any of the values specified in an array. |
| $lt | Matches values that are less than a specified value. |
| $lte | Matches values that are less than or equal to a specified value. |
| $ne | Matches all values that are not equal to a specified value. |
| $nin | Matches none of the values specified in an array. |

### 元素

| Name | Description |
| :--- | :--- |
| $exists | Matches documents that have the specified field. |
| $type | Selects documents if a field is of the specified type. |

### 邏輯

| Name | Description |
| :--- | :--- |
| $and | Joins query clauses with a logical AND returns all documents that match the conditions of both clauses. |
| $not | Inverts the effect of a query expression and returns documents that do not match the query expression. |
| $nor | Joins query clauses with a logical NOR returns all documents that fail to match both clauses. |
| $or | Joins query clauses with a logical OR returns all documents that match the conditions of either clause. |

## 2. 基礎查詢

格式：

* 值。{ : , ... }
* 操作符的值。{ : { :  }, ... }

| Mongo | SQL |
| :--- | :--- |
| db.inventory.find\( {} \) | SELECT \* FROM inventory |
| db.inventory.find\( { status: "D" } \) | SELECT \* FROM inventory WHERE status = "D" |
| db.inventory.find\( { status: { $in: \[ "A", "D" \] } } \) | SELECT \* FROM inventory WHERE status in \("A", "D"\) |
| db.inventory.find\( { status: "A", qty: { $lt: 30 } } \) | SELECT \* FROM inventory WHERE status = "A" AND qty &lt; 30 |
| db.inventory.find\( { $or: \[ { status: "A" }, { qty: { $lt: 30 } } \] } \) | SELECT \* FROM inventory WHERE status = "A" OR qty &lt; 30 |
| db.inventory.find\( { status: "A",$or: \[ { qty: { $lt: 30 } }, { item: /^p/ } \]} \) | SELECT \* FROM inventory WHERE status = "A" AND \( qty &lt; 30 OR item LIKE "p%"\) |

## 3. 查詢嵌套的文檔

```text
# 查詢整個文檔，要求完全匹配
db.inventory.find( { size: { h: 14, w: 21, uom: "cm" } } )
db.inventory.find(  { size: { w: 21, h: 14, uom: "cm" } }  )

# 點查詢
db.inventory.find( { "size.uom": "in" } )
db.inventory.find( { "size.h": { $lt: 15 } } )
db.inventory.find( { "size.h": { $lt: 15 }, "size.uom": "in", status: "D" } )
```

