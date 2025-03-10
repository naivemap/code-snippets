# TiTiler

[TiTiler](https://developmentseed.org/titiler/): A modern dynamic tile server built on top of FastAPI and Rasterio/GDAL.

## /cog

### colormap

#### 离散

```js
const colormap = encodeURIComponent(
  JSON.stringify({
    0: "#f00",
    1: "#0f0",
    2: "#00f",
  })
);
```

#### 线性

```js
// 从 0 到 255 的 256 个值
const linearColors: Record<number, string> = {};
for (let i = 0; i < 256; i++) {
  colormap[i] = `#${i.toString(16).padStart(2, "0")}0000`;
}

colormap = encodeURIComponent(JSON.stringify(linearColors));
```

#### 区间

```js
const colormap = JSON.stringify([
  // [[min, max], [r, g, b, a]]
  [[0, 40], "#FF0000"],
  [
    [40, 200],
    [0, 255, 0, 255],
  ],
  [
    [200, 255],
    [0, 0, 255, 255],
  ],
]);
```
