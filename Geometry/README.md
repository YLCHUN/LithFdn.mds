# Geometry 几何图形绘制组件

一个iOS几何图形绘制工具集合，提供气泡贝塞尔曲线和多边形贝塞尔路径的创建功能，适用于自定义UI绘制和图形设计。

## 工作原理流程图

```mermaid
flowchart TD
    A[输入几何参数] --> B{图形类型判断}
    
    B -->|气泡形状| C[BubbleBezierPath处理]
    B -->|多边形| D[UIBezierPath+Polygon处理]
    
    C --> E[设置矩形区域和圆角]
    E --> F[计算气泡角位置]
    F --> G[确定气泡角朝向点]
    
    G --> H[计算气泡角法线]
    H --> I[设置气泡角角度]
    I --> J[配置气泡角圆角]
    
    J --> K[生成气泡贝塞尔路径]
    K --> L[计算控制点]
    L --> M[绘制矩形主体]
    
    M --> N[绘制气泡角]
    N --> O[应用圆角处理]
    O --> P[完成气泡路径]
    
    D --> Q[设置多边形顶点]
    Q --> R[配置顶点圆角半径]
    R --> S[计算顶点连接线]
    
    S --> T{是否启用圆角自适应?}
    T -->|是| U[计算最优圆角半径]
    T -->|否| V[使用指定圆角半径]
    
    U --> W[验证圆角半径有效性]
    V --> W
    
    W --> X[生成多边形贝塞尔路径]
    X --> Y[绘制顶点连接线]
    Y --> Z[应用圆角处理]
    
    Z --> AA[闭合多边形路径]
    AA --> BB[完成多边形路径]
    
    P --> CC[路径验证和优化]
    BB --> CC
    
    CC --> DD{路径是否有效?}
    DD -->|否| EE[路径错误处理]
    DD -->|是| FF[路径后处理]
    
    EE --> GG[记录错误日志]
    FF --> HH[路径平滑处理]
    
    GG --> II[返回错误状态]
    HH --> JJ[路径缓存优化]
    
    JJ --> KK[返回贝塞尔路径对象]
    II --> LL[使用默认路径]
    
    KK --> MM[完成几何图形绘制]
    LL --> MM
    
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
    style II fill:#e8f5e8
    style JJ fill:#fff3e0
    style KK fill:#fce4ec
    style LL fill:#f1f8e9
    style MM fill:#e0f2f1
```

## 组件列表

### [BubbleBezierPath](./BubbleBezierPath.h)
一个专门用于创建气泡形状贝塞尔曲线的工具类，支持自定义气泡角的位置、角度和圆角半径。

#### 主要功能
- 支持两种气泡角构建方式：基于角度和基于切线长度
- 可自定义矩形区域的圆角半径
- 支持气泡角朝向点的精确定位
- 可控制气泡角的法线高度和夹角角度
- 支持气泡角本身的圆角处理

#### 使用示例
```objc
// 创建引导气泡
CGPoint orientation = CGPointMake(115, 90);
BubbleBezierPath *path = [BubbleBezierPath bezierPathWithRect:CGRectMake(100, 200, 200, 100) 
                                                       corner:10 
                                                 orientation:orientation 
                                                       normal:10 
                                                        degree:90 
                                                        corner:0];
```

### [UIBezierPath+Polygon](./UIBezierPath+Polygon.h)
UIBezierPath的多边形扩展，支持创建带圆角的闭合多边形贝塞尔路径。

#### 主要功能
- 支持任意顶点数量的多边形创建
- 每个顶点可独立设置圆角半径
- 提供圆角半径自适应功能
- 使用PolygonVertex结构体定义顶点信息

#### 使用示例
```objc
// 创建类似专辑封面的遮罩形状
CGRect rect = CGRectMake(0, 0, 100, 100);
CGFloat minX = CGRectGetMinX(rect);
CGFloat maxX = CGRectGetMaxX(rect);
CGFloat minY = CGRectGetMinY(rect);
CGFloat maxY = CGRectGetMaxY(rect);

CGPoint lt = CGPointMake(minX, minY);
CGPoint rt = CGPointMake(maxX, minY);
CGPoint rb = CGPointMake(maxX, maxY);
CGPoint lb = CGPointMake(minX, maxY);

CGFloat rRadius = 8;
CGPoint p = CGPointMake(maxX-10, minY + (maxY-minY)/2.0);
CGPoint pt = p;
pt.x = maxX;
pt.y -= 10;

CGPoint pb = p;
pb.x = maxX;
pb.y += 10;

PolygonVertex vs[7];
vs[0] = PolygonVertextMake(lt.x, lt.y, rRadius);
vs[1] = PolygonVertextMake(rt.x, rt.y, rRadius);
vs[2] = PolygonVertextMake(pt.x, pt.y, rRadius);
vs[3] = PolygonVertextMake(p.x, p.y, rRadius);
vs[4] = PolygonVertextMake(pb.x, pb.y, rRadius);
vs[5] = PolygonVertextMake(rb.x, rb.y, rRadius);
vs[6] = PolygonVertextMake(lb.x, lb.y, rRadius);

UIBezierPath *path = [UIBezierPath bezierPathWithPolygonVertexs:vs count:7 cornerAdaption:YES];
```

## 安装使用

将Geometry文件夹添加到你的iOS项目中，确保包含所有.h和.m文件。

## 系统要求

- iOS 8.0+
- Xcode 8.0+
- Objective-C

## 许可证

Copyright © 2024 YLCHUN/Cityu. All rights reserved.
