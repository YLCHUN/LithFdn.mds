# ControlHandler

UIControl 事件处理的扩展工具，提供基于 Block 的事件绑定机制。

## 功能特性

- 支持 Block 方式绑定 UIControl 事件
- 自动管理事件生命周期，避免内存泄漏
- 支持动态子类化，无需考虑 KVO 影响
- 提供事件准备和重写机制

## 主要组件

### UIControl+Handler

UIControl 的分类扩展，提供事件处理的核心功能：

- `ca_handlerPrepare` - 事件准备，注册 KVO 之前执行
- `ca_sendActionOverwrite` - 重写 sendAction:to:forEvent: 方法
- `ca_sendControlAction:to:forEvent:` - 触发 ControlAction
- `ca_addHandler:forControlEvents:` - 添加 Block 事件
- `ca_removeHandler:forControlEvents:` - 移除 Block 事件

### ControlAction

事件动作的封装类，包含：

- 事件处理器 Block
- 控制事件类型
- 唯一标识符
- 事件管理方法

## 使用示例

```objc
// 添加事件处理
ControlAction *action = [button ca_addHandler:^(UIControl *sender) {
    NSLog(@"按钮被点击");
} forControlEvents:UIControlEventTouchUpInside];

// 移除事件处理
[button ca_removeHandler:action forControlEvents:UIControlEventTouchUpInside];
```

## 注意事项

- 使用前需要调用 `ca_handlerPrepare` 进行初始化
- 支持自动内存管理，对象销毁时会自动清理事件
- 动态子类化机制确保与 KVO 的兼容性

## 版本信息

- 创建时间：2022年5月14日
- 作者：Cityu
