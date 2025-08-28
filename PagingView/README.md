# PagingView

一个功能完善的iOS分页视图组件，提供灵活的分页计算和分页滚动视图实现，支持自定义分页大小、前后缀设置和智能分页逻辑。

## 功能特性

- **智能分页计算**: 自动计算分页索引和偏移量
- **灵活的前后缀支持**: 支持设置分页内容的前缀和后缀
- **多方向滚动**: 支持水平和垂直方向的分页滚动
- **高性能代理**: 使用代理模式优化滚动性能
- **精确的偏移计算**: 基于滑动速率和分页速度的智能偏移计算

## 工作原理流程图

```mermaid
flowchart TD
    A[初始化Paging组件] --> B[设置视图尺寸参数]
    B --> C[配置分页大小]
    C --> D[设置前后缀偏移量]
    
    D --> E[调用calculatePagingIfNeed]
    E --> F[验证参数有效性]
    
    F --> G{参数是否有效?}
    G -->|否| H[返回计算失败]
    G -->|是| I[计算分页数量]
    
    I --> J[计算前缀偏移量]
    J --> K[计算后缀偏移量]
    K --> L[计算主体分页数量]
    
    L --> M[生成分页索引映射]
    M --> N[完成分页计算]
    
    N --> O[用户开始滚动]
    O --> P[监听滚动事件]
    
    P --> Q[计算当前偏移量]
    Q --> R[调用indexWithContentOffset]
    
    R --> S[确定当前分页索引]
    S --> T{是否为分页边界?}
    
    T -->|是| U[触发分页切换事件]
    T -->|否| V[继续滚动监听]
    
    U --> W[计算目标偏移量]
    W --> X[应用分页动画]
    
    X --> Y[更新分页状态]
    Y --> Z[完成分页切换]
    
    V --> P
    Z --> P
    
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
```

## 核心组件

### Paging
分页计算的核心类，负责处理分页逻辑和索引计算。

#### 主要属性
- `viewSize`: 视图大小
- `contentSize`: 内容大小
- `pagingSize`: 分页大小
- `prefixInset`: 前缀偏移量（只读）
- `suffixInset`: 后缀偏移量（只读）

#### 核心方法
```objc
// 设置分页前后缀
- (void)setContentInsetPrefix:(CGFloat)prefix suffix:(CGFloat)suffix;

// 计算分页
- (BOOL)calculatePagingIfNeed;

// 通过索引计算内容偏移量
- (CGFloat)contentOffsetWithIndex:(PagingIndex)index;

// 通过内容偏移量计算索引
- (PagingIndex)indexWithContentOffset:(CGFloat)contentOffset;

// 计算索引偏移结果
- (PagingIndex)calculateIndex:(PagingIndex)index offset:(NSInteger)offset;

// 计算目标偏移量
- (CGFloat)targetOffsetWithCurrent:(CGFloat)currentOffset 
                      proposed:(CGFloat)proposedOffset 
              scrollingVelocity:(CGFloat)scrollingVelocity 
                 pagingVelocity:(CGFloat)pagingVelocity;
```

### PagingScrollView
继承自 UIScrollView 的分页滚动视图，支持水平和垂直方向的分页滚动。

#### 主要属性
- `pagingSize`: 分页大小
- `isHorizontalDirection`: 是否为水平方向

### PagingCollectionView
继承自 UICollectionView 的分页集合视图，专门为集合视图提供分页功能。

#### 主要属性
- `pagingSize`: 分页大小

## 使用示例

### 基本分页设置
```objc
Paging *paging = [[Paging alloc] init];
paging.viewSize = 300.0;        // 视图宽度
paging.contentSize = 1500.0;    // 内容总宽度
paging.pagingSize = 300.0;      // 每页宽度

// 设置前后缀
[paging setContentInsetPrefix:50.0 suffix:50.0];

// 计算分页
if ([paging calculatePagingIfNeed]) {
    // 分页计算成功
}
```

### 分页滚动视图使用
```objc
PagingScrollView *scrollView = [[PagingScrollView alloc] init];
scrollView.pagingSize = CGSizeMake(300, 200);
scrollView.isHorizontalDirection = YES;

// 设置代理
scrollView.delegate = self;
```

### 分页集合视图使用
```objc
PagingCollectionView *collectionView = [[PagingCollectionView alloc] init];
collectionView.pagingSize = CGSizeMake(300, 200);

// 设置布局和代理
UICollectionViewFlowLayout *layout = [[UICollectionViewFlowLayout alloc] init];
layout.itemSize = CGSizeMake(300, 200);
layout.scrollDirection = UICollectionViewScrollDirectionHorizontal;
collectionView.collectionViewLayout = layout;
```

## 分页索引类型

```objc
typedef NS_ENUM(NSInteger, PagingIndexType) {
    PagingPrefixIndex = -1,    // 前缀索引
    PagingBodyIndex = 0,       // 主体索引
    PagingSuffixIndex = 1      // 后缀索引
};

typedef struct {
    NSInteger index;           // 索引值
    PagingIndexType type;      // 索引类型
} PagingIndex;
```

## 注意事项

1. 分页大小必须大于0才能正常计算
2. 视图大小和内容大小必须大于0
3. 前后缀设置会影响分页计算逻辑
4. 使用代理模式时注意内存管理

## 系统要求

- iOS 8.0+
- Xcode 8.0+
- Objective-C

## 许可证

Copyright © 2024 YLCHUN/Cityu. All rights reserved.
