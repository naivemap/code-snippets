# Performance

## 生成随机整数

### Math.floor

```js
const i = Math.floor(Math.random() * 100);
```

### 位运算

```js
const i = (Math.random() * 100) | 0;
```

!!! info "提示"
	`| 0 `这个技巧是一个性能优化的方案，比使用 `Math.floor()` 要快。适合在在性能关键的场景使用，如：游戏渲染循环、大量数据处理等。

### 更快原因

#### 底层实现差异

  1. 位运算 | 0：
    1. 直接转换为 CPU 的一条位运算指令
    2. 操作数直接在 CPU 寄存器中完成
    3. 不需要函数调用开销
  2. Math.floor()
    1. 需要函数调用开销  
    2. 需要处理各种边界情况（如 NaN、Infinity 等）

#### 操作步骤对比

```js
// Math.floor() 的过程
let x = 3.7;
Math.floor(x);  // 涉及以下步骤：
// 1. 查找 Math 对象
// 2. 调用 floor 函数
// 3. 创建调用栈
// 4. 执行函数逻辑
// 5. 返回结果

// | 0 的过程
let x = 3.7;
x | 0;  // 直接进行位运算，一步到位
```

### 限制和注意事项

```js
// 1. 32位整数范围限制
console.log(2147483647 | 0);      // 2147483647 (最大值)
console.log(2147483648 | 0);      // -2147483648 (溢出)

// 2. 不支持大数处理
console.log(Math.floor(Number.MAX_SAFE_INTEGER));  // 9007199254740991
console.log(Number.MAX_SAFE_INTEGER | 0);          // -1 (错误结果)

// 3. 只能用于数字类型
const num = "3.14" | 0;     // 可以工作
const invalid = "abc" | 0;  // 结果为 0，可能不是期望的行为
```

### 最佳实践

```js
// 推荐的使用场景
function fastIntegerConversion(value: number): number {
    // 当确定数值在 32 位整数范围内时使用 | 0
    if (value > -2147483648 && value < 2147483647) {
        return value | 0;
    }
    // 其他情况使用 Math.floor
    return Math.floor(value);
}
```

在实际开发中，除非在性能关键的场景（如游戏渲染循环、大量数据处理等），否则可读性通常比这种微优化更重要。
