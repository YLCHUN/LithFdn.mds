# InvokeMap

一个轻量级的Objective-C函数调用映射框架，支持多上下文函数注册、异步调用和优先级排序。

## 功能特性

- 🔗 **函数映射**: 支持一个函数名对应多个上下文处理器
- 🚀 **异步支持**: 支持同步和异步函数调用
- 📊 **优先级排序**: 通过协议实现调用上下文的优先级排序
- 🎯 **灵活注册**: 支持Block和Selector两种注册方式
- 💾 **内存管理**: 使用弱引用避免循环引用问题
- 🔄 **回调机制**: 支持异步回调处理

## 系统要求

- iOS 8.0+ / macOS 10.10+
- Xcode 8.0+
- Objective-C

## 安装

将以下文件添加到您的项目中：
- `InvokeMap.h`
- `InvokeMap.m`

## 使用方法

### 基本用法

```objc
#import "InvokeMap.h"

// 创建InvokeMap实例
InvokeMap<UIViewController *> *invokeMap = [[InvokeMap alloc] init];

// 注册函数处理器
[invokeMap setFunc:@"handleData" 
               ctx:viewController 
           handler:^(UIViewController *ctx, NSDictionary *args, InvokeMapCallback callback) {
    // 处理逻辑
    NSLog(@"处理数据: %@", args);
    callback(@{@"result": @"success"});
} 
            async:NO];

// 调用函数
[invokeMap invokeFunc:@"handleData" 
                  args:@{@"key": @"value"} 
                  ctxId:nil 
                  async:NO 
               callback:^(NSDictionary *params) {
    NSLog(@"回调结果: %@", params);
}];
```

### 使用Selector注册

```objc
// 在ViewController中实现方法
- (void)handleDataWithParam:(NSDictionary *)args callback:(InvokeMapCallback)callback {
    // 处理逻辑
    callback(@{@"status": @"completed"});
}

// 注册Selector
[invokeMap setFunc:@"handleData" 
               ctx:viewController 
               sel:@selector(handleDataWithParam:callback:) 
            async:NO];
```

### 异步调用

```objc
// 异步调用
[invokeMap invokeFunc:@"handleData" 
                  args:@{@"async": @YES} 
                  ctxId:nil 
                  async:YES 
               callback:^(NSDictionary *params) {
    dispatch_async(dispatch_get_main_queue(), ^{
        // 更新UI
        NSLog(@"异步处理完成: %@", params);
    });
}];
```

### 优先级排序

```objc
@interface MyContextSorter : NSObject <InvokeMapCtxSort>
@end

@implementation MyContextSorter

- (NSArray<InvokeMapCtxId *> *)sortCtxs:(NSArray<InvokeMapCtxId *> *)ctxs 
                                    func:(InvokeMapFuncName *)func 
                                    args:(NSDictionary *)args {
    // 实现自定义排序逻辑
    return [ctxs sortedArrayUsingComparator:^NSComparisonResult(InvokeMapCtxId *obj1, InvokeMapCtxId *obj2) {
        // 根据业务逻辑排序
        return [obj1 compare:obj2];
    }];
}

@end

// 设置排序器
MyContextSorter *sorter = [[MyContextSorter alloc] init];
invokeMap.ctxSort = sorter;
```

## API 参考

### InvokeMap 类

#### 属性

- `ctxSort`: 上下文排序器，实现 `InvokeMapCtxSort` 协议

#### 方法

##### 注册函数

```objc
// 使用Block注册
- (BOOL)setFunc:(InvokeMapFuncName *)func 
             ctx:(CtxType)ctx 
         handler:(InvokeMapHandler)handler 
            async:(BOOL)async;

// 使用Selector注册
- (BOOL)setFunc:(NSString *)func 
             ctx:(id)ctx 
             sel:(SEL)sel 
            async:(BOOL)async;
```

##### 调用函数

```objc
- (NSUInteger)invokeFunc:(InvokeMapFuncName *)func 
                    args:(NSDictionary *)args 
                    ctxId:(InvokeMapCtxId *)ctxId 
                    async:(BOOL)async 
                 callback:(InvokeMapCallback)callback;
```

##### 工具方法

```objc
+ (InvokeMapCtxId *)ctxId:(id)ctx;
```

### 类型定义

```objc
typedef NSString InvokeMapFuncName;        // 函数名类型
typedef NSString InvokeMapCtxId;          // 上下文ID类型

typedef void(^InvokeMapCallback)(NSDictionary *_Nullable args);           // 回调Block
typedef void(^InvokeMapHandler)(CtxType ctx, NSDictionary *_Nullable args, InvokeMapCallback callback); // 处理器Block
```

### InvokeMapCtxSort 协议

```objc
@protocol InvokeMapCtxSort <NSObject>

- (NSArray<InvokeMapCtxId *> *)sortCtxs:(NSArray<InvokeMapCtxId *> *)ctxs 
                                    func:(InvokeMapFuncName *)func 
                                    args:(NSDictionary *_Nullable)args;

@end
```

## 使用场景

- **模块间通信**: 实现松耦合的模块间函数调用
- **插件系统**: 支持动态注册和调用插件功能
- **事件处理**: 多处理器的事件分发系统
- **中间件**: 实现函数调用的拦截和排序

## 注意事项

1. **内存管理**: 框架使用弱引用管理上下文对象，避免循环引用
2. **线程安全**: 当前版本不支持多线程并发访问，请在主线程使用
3. **函数名**: 函数名不能为空字符串
4. **上下文对象**: 上下文对象不能为nil

## 许可证

本项目采用 MIT 许可证。

## 作者

Created by Cityu on 2023/8/31
