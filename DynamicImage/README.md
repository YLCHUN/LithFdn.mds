# DynamicImage

一个iOS动态图片处理组件，支持深色模式自动切换和运行时图片动态生成。

## 功能特性

- **深色模式支持**: 根据UIUserInterfaceStyle自动切换图片资源
- **动态图片生成**: 支持运行时动态创建和修改图片
- **图片克隆**: 提供图片克隆功能，避免修改原始图片
- **动态资源检测**: 自动识别动态图片资源类型
- **原始图片提取**: 从动态图片中提取原始图片数据
- **iOS 13+兼容**: 充分利用iOS 13的新特性

## 工作原理流程图

```mermaid
flowchart TD
    A[创建ImageDynamicAsset] --> B[设置图片提供者Block]
    B --> C[注册到系统图片缓存]
    
    C --> D[等待图片请求]
    D --> E{图片请求类型}
    
    E -->|resolvedImageWithStyle| F[获取当前UI样式]
    E -->|isDynamicAssetImage| G[检测动态图片类型]
    E -->|rawImageFromDynamicAsset| H[提取原始图片数据]
    
    F --> I[检查图片缓存]
    I --> J{缓存中是否存在?}
    
    J -->|是| K[返回缓存的图片]
    J -->|否| L[调用图片提供者Block]
    
    L --> M[执行图片生成逻辑]
    M --> N{生成方式判断}
    
    N -->|从文件加载| O[加载本地图片文件]
    N -->|动态生成| P[使用Core Graphics生成]
    N -->|图片克隆| Q[克隆现有图片]
    N -->|滤镜处理| R[应用图片滤镜]
    
    O --> S[图片数据验证]
    P --> S
    Q --> S
    R --> S
    
    S --> T{图片是否有效?}
    T -->|否| U[使用兜底图片]
    T -->|是| V[图片后处理]
    
    U --> W[记录错误日志]
    V --> X[图片格式优化]
    
    W --> Y[返回兜底图片]
    X --> Z[添加到图片缓存]
    
    Z --> AA[返回生成的图片]
    K --> BB[图片使用完成]
    Y --> BB
    AA --> BB
    
    BB --> CC{是否需要释放?}
    CC -->|是| DD[从缓存中移除]
    CC -->|否| EE[保持缓存状态]
    
    DD --> FF[释放图片内存]
    EE --> GG[更新缓存状态]
    
    FF --> HH[完成图片生命周期]
    GG --> HH
    
    style A fill:#e1f5fe
    style B fill:#f3e5f5
    style C fill:#e8f5e8
    style D fill:#fff3e0
    style E fill:#fce4ec
    style F fill:#f1f8e9
    style G fill:#e0f2f1
    style H fill:#fafafa
    style I fill:#fff8e1
    style J fill:#f3e5f5
    style K fill:#e8f5e8
    style L fill:#fff3e0
    style M fill:#fce4ec
    style N fill:#f1f8e9
    style O fill:#e0f2f1
    style P fill:#fafafa
    style Q fill:#fff8e1
    style R fill:#f3e5f5
    style S fill:#e8f5e8
    style T fill:#fff3e0
    style U fill:#fce4ec
    style V fill:#f1f8e9
    style W fill:#e0f2f1
    style X fill:#fafafa
    style Y fill:#fff8e1
    style Z fill:#f3e5f5
    style AA fill:#e8f5e8
    style BB fill:#fff3e0
    style CC fill:#fce4ec
    style DD fill:#f1f8e9
    style EE fill:#e0f2f1
    style FF fill:#fafafa
    style GG fill:#fff8e1
    style HH fill:#f3e5f5
```

## 技术实现

### 核心架构
- **图片提供者模式**: 使用Block闭包作为图片提供者，支持动态生成
- **运行时图片创建**: 在运行时根据样式动态创建图片
- **图片缓存机制**: 智能缓存不同样式的图片，提升性能
- **内存管理**: 自动管理图片内存，避免内存泄漏

### 实现原理

#### 动态图片机制
1. 创建ImageDynamicAsset实例，传入图片提供者Block
2. 图片提供者Block根据UIUserInterfaceStyle返回对应图片
3. 系统自动调用resolvedImageWithStyle:方法获取当前样式图片
4. 支持图片的动态切换和缓存

#### 深色模式适配
- 监听UIUserInterfaceStyle变化
- 根据当前样式自动选择对应图片
- 支持light、dark、unspecified三种样式
- 平滑的图片切换动画

#### 图片克隆技术
- 使用Core Graphics创建图片副本
- 保持原始图片的尺寸和质量
- 支持各种图片格式（PNG、JPEG等）
- 避免修改原始图片资源

## 使用示例

