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
