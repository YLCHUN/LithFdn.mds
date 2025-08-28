# GridStaggered

一个功能完善iOS网格交错布局组件，支持多种错位类型、旋转角度和自定义布局参数，适用于创建独特的网格视觉效果。

## 功能特性

- **多种错位类型**：支持正常、列错位、行错位三种布局模式
- **旋转支持**：可设置整体网格的旋转角度
- **灵活配置**：支持自定义间距、尺寸和错位偏移量
- **高性能**：基于 UICollectionView 实现，支持大量元素的高效渲染
- **易于使用**：简单的API接口，支持数据绑定回调

## 工作原理流程图

```mermaid
flowchart TD
    A[初始化GridStaggeredView] --> B[设置布局参数]
    B --> C[配置错位类型]
    C --> D[设置旋转角度]
    D --> E[计算网格尺寸]
    
    E --> F[创建UICollectionView]
    F --> G[设置数据源和代理]
    G --> H[注册网格单元格类]
    
    H --> I[调用reloadData]
    I --> J[布局引擎开始工作]
    
    J --> K[计算可见区域]
    K --> L[遍历所有网格项]
    
    L --> M{错位类型判断}
    M -->|正常| N[标准网格布局计算]
    M -->|列错位| O[列错位布局计算]
    M -->|行错位| P[行错位布局计算]
    
    N --> Q[计算标准行列位置]
    O --> R[计算列错位偏移量]
    P --> S[计算行错位偏移量]
    
    Q --> T[应用旋转角度]
    R --> T
    S --> T
    
    T --> U[生成布局属性]
    U --> V[设置位置和尺寸]
    V --> W[应用旋转变换]
    
    W --> X[创建布局属性对象]
    X --> Y[添加到布局数组]
    
    Y --> Z{是否还有更多项?}
    Z -->|是| L
    Z -->|否| AA[完成布局计算]
    
    AA --> BB[UICollectionView应用布局]
    BB --> CC[渲染可见网格]
    CC --> DD[复用不可见网格]
    
    DD --> EE[完成网格渲染]
    
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
```

## 核心组件

### GridStaggeredView
主要的视图组件，继承自 UIView，内部使用 UICollectionView 实现网格布局。

### GridStaggeredLayout
布局计算核心，负责生成每个网格元素的位置、尺寸和旋转角度等属性。

### GridStaggeredLayoutAttributes
布局属性对象，包含每个网格的位置、尺寸和旋转信息。

## 使用方法

### 基本使用

```objc
// 创建视图
GridStaggeredView *staggeredView = [[GridStaggeredView alloc] initWithFrame:self.view.bounds];

// 设置网格类型和数据绑定
[staggeredView setGridClass:[UICollectionViewCell class] dataBinding:^(UIView *grid, NSUInteger index) {
    // 配置每个网格的内容
    grid.backgroundColor = [UIColor colorWithHue:index * 0.1 saturation:0.8 brightness:0.8 alpha:1.0];
}];

// 配置布局参数
GridStaggeredLayout *layout = staggeredView.layout;
layout.itemSize = CGSizeMake(60, 60);
layout.itemSpacing = 10;
layout.lineSpacing = 10;
layout.angle = 15; // 15度旋转
layout.staggerOffset = 20; // 错位偏移量
layout.staggeredType = GridStaggeredTypeColumn; // 列错位

// 添加到父视图
[self.view addSubview:staggeredView];

// 重新加载数据
[staggeredView reloadData];
```

### 布局参数说明

- `itemSize`: 网格元素尺寸
- `itemSpacing`: 网格元素之间的水平间距
- `lineSpacing`: 网格行之间的垂直间距
- `angle`: 整体网格的旋转角度（度）
- `staggerOffset`: 错位偏移量
- `staggeredType`: 错位类型
  - `GridStaggeredTypeNormal`: 正常网格
  - `GridStaggeredTypeColumn`: 列错位
  - `GridStaggeredTypeRow`: 行错位

## 错位类型效果

### 正常网格 (GridStaggeredTypeNormal)
标准的网格布局，所有元素按行列整齐排列。

### 列错位 (GridStaggeredTypeColumn)
相邻列之间产生垂直错位，创建波浪般的视觉效果。

### 行错位 (GridStaggeredTypeRow)
相邻行之间产生水平错位，创建锯齿状的视觉效果。

## 性能优化

- 使用 UICollectionView 的复用机制，支持大量元素的高效渲染
- 智能的可见区域计算，只渲染可见的网格元素
- 自动布局更新，支持动态尺寸变化

## 系统要求

- iOS 9.0+
- Xcode 10.0+
- Objective-C

## 许可证

Copyright © 2024 YLCHUN/Cityu. All rights reserved.
