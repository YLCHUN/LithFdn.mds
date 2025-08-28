# Decimal

数字单位转换和格式化工具，支持中文单位显示和多种舍入模式。提供Objective-C和Swift两种语言实现版。

## 功能特性

- 支持大数字的单位转换（个、十、百、千、万、十万、百万、千万、亿）
- 多种小数舍入模式（四舍五入、向下取整、向上取整）
- 灵活的小数位数控制
- 支持中文单位名称显示
- 整数部分位数控制，可省略小数部分

## 主要组件

### DigitalUnits

数字单位枚举，支持位运算组合：

```objc
typedef NS_OPTIONS(long, DigitalUnits) {
    DigitalUnits1 = 1<<0,         // 个
    DigitalUnits10 = 1<<1,        // 十
    DigitalUnits100 = 1<<2,       // 百
    DigitalUnits1000 = 1<<3,      // 千
    DigitalUnits10000 = 1<<4,     // 万
    DigitalUnits100000 = 1<<5,    // 十万
    DigitalUnits1000000 = 1<<6,   // 百万
    DigitalUnits10000000 = 1<<7,  // 千万
    DigitalUnits100000000 = 1<<8, // 亿
};
```

### DigitalRoundingMode

舍入模式枚举：

```objc
typedef NS_ENUM(NSUInteger, DigitalRoundingMode) {
    DigitalRoundPlain = 0,   // 四舍五入（小数点后第1位）
    DigitalRoundDown = 1,    // 向下取值
    DigitalRoundUp = 2,      // 向上取值
    DigitalRoundPlain2 = 3,  // 四舍五入（小数点后第2位）
    DigitalRoundPlain3 = 4,  // 四舍五入（小数点后第3位）
    DigitalRoundPlain4 = 5,  // 四舍五入（小数点后第4位）
};
```

### DecimalOpt

格式化选项结构体：

```objc
typedef struct DecimalOpt {
    short places;                    // 小数保留位数
    DigitalRoundingMode mode;        // 舍入模式
    short ipddp;                     // 整数部分位数阈值，超过时省略小数
} DecimalOpt;
```

## 使用示例

```objc
// 创建 Decimal 实例
Decimal *decimal = [[Decimal alloc] initWithInteger:1234567 units:DigitalUnitsBigNumber];

// 基本格式化
DecimalOpt opt = DecimalOptMake(2, DigitalRoundPlain, 4);
NSString *result = [decimal toStringWithOpt:opt];
// 输出：123.46万

// 使用中文单位名称
NSString *resultCN = [decimal toStringWithOpt:opt unitNames:[Decimal unitNamesCN]];
// 输出：123.46万
```

## 核心方法

- `initWithInteger:units:` - 初始化 Decimal 实例
- `toStringWithOpt:` - 根据选项格式化输出
- `toStringWithOpt:unitNames:` - 使用自定义单位名称格式化输出

## 版本信息

- 创建时间：2024年9月26日
- 作者：Cityu
