# PostgreSQL

## 修改字段类型

```sql
-- 不带转换
ALTER TABLE table_name ALTER COLUMN column_name TYPE new_type;

-- 带转换
ALTER TABLE table_name ALTER COLUMN column_name TYPE new_type USING column_name::new_type;
```