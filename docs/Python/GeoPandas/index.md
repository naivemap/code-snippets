---
comments: true
tags:
  - GIS
---

# GeoPandas

[GeoPandas](https://geopandas.org/en/stable/index.html) is an open source project to make working with geospatial data in python easier.

## 按属性分割

```python
gdf = geopandas.read_file("path/to/file.shp")
for cnty_code, sub in gdf.groupby("cnty_code"):
    out_path = f"code_{str(cnty_code)}.csv"
    sub.drop(columns=["geometry"]).sort_values(by="code").to_csv(out_path, index=False, encoding="utf-8")
```