### 基础深色模式支持
```objc
// 创建动态图片资源
ImageDynamicAsset *asset = [ImageDynamicAsset assetWithImageProvider:^UIImage * _Nullable(UIUserInterfaceStyle style) {
    switch (style) {
        case UIUserInterfaceStyleLight:
            return [UIImage imageNamed:@"icon_light"];
        case UIUserInterfaceStyleDark:
            return [UIImage imageNamed:@"icon_dark"];
        default:
            return [UIImage imageNamed:@"icon_default"];
    }
}];

// 使用动态图片
UIImageView *imageView = [[UIImageView alloc] init];
imageView.image = [asset resolvedImageWithStyle:UIUserInterfaceStyleUnspecified];
```

### 运行时图片生成
```objc
// 动态生成渐变图片
ImageDynamicAsset *gradientAsset = [ImageDynamicAsset assetWithImageProvider:^UIImage * _Nullable(UIUserInterfaceStyle style) {
    // 根据样式生成不同颜色的渐变
    UIColor *startColor, *endColor;
    
    if (style == UIUserInterfaceStyleDark) {
        startColor = [UIColor colorWithRed:0.2 green:0.2 blue:0.2 alpha:1.0];
        endColor = [UIColor colorWithRed:0.8 green:0.8 blue:0.8 alpha:1.0];
    } else {
        startColor = [UIColor colorWithRed:0.9 green:0.9 blue:0.9 alpha:1.0];
        endColor = [UIColor colorWithRed:0.1 green:0.1 blue:0.1 alpha:1.0];
    }
    
    return [self createGradientImageWithStartColor:startColor endColor:endColor size:CGSizeMake(100, 100)];
}];
```

### 图片克隆和修改
```objc
// 克隆现有图片
UIImage *originalImage = [UIImage imageNamed:@"avatar"];
ImageDynamicAsset *asset = [[ImageDynamicAsset alloc] initWithImageProvider:^UIImage * _Nullable(UIUserInterfaceStyle style) {
    // 克隆原始图片
    UIImage *clonedImage = [ImageDynamicAsset cloneImageFromImage:originalImage];
    
    // 根据样式添加不同的滤镜效果
    if (style == UIUserInterfaceStyleDark) {
        return [self applyDarkFilter:clonedImage];
    } else {
        return [self applyLightFilter:clonedImage];
    }
}];
```

### 动态资源检测
```objc
// 检测是否为动态图片
UIImage *image = [UIImage imageNamed:@"dynamic_icon"];
if ([ImageDynamicAsset isDynamicAssetImage:image]) {
    // 提取原始图片
    UIImage *rawImage = [ImageDynamicAsset rawImageFromDynamicAssetImage:image];
    NSLog(@"这是一个动态图片资源");
} else {
    NSLog(@"这是一个普通图片资源");
}
```

### 复杂动态图片场景
```objc
// 创建复杂的动态图片系统
ImageDynamicAsset *complexAsset = [ImageDynamicAsset assetWithImageProvider:^UIImage * _Nullable(UIUserInterfaceStyle style) {
    // 根据样式、时间、用户偏好等条件生成图片
    NSDate *now = [NSDate date];
    NSCalendar *calendar = [NSCalendar currentCalendar];
    NSInteger hour = [calendar component:NSCalendarUnitHour fromDate:now];
    
    // 根据时间和样式生成不同图片
    if (style == UIUserInterfaceStyleDark) {
        if (hour >= 6 && hour < 18) {
            return [self createDarkDayImage];
        } else {
            return [self createDarkNightImage];
        }
    } else {
        if (hour >= 6 && hour < 18) {
            return [self createLightDayImage];
        } else {
            return [self createLightNightImage];
        }
    }
}];
```

## 核心API

### 初始化方法
- `initWithImageProvider:`: 通过图片提供者Block初始化
- `assetWithImageProvider:`: 便利构造方法

### 图片解析
- `resolvedImageWithStyle:`: 根据样式解析图片
- `cloneImageFromImage:`: 克隆现有图片

### 工具方法
- `isDynamicAssetImage:`: 检测是否为动态图片
- `rawImageFromDynamicAssetImage:`: 提取原始图片

## 性能特点

- **智能缓存**: 自动缓存不同样式的图片，避免重复生成
- **延迟加载**: 按需生成图片，减少内存占用
- **图片优化**: 支持图片压缩和格式优化
- **内存管理**: 自动释放不需要的图片资源

## 适用场景

- **主题系统**: 支持深色/浅色主题切换的应用
- **动态UI**: 需要根据条件动态生成图片的界面
- **个性化**: 根据用户偏好动态调整图片样式
- **时间相关**: 根据时间、日期等条件显示不同图片
- **状态指示**: 根据应用状态动态生成状态图标

## 注意事项

- 需要iOS 13.0+支持
- 图片提供者Block会在主线程调用
- 避免在Block中执行耗时操作
- 合理使用图片缓存，避免内存占用过大

## 系统要求

- iOS 13.0+
- Xcode 11.0+
- ARC支持

## 许可证

Copyright © 2020 YLCHUN. All rights reserved.


