# LithFdn

一个功能丰富的iOS开发工具集合，包含17个实用的小组件，每个组件都专注于解决特定的开发需求。基于现代化的架构设计，提供Objective-C和Swift两种语言实现版本。

## 🚀 项目特色

- **模块化设计**: 每个组件独立，可按需引入，不影响项目整体架构
- **双语言支持**: 核心组件提供Objective-C和Swift两种实现版本
- **高性能**: 基于底层优化和现代iOS技术，确保性能优异
- **易用性**: 简洁的API设计，快速集成，降低学习成本
- **生产就绪**: 经过实际项目验证，稳定可靠

## 📱 组件总览

### 🎨 界面组件

#### [VSync](./VSync/)
高性能垂直同步调度组件，基于CADisplayLink实现V-Sync信号同步，提供精确的任务调度和执行控制。适用于动画同步、游戏开发、UI更新等需要与屏幕刷新同步的场景。

#### [NestedScrollView](./NestedScrollView/)
高性能嵌套滚动视图解决方案，支持UIScrollView和UICollectionView的嵌套滚动。解决复杂滚动场景下的手势冲突和滚动体验问题。

#### [GridStaggered](./GridStaggered/)
功能完善的网格交错布局组件，支持多种错位类型、旋转角度和自定义布局参数。适用于创建独特的网格视觉效果和瀑布流布局。

#### [PagingView](./PagingView/)
功能完善的分页视图组件，提供灵活的分页计算和分页滚动视图实现。支持自定义分页大小、前后缀设置和智能分页逻辑。

#### [ProgressSlider](./ProgressSlider/)
功能完善的进度滑块组件，支持双进度显示（当前进度和预加载进度）、拖拽交互和丰富的自定义选项。适用于视频播放器、音频播放器等需要进度控制的场景。

### 🎨 视觉组件

#### [ColorPalette](./ColorPalette/)
功能完善的iOS颜色调色板框架，提供完整的颜色处理、转换和调色功能。支持RGB、HSL、ABGR、ALSH等多种颜色空间转换，基于协议导向设计。

#### [DynamicImage](./DynamicImage/)
iOS动态图片处理组件，支持深色模式自动切换和运行时图片动态生成。充分利用iOS 13+的新特性，提供智能的图片管理方案。

#### [Geometry](./Geometry/)
iOS几何图形绘制工具集合，提供气泡贝塞尔曲线和多边形贝塞尔路径的创建功能。适用于自定义UI绘制和图形设计，支持复杂的几何图形生成。

### 🔧 功能组件

#### [ContentProvideManager](./ContentProvideManager/)
基于协议导向设计的内容提供管理框架，用于iOS应用中的动态内容管理和UI组件复用。支持运行时注册和注销内容提供者，提供Objective-C和Swift两种语言实现版本。

#### [ControlHandler](./ControlHandler/)
UIControl事件处理的扩展工具，提供基于Block的事件绑定机制，支持动态子类化和自动内存管理。使用Method Swizzling技术，代码更简洁易读。

#### [InputPrompt](./InputPrompt/)
用于iOS文本输入长度监控和截断的Objective-C库，支持实时长度提示、自动截断和自定义回调。提供智能的文本输入管理方案。

#### [StringFit](./StringFit/)
NSString的文本适配扩展工具，提供智能的文本宽度计算和截断功能。适用于UI布局优化和文本显示适配。

#### [ViewHidden](./ViewHidden/)
功能完善的iOS视图隐藏管理组件，提供基于键值对的视图隐藏状态管理。支持多状态控制、KVO监听和智能隐藏逻辑，解决到处Hidden导致的结果混乱。

### 📊 数据处理

#### [Decimal](./Decimal/)
数字单位转换和格式化工具，支持中文单位显示、多种舍入模式和灵活的小数位数控制。基于整数运算，避免浮点数精度问题，提供Objective-C和Swift两种语言实现版本。

