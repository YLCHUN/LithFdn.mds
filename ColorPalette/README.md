# ColorPalette

一个功能完善的iOS颜色调色板框架，提供完整的颜色处理、转换和调色功能。

## 功能特性

- 🎨 **HSL调色器** - 基于ALSH（Alpha, Lightness, Saturation, Hue）颜色空间的调色功能
- 🔄 **颜色转换** - 支持RGB、HSL、ABGR、ALSH等多种颜色格式之间的转换
- 🎯 **智能调色** - 提供兜底色机制和智能颜色调制
- 🧮 **颜色计算** - 支持颜色混合、叠加、渐变等高级操作
- 📱 **iOS原生** - 完全基于UIKit，专为iOS应用设计

## 项目结构

```
ColorPalette/
├── ColorPalette.h/.m          # 核心调色协议和基础实现
├── ColorALSH.h                # ALSH颜色空间协议定义
├── ColorALSHPalette.h/.m      # HSL调色器实现
└── ColorUtil.h/.m             # 颜色工具函数集合
```

## 核心组件

### 1. ColorPalette
基础调色协议，定义了调色器的核心接口：

```objc
@protocol ColorPalette <NSObject>
- (UIColor *)palette:(UIColor *)color;
@end
```

主要功能：
- 颜色兜底处理
- 基础调色操作

### 2. ColorALSH
ALSH颜色空间协议，定义了Alpha、Lightness、Saturation、Hue四个通道：

```objc
@protocol ColorALSH <NSObject>
@property (nonatomic, strong, nullable) NSNumber *H; // 0~360
@property (nonatomic, strong, nullable) NSNumber *L; // 0~100
@property (nonatomic, strong, nullable) NSNumber *S; // 0~100
@property (nonatomic, strong, nullable) NSNumber *A; // 0~100
@end
```

### 3. ColorALSHPalette
HSL调色器的具体实现，继承自ColorPalette并实现ColorALSH协议：

- **ALSH调制**：`modulatedALSH:` 方法
- **智能调色**：支持JavaScript风格的调色脚本
- **工具方法**：最近值查找、值范围限制等

### 4. ColorUtil
丰富的颜色工具函数集合：

#### 颜色格式转换
- `UIColor_to_abgr()` / `abgr_to_UIColor()`
- `UIColor_to_alsh()` / `alsh_to_UIColor()`
- `abgr_to_alsh()` / `alsh_to_abgr()`

#### 颜色操作
- **混合**：`abgr_mix()` / `UIColor_mix()`
- **叠加**：`abgr_filter()` / `UIColor_filter()`
- **渐变**：`abgr_gradation()` / `UIColor_gradation()`
- **中位色**：`abgr_median()` / `UIColor_median()`

## 使用示例

### 基础调色
```objc
// 创建HSL调色器
ColorALSHPalette *palette = [ColorALSHPalette palette:^(id<ColorALSH> alsh) {
    alsh.H = @30;           // 设置色相为30度
    alsh.L = @60;           // 设置亮度为60%
    alsh.S = @80;           // 设置饱和度为80%
    alsh.A = @100;          // 设置透明度为100%
}];

// 应用调色
UIColor *originalColor = [UIColor redColor];
UIColor *adjustedColor = [palette palette:originalColor];
```

### JavaScript风格调色
```objc
// 使用JavaScript风格的调色脚本
NSString *script = @"if (a = 10) { l = clampValue(l, 20, 60); s = nearestValue(s, [10, 20, 30]); h = 30; }";
id<ColorALSH>(^paletteBlock)(id<ColorALSH>) = [ColorALSHPalette jsPaletteContext:script];

// 应用调色
id<ColorALSH> alsh = /* 获取当前颜色的ALSH值 */;
id<ColorALSH> result = paletteBlock(alsh);
```

### 颜色转换
```objc
// RGB转ALSH
UIColor *rgbColor = [UIColor blueColor];
uint32_t alshValue = UIColor_to_alsh(rgbColor);

// ALSH转RGB
UIColor *convertedColor = alsh_to_UIColor(alshValue);
```

### 颜色混合
```objc
// 混合两个颜色
UIColor *color1 = [UIColor redColor];
UIColor *color2 = [UIColor blueColor];
UIColor *mixedColor = UIColor_mix(color1, color2);

// 创建渐变
UIColor *gradientColor = UIColor_gradation(color1, 0.5, color2);
```

## 安装要求

- iOS 8.0+
- Xcode 8.0+
- Objective-C

## 使用方法

1. 将项目文件添加到您的iOS项目中
2. 导入需要的头文件
3. 根据需要创建调色器实例
4. 调用相应的方法进行颜色处理

## 注意事项

- ALSH颜色空间：A(0-100), L(0-100), S(0-100), H(0-360)
- ABGR颜色空间：A(0-255), B(0-255), G(0-255), R(0-255)
- 所有颜色操作都支持透明度通道
- JavaScript调色脚本需要遵循特定语法规则

## 许可证

Copyright © 2024 YLCHUN. All rights reserved.

## 贡献

欢迎提交Issue和Pull Request来改进这个项目。

## 更新日志

- **2024/3/15** - 添加ColorALSH协议
- **2024/3/8** - 创建基础ColorPalette框架
- **2024/3/7** - 项目初始化
