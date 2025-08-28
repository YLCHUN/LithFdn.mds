# ViewHidden

一个功能完善的 iOS 视图隐藏管理组件，提供基于键值对的视图隐藏状态管理，支持多状态控制、KVO 监听和智能隐藏逻辑。解决到处Hidden导致的结果混乱。

## 功能特性

- **多状态管理**: 支持通过不同的 key 来管理视图的隐藏状态
- **智能隐藏逻辑**: 提供 `onlyKey` 机制，支持独占式隐藏控制
- **KVO 支持**: 自动监听视图的 hidden 属性变化，支持双向绑定
- **内存安全**: 使用弱引用避免循环引用，自动管理内存
- **简单易用**: 通过 UIView 分类提供便捷的访问方式

## 安装使用

将 `ViewHidden.h` 和 `ViewHidden.m` 文件添加到你的项目中，并确保项目中包含 `KVOController` 依赖。

## API 说明

### ViewHidden 类

#### 属性

- `hiddenKeys` (readonly): 当前所有隐藏状态的键值集合
- `hidden` (readonly): 视图是否应该被隐藏
- `onlyKey`: 独占键值，设置后只有该键值可以控制隐藏状态

#### 方法

- `setHidden:forKey:`: 为指定键值设置隐藏状态
- `hiddenForKey:`: 获取指定键值的隐藏状态

### UIView 分类

- `hiddenBridge`: 获取视图的隐藏管理桥接对象

## 使用示例

### 基本使用

```objc
// 获取视图的隐藏管理对象
ViewHidden *hiddenManager = self.someView.hiddenBridge;

// 设置不同状态的隐藏
[hiddenManager setHidden:YES forKey:@"loading"];
[hiddenManager setHidden:YES forKey:@"error"];
[hiddenManager setHidden:NO forKey:@"content"];

// 检查特定状态的隐藏
BOOL isLoadingHidden = [hiddenManager hiddenForKey:@"loading"];
```

### 独占模式使用

```objc
// 设置独占键值，只有该键值可以控制隐藏状态
hiddenManager.onlyKey = @"maintenance";

// 此时其他键值的设置不会影响视图的隐藏状态
[hiddenManager setHidden:YES forKey:@"loading"]; // 不会生效
[hiddenManager setHidden:YES forKey:@"maintenance"]; // 会生效
```

### 监听隐藏状态变化

```objc
// 通过 KVO 监听 hidden 属性变化
[hiddenManager addObserver:self 
                forKeyPath:@"hidden" 
                   options:NSKeyValueObservingOptionNew 
                   context:nil];

// 监听 hiddenKeys 变化
[hiddenManager addObserver:self 
                forKeyPath:@"hiddenKeys" 
                   options:NSKeyValueObservingOptionNew 
                   context:nil];
```

## 工作原理

1. **状态管理**: 内部维护一个 `NSSet` 来存储所有隐藏状态的键值
2. **智能判断**: 当 `onlyKey` 为 nil 时，任何键值被设置为隐藏都会导致视图隐藏；当 `onlyKey` 有值时，只有该键值能控制视图隐藏
3. **双向绑定**: 自动监听视图的 `hidden` 属性变化，确保状态同步
4. **内存管理**: 使用关联对象和弱引用来避免循环引用

## 适用场景

- **多状态 UI 管理**: 如加载中、错误、空数据等状态的显示控制
- **复杂界面逻辑**: 需要根据多个条件控制视图显示的场景
- **状态机管理**: 适合实现复杂的状态转换逻辑
- **模块化开发**: 不同模块可以独立控制视图的显示状态

## 依赖

- iOS 8.0+
- KVOController

## 许可证

Copyright © 2024 YLCHUN/Cityu. All rights reserved.
