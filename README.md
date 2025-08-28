# LithFdn 小组件集合

这是一个iOS开发工具集合，包含多个实用的小组件，每个组件都专注于解决特定的开发需求。

## 组件列表

### [VSync](./VSync/)
一个高性能的iOS垂直同步调度组件，基于CADisplayLink实现V-Sync信号同步，提供精确的任务调度和执行控制。支持60Hz刷新率同步、智能时间窗口控制、线程安全的任务队列管理，适用于动画同步、游戏开发、UI更新等需要与屏幕刷新同步的场景。提供Objective-C和Swift两种语言实现版本。

### [NestedScrollView](./NestedScrollView/)
一个高性能的iOS嵌套滚动视图解决方案，支持UIScrollView和UICollectionView的嵌套滚动。

### [GridStaggered](./GridStaggered/)
一个功能完善的iOS网格交错布局组件，支持多种错位类型、旋转角度和自定义布局参数，适用于创建独特的网格视觉效果。

### [ContentProvideManager](./ContentProvideManager/)
一个基于协议导向设计的内容提供管理框架，用于iOS应用中的动态内容管理和UI组件复用。提供Objective-C和Swift两种语言实现版本。

### [DynamicImage](./DynamicImage/)
一个iOS动态图片处理组件，支持深色模式自动切换和运行时图片动态生成。

### [Geometry](./Geometry/)
一个iOS几何图形绘制工具集合，提供气泡贝塞尔曲线和多边形贝塞尔路径的创建功能，适用于自定义UI绘制和图形设计。

### [PagingView](./PagingView/)
一个功能完善的iOS分页视图组件，提供灵活的分页计算和分页滚动视图实现，支持自定义分页大小、前后缀设置和智能分页逻辑。

### [ProgressSlider](./ProgressSlider/)
一个功能完善的iOS进度滑块组件，支持双进度显示（当前进度和预加载进度）、拖拽交互和丰富的自定义选项，适用于视频播放器、音频播放器等需要进度控制的场景。

### [TouchMonitor](./TouchMonitor/)
一个轻量级的iOS触摸事件监控工具，支持Objective-C和Swift两种语言实现，提供实时触摸事件监控和手势识别功能。

### [ControlHandler](./ControlHandler/)
UIControl事件处理的扩展工具，提供基于Block的事件绑定机制，支持动态子类化和自动内存管理。

### [InvokeMap](./InvokeMap/)
一个轻量级的Objective-C函数调用映射框架，支持多上下文函数注册、异步调用和优先级排序。

### [ColorPalette](./ColorPalette/)
一个功能完善的iOS颜色调色板框架，提供完整的颜色处理、转换和调色功能。

### [InputPrompt](./InputPrompt/)
一个用于iOS文本输入长度监控和截断的Objective-C库，支持实时长度提示、自动截断和自定义回调。

### [StringFit](./StringFit/)
NSString的文本适配扩展工具，提供智能的文本宽度计算和截断功能，适用于UI布局优化。

### [ViewHidden](./ViewHidden/)
一个功能完善的iOS视图隐藏管理组件，提供基于键值对的视图隐藏状态管理，支持多状态控制、KVO监听和智能隐藏逻辑，适用于复杂UI状态管理和多条件显示控制。解决到处Hidden导致的结果混乱。

### [Decimal](./Decimal/)
数字单位转换和格式化工具，支持中文单位显示、多种舍入模式和灵活的小数位数控制。提供Objective-C和Swift两种语言实现版。

### [PerformDelay](./PerformDelay/)
NSObject的延迟执行扩展工具，支持Selector和Block两种方式，自动管理内存和取消操作，延迟执行期间，当前对象不会被持有。

## 安装使用

每个组件都可以独立使用，请查看对应组件目录下的README.md文档了解详细的使用方法和API说明。

## 许可证

Copyright © 2024-2025 YLCHUN/Cityu. All rights reserved.

## 贡献

欢迎提交Issue和Pull Request来改进这些组件。
