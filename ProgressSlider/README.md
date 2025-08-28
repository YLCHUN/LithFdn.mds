# ProgressSlider

一个功能完善的iOS进度滑块组件，支持双进度显示、拖拽交互和丰富的自定义选项。

## 功能特性

- **双进度显示**: 支持当前进度和预加载进度的同时显示
- **交互支持**: 支持拖拽和点击两种交互方式
- **自定义外观**: 可自定义进度条高度、圆角、滑块大小等
- **事件回调**: 提供丰富的UIControl事件和代理方法
- **触摸监控**: 集成TouchMonitor实现精确的触摸事件处理
- **布局灵活**: 支持自定义进度条区域和事件响应区域

## 工作原理流程图

```mermaid
flowchart TD
    A[初始化ProgressSlider] --> B[创建子视图组件]
    B --> C[设置默认属性]
    C --> D[配置TouchMonitor]
    
    D --> E[等待用户交互]
    E --> F{交互类型判断}
    
    F -->|触摸开始| G[TouchMonitor开始监控]
    F -->|触摸移动| H[计算触摸位置]
    F -->|触摸结束| I[TouchMonitor结束监控]
    
    G --> J[记录触摸起始位置]
    H --> K[计算拖拽偏移量]
    I --> L[处理触摸结束事件]
    
    J --> M[更新交互状态]
    K --> N[计算新进度值]
    L --> O[触发进度改变事件]
    
    M --> P{是否为拖拽操作?}
    N --> Q{进度值是否有效?}
    O --> R[更新UI显示]
    
    P -->|是| S[启用拖拽模式]
    P -->|否| T[启用点击模式]
    
    Q -->|是| U[调用代理验证]
    Q -->|否| V[忽略无效进度]
    
    S --> W[更新滑块位置]
    T --> X[计算点击位置对应进度]
    
    U --> Y{代理是否允许?}
    V --> Z[保持原进度值]
    
    Y -->|允许| AA[更新当前进度]
    Y -->|拒绝| BB[恢复原进度值]
    
    AA --> CC[触发UIControlEventValueChanged]
    BB --> DD[触发UIControlEventEditingChanged]
    
    CC --> EE[更新进度条显示]
    DD --> FF[更新滑块位置]
    
    EE --> GG[完成进度更新]
    FF --> GG
    
    GG --> E
    Z --> E
    
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
```

## 主要组件

- `backgroundView`: 背景进度条视图
- `curProgressView`: 当前进度视图
- `preProgressView`: 预加载进度视图
- `dotView`: 可拖拽的滑块视图

## API 接口

### 属性

```objc
// 代理对象
@property (nonatomic, weak) id<ProgressSliderDelegate> delegate;

// 外观设置
@property (nonatomic, assign) BOOL progressRounded;        // 进度条圆角，默认YES
@property (nonatomic, assign) CGSize dotSize;              // 滑块大小，默认进度条高度*2

// 进度控制
@property (nonatomic, assign) CGFloat curProgress;          // 当前进度 (0.0-1.0)
@property (nonatomic, assign) CGFloat preProgress;          // 预加载进度 (0.0-1.0)

// 交互控制
@property (nonatomic, assign) BOOL progressDragEnabled;     // 是否允许拖拽，默认YES

// 状态查询
@property (nonatomic, readonly) BOOL tracking;              // 是否正在拖拽
@property (nonatomic, readonly) BOOL taping;                // 是否正在点击
@property (nonatomic, readonly) BOOL editing;               // 是否正在编辑
@property (nonatomic, readonly) BOOL interactingChanged;    // 是否由内部交互产生变化
@property (nonatomic, readonly) CGFloat editingTranslation; // 编辑时的偏移量
```

### 代理方法

```objc
@protocol ProgressSliderDelegate <NSObject>
@optional
// 进度改变前的验证
- (BOOL)progressSlider:(ProgressSlider *)progressSlider shouldChangeCurProgress:(CGFloat)progress;
@end
```

### 事件

- `UIControlEventValueChanged`: 进度值改变时触发
- `UIControlEventEditingChanged`: 编辑状态改变时触发

### 核心方法

```objc
// 初始化完成后的设置
- (void)didInitialize;

// 获取进度条区域
- (CGRect)progressRect;

// 根据进度值获取对应区域
- (CGRect)rectWithProgress:(CGFloat)progress size:(CGSize)size;

// 获取滑块事件响应区域
- (CGRect)dotEventFrame;
```

## 使用示例

### 基本使用

```objc
ProgressSlider *slider = [[ProgressSlider alloc] initWithFrame:CGRectMake(20, 100, 280, 40)];
slider.curProgress = 0.3;
slider.preProgress = 0.6;
slider.progressRounded = YES;
slider.dotSize = CGSizeMake(20, 20);

[slider addTarget:self action:@selector(sliderValueChanged:) forControlEvents:UIControlEventValueChanged];
[self.view addSubview:slider];
```

### 代理使用

```objc
slider.delegate = self;

- (BOOL)progressSlider:(ProgressSlider *)progressSlider shouldChangeCurProgress:(CGFloat)progress {
    // 验证进度值是否允许改变
    return progress >= 0.0 && progress <= 1.0;
}
```

### 自定义样式

```objc
// 设置进度条颜色
slider.backgroundView.backgroundColor = [UIColor lightGrayColor];
slider.curProgressView.backgroundColor = [UIColor blueColor];
slider.preProgressView.backgroundColor = [UIColor grayColor];
slider.dotView.backgroundColor = [UIColor whiteColor];

// 设置圆角
slider.backgroundView.layer.cornerRadius = 5;
slider.curProgressView.layer.cornerRadius = 5;
slider.preProgressView.layer.cornerRadius = 5;
slider.dotView.layer.cornerRadius = 10;
```

## 依赖

- iOS 8.0+
- TouchMonitor

## 许可证

Copyright © 2023 YLCHUN. All rights reserved.
