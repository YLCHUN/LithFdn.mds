# PerformDelay

NSObject 的延迟执行扩展工具，支持 Selector 和 Block 两种方式，自动管理内存和取消操作。

## 功能特性

- 支持 Selector 和 Block 两种延迟执行方式
- 自动内存管理，延迟期间对象不会被持有
- 支持 RunLoop 模式控制
- 自动取消机制，对象销毁时自动清理
- 支持通过 key 标识的 Block 执行

## 工作原理流程图

```mermaid
flowchart TD
    A[调用延迟执行方法] --> B{执行方式判断}
    
    B -->|Selector方式| C[调用pd_performSelector:withObject:afterDelay:]
    B -->|Block方式| D[调用pd_performBlock:afterDelay:block:]
    
    C --> E[创建延迟任务对象]
    D --> E
    
    E --> F[设置延迟时间]
    F --> G[设置RunLoop模式]
    G --> H[创建定时器]
    
    H --> I[将任务添加到任务队列]
    I --> J[启动定时器倒计时]
    
    J --> K[等待延迟时间结束]
    K --> L{延迟时间是否到达?}
    
    L -->|否| K
    L -->|是| M[准备执行任务]
    
    M --> N{任务是否已被取消?}
    N -->|是| O[忽略已取消的任务]
    N -->|否| P[检查对象是否有效]
    
    P --> Q{对象是否仍然存在?}
    Q -->|否| R[对象已销毁，取消执行]
    Q -->|是| S[执行任务]
    
    S --> T{执行方式判断}
    T -->|Selector| U[调用performSelector:withObject:]
    T -->|Block| V[执行Block回调]
    
    U --> W[任务执行完成]
    V --> W
    
    W --> X[从任务队列中移除]
    X --> Y[清理定时器资源]
    Y --> Z[完成延迟执行]
    
    O --> AA[清理已取消任务]
    R --> AA
    
    AA --> BB[等待下次延迟执行]
    Z --> BB
    
    BB --> CC[监听取消请求]
    CC --> DD{是否收到取消请求?}
    
    DD -->|是| EE[执行取消操作]
    DD -->|否| FF[继续等待]
    
    EE --> GG[停止定时器]
    GG --> HH[从队列中移除任务]
    HH --> II[释放任务资源]
    
    II --> JJ[取消完成]
    FF --> CC
    
    JJ --> CC
    
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
```

## 主要方法

### Selector 延迟执行

```objc
- (void)pd_performSelector:(SEL)aSelector 
                withObject:(id)anArgument 
                afterDelay:(NSTimeInterval)delay;

- (void)pd_performSelector:(SEL)aSelector 
                withObject:(id)anArgument 
                afterDelay:(NSTimeInterval)delay 
                   inModes:(NSArray<NSRunLoopMode> *)modes;
```

### Block 延迟执行

```objc
- (void)pd_performBlock:(NSString *)key 
              afterDelay:(NSTimeInterval)delay 
                   block:(void(^)(id sender))block;

- (void)pd_performBlock:(NSString *)key 
              afterDelay:(NSTimeInterval)delay 
                 inModes:(NSArray<NSRunLoopMode> *)modes 
                   block:(void(^)(id sender))block;
```

### 取消操作

```objc
- (void)pd_cancelPreviousPerformRequests;
- (void)pd_cancelPreviousPerformRequestsWithSelector:(SEL)aSelector 
                                              object:(id)anArgument;
- (void)pd_cancelPreviousPerformRequestsWithBlock:(NSString *)key;
```

## 使用示例

```objc
// Selector 延迟执行
[self pd_performSelector:@selector(doSomething) 
              withObject:nil 
              afterDelay:2.0];

// Block 延迟执行
[self pd_performBlock:@"myTask" 
           afterDelay:3.0 
                block:^(id sender) {
    NSLog(@"延迟任务执行");
}];

// 取消特定任务
[self pd_cancelPreviousPerformRequestsWithBlock:@"myTask"];

// 取消所有延迟任务
[self pd_cancelPreviousPerformRequests];
```

## 核心特性

### 内存安全
- 延迟执行期间，当前对象不会被持有
- 避免循环引用和内存泄漏问题

### 自动清理
- 对象销毁时自动执行 cancel 操作
- 无需手动管理延迟任务的清理

### RunLoop 模式支持
- 支持指定 RunLoop 模式执行
- 适用于不同场景下的任务调度

## 注意事项

- 使用 Block 方式时，建议使用有意义的 key 标识
- 在对象销毁前主动取消不需要的延迟任务
- 支持嵌套调用，但要注意避免过度嵌套

## 版本信息

- 创建时间：2023年8月8日
- 作者：Cityu