#### [InvokeMap](./InvokeMap/)
轻量级的Objective-C函数调用映射框架，支持多上下文函数注册、异步调用和优先级排序。提供灵活的函数调用管理方案。

### ⚡ 性能优化

#### [PerformDelay](./PerformDelay/)
NSObject的延迟执行扩展工具，支持Selector和Block两种方式，自动管理内存和取消操作。延迟执行期间，当前对象不会被持有，避免内存泄漏。

#### [TouchMonitor](./TouchMonitor/)
轻量级的iOS触摸事件监控工具，支持Objective-C和Swift两种语言实现。提供实时触摸事件监控和手势识别功能，适用于触摸事件分析和调试。

## 🛠 技术架构

### 设计原则
- **单一职责**: 每个组件专注于解决特定问题
- **开闭原则**: 支持扩展，对修改关闭
- **依赖倒置**: 基于协议和抽象，而非具体实现
- **接口隔离**: 提供精简而完整的API接口

### 核心技术
- **协议导向编程**: 大量使用协议定义接口契约
- **运行时技术**: Method Swizzling、动态子类化等
- **内存管理**: ARC支持，智能引用管理
- **性能优化**: 底层优化，最小化性能开销

## 📦 安装使用

### 手动集成
1. 将需要的组件目录复制到项目中
2. 导入对应的头文件
3. 根据组件文档进行配置和使用

### 组件依赖
大部分组件无外部依赖，可直接使用。少数组件可能需要：
- iOS 8.0+ (基础组件)
- iOS 13.0+ (DynamicImage等新特性组件)
- ARC支持

## 🔍 使用示例

### 快速开始
```objc
// 使用VSync进行动画同步
VSync *vsync = [VSync sharedInstance];
[vsync scheduleTask:^{
    [self updateAnimation];
}];

// 使用ColorPalette进行颜色转换
uint32_t abgr = UIColor_to_abgr([UIColor redColor]);
UIColor *convertedColor = abgr_to_UIColor(abgr);

// 使用ControlHandler绑定事件
[button ca_addHandler:^(UIButton *sender) {
    NSLog(@"按钮被点击");
} forControlEvents:UIControlEventTouchUpInside];
```

### 组件组合使用
```objc
// 组合多个组件创建复杂功能
// 1. 使用VSync同步动画
// 2. 使用ColorPalette动态调整颜色
// 3. 使用Geometry绘制自定义图形
// 4. 使用ControlHandler处理交互事件
```

## 📚 文档说明

每个组件都包含详细的README.md文档，涵盖：
- 功能特性介绍
- 技术实现原理
- 使用示例代码
- API接口说明
- 性能特点分析
- 适用场景说明

## 🎯 适用场景

- **移动应用开发**: 提供完整的iOS开发工具链
- **游戏开发**: VSync、Geometry等组件支持游戏开发需求
- **企业应用**: ContentProvideManager、ViewHidden等支持复杂业务逻辑
- **UI设计工具**: ColorPalette、DynamicImage等支持设计相关功能
- **性能优化**: VSync、PerformDelay等支持应用性能优化

## 🔧 系统要求

- **基础要求**: iOS 8.0+, Xcode 8.0+, ARC支持
- **新特性组件**: iOS 13.0+, Xcode 11.0+
- **Swift组件**: Swift 5.0+

## 📄 许可证

Copyright © 2024-2025 YLCHUN/Cityu. All rights reserved.

## 🤝 贡献指南

欢迎提交Issue和Pull Request来改进这些组件：

1. Fork项目
2. 创建功能分支
3. 提交更改
4. 推送到分支
5. 创建Pull Request

## 📞 联系方式

如有问题或建议，请通过以下方式联系：
- 提交GitHub Issue
- 发送邮件至项目维护者

---

**LithFdn** - 让iOS开发更简单、更高效、更优雅 🚀
