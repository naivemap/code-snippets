---
comments: true
tags:
  - GIS
---

# PostGIS

## 安装

```sql
CREATE SCHEMA postgis;
GRANT USAGE ON schema postgis to public;

CREATE EXTENSION postgis SCHEMA postgis;
CREATE EXTENSION postgis_raster SCHEMA postgis;

ALTER DATABASE postgis_in_action SET search_path=public,postgis,contrib;
```

!!! info "提示"
虽然不是必需的，但建议在单独的模式（如 postgis）中安装 postgis，这样函数就不会弄乱默认的公共模式。

## 查询数据库下所有空间表

```sql
WITH columns AS (
	SELECT
		ns.nspname AS table_schema, -- 模式
		cls.relname AS table_name, -- 表名
		cls.reltuples AS tuples, -- 记录数
		des.description AS description, -- 表描述
		attr.attname AS column_name, -- 列名
		col_description(attr.attrelid, attr.attnum) AS column_comment, -- 列描述
		trim(leading '_' from tp.typname) AS column_type -- 列类型
	FROM pg_catalog.pg_attribute attr
		JOIN pg_catalog.pg_class AS cls ON cls.oid = attr.attrelid
		JOIN pg_catalog.pg_namespace AS ns ON ns.oid = cls.relnamespace
		JOIN pg_catalog.pg_type AS tp ON tp.oid = attr.atttypid
		JOIN pg_catalog.pg_description AS des ON cls.oid = des.objoid AND des.objsubid = 0
	WHERE
		NOT attr.attisdropped
		AND attr.attnum > 0
		AND cls.relname NOT LIKE 'pg_%'
		AND ns.nspname NOT IN ('pg_catalog', 'information_schema')
		-- AND ns.nspname = 'other'
		-- AND cls.relname like 'flash_flood_hazard_point'
)
SELECT
	f_table_name as id,
	f_table_schema as workspace,
	f_table_name as data_name,
	columns.description as description,
	columns.tuples as tuples,
	f_geometry_column as geometry_column,
	type as geometry_type,
	srid,
	jsonb_object_agg(
		columns.column_name,
		json_build_object(
			'type',columns.column_type,
			'title',columns.column_comment
		)
	) AS fields
FROM postgis.geometry_columns as gc
	JOIN columns ON
		gc.f_table_schema = columns.table_schema AND
		gc.f_table_name = columns.table_name AND
		gc.f_geometry_column != columns.column_name
GROUP BY f_table_schema, f_table_name, description, tuples, f_geometry_column, srid, type

```
