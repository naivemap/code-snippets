---
comments: true
tags:
  - GIS
---

# [ST_AsImage](https://help.aliyun.com/zh/polardb/polardb-for-postgresql/st-asimage?spm=a2c4g.11186623.help-menu-2249963.d_8_5_3_2.435d1dc7c8te2K)

将栅格对象转化为影像格式二进制流。

```sql
SELECT
	encode(
		ST_AsImage (
			rast,
			'(140.025,60.025),(69.975,-0.025)' :: box
			0,
			'0',
			'jpeg',
			'{"strength": "ratio", "quality": 70}'
		),
		'hex'
	)
FROM
	surf_hor_metadata
WHERE
	ele = 'PRE'
```
