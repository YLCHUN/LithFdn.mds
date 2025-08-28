# DynamicImage

一个iOS动态图片处理组件，支持深色模式自动切换和运行时图片动态生成。

## 功能特性

- 🌙 **深色模式支持**: 自动根据系统主题切换图片
- ⚡ **运行时动态生成**: 支持闭包动态提供图片内容
- 🔄 **自动回退机制**: 当指定主题图片不存在时自动回退到其他主题
- 🎨 **无缝集成**: 与UIKit完全兼容，无需修改现有代码
- 🚀 **高性能**: 使用运行时技术，最小化内存占用
- 📱 **iOS 13+**: 原生支持最新的iOS系统特性

## 实现原理

### 核心架构

DynamicImage使用Objective-C运行时技术实现图片的动态切换功能：

1. **动态子类生成**: 通过`objc_allocateClassPair`创建UIImage的动态子类
2. **方法重写**: 重写关键方法如`imageWithConfiguration:`和`resizableImageWithCapInsets:`
3. **关联对象**: 使用`objc_setAssociatedObject`存储图片的元数据信息
4. **线程安全**: 使用`pthread_mutex`确保动态类创建的线程安全

### 技术细节

- **ImageDynamicAsset**: 核心类，管理图片提供者和样式切换逻辑
- **_IDATrait**: 内部特性类，存储图片的元数据和样式信息
- **运行时方法替换**: 通过IMP替换实现动态图片切换
- **自动回退**: 当深色模式图片不存在时，自动使用浅色模式图片

### 手动集成

1. 将`ImageDynamicAsset.h/m`和`UIImage+Dynamic.h/m`文件添加到项目中
2. 确保项目支持iOS 13.0+

## 使用示例

### 基础用法

#### 1. 创建深色/浅色模式图片

```objc
UIImage *lightImage = [UIImage imageNamed:@"icon_light"];
UIImage *darkImage = [UIImage imageNamed:@"icon_dark"];

// 自动根据系统主题切换
UIImage *dynamicImage = [UIImage imageWithLight:lightImage dark:darkImage];

// 设置到ImageView
imageView.image = dynamicImage;
```

#### 2. 使用动态提供者

```objc
UIImage *dynamicImage = [UIImage imageWithDynamicProvider:^UIImage * _Nullable(UIUserInterfaceStyle style) {
    if (style == UIUserInterfaceStyleDark) {
        return [UIImage imageNamed:@"dark_theme_icon"];
    } else {
        return [UIImage imageNamed:@"light_theme_icon"];
    }
}];
```

#### 3. 高级用法 - 自定义ImageDynamicAsset

```objc
ImageDynamicAsset *asset = [[ImageDynamicAsset alloc] initWithImageProvider:^UIImage * _Nullable(UIUserInterfaceStyle style) {
    // 根据主题返回不同的图片
    switch (style) {
        case UIUserInterfaceStyleDark:
            return [self generateDarkModeImage];
        case UIUserInterfaceStyleLight:
            return [self generateLightModeImage];
        default:
            return [self generateDefaultImage];
    }
}];

UIImage *image = [asset resolvedImageWithStyle:UIUserInterfaceStyleDark];
```

### 实际应用场景

#### 1. 应用图标动态切换

```objc
- (void)setupAppIcon {
    UIImage *lightIcon = [UIImage imageNamed:@"app_icon_light"];
    UIImage *darkIcon = [UIImage imageNamed:@"app_icon_dark"];
    
    UIImage *dynamicIcon = [UIImage imageWithLight:lightIcon dark:darkIcon];
    self.iconImageView.image = dynamicIcon;
}
```

#### 2. 按钮背景图片

```objc
- (void)setupButton {
    UIImage *lightBg = [UIImage imageNamed:@"button_bg_light"];
    UIImage *darkBg = [UIImage imageNamed:@"button_bg_dark"];
    
    UIImage *dynamicBg = [UIImage imageWithLight:lightBg dark:darkBg];
    [self.button setBackgroundImage:dynamicBg forState:UIControlStateNormal];
}
```

#### 3. 复杂图片生成

```objc
UIImage *complexImage = [UIImage imageWithDynamicProvider:^UIImage * _Nullable(UIUserInterfaceStyle style) {
    // 根据主题生成复杂的图片内容
    UIGraphicsImageRenderer *renderer = [[UIGraphicsImageRenderer alloc] initWithSize:CGSizeMake(100, 100)];
    
    return [renderer imageWithActions:^(UIGraphicsImageRendererContext * _Nonnull rendererContext) {
        UIColor *backgroundColor = (style == UIUserInterfaceStyleDark) ? 
            [UIColor blackColor] : [UIColor whiteColor];
        [backgroundColor setFill];
        
        UIRectFill(CGRectMake(0, 0, 100, 100));
        
        // 绘制其他内容...
    }];
}];
```

## API 参考

### UIImage+Dynamic

| 方法 | 描述 |
|------|------|
| `+imageWithDynamicProvider:` | 使用闭包动态提供图片 |
| `+imageWithLight:dark:` | 创建支持深色/浅色模式的图片 |
| `-dynamicProviderRawImage` | 获取原始图片 |
| `-isDynamicProviderImage` | 判断是否为动态图片 |

### ImageDynamicAsset

| 方法 | 描述 |
|------|------|
| `+assetWithImageProvider:` | 创建图片资源对象 |
| `-resolvedImageWithStyle:` | 根据样式解析图片 |
| `+isDynamicAssetImage:` | 判断是否为动态资源图片 |
| `+rawImageFromDynamicAssetImage:` | 从动态图片获取原始图片 |

## 注意事项

1. **iOS版本要求**: 需要iOS 13.0或更高版本
2. **内存管理**: 动态生成的图片会自动管理内存，无需手动释放
3. **性能考虑**: 首次创建动态类会有轻微性能开销，后续使用无影响
4. **线程安全**: 支持多线程环境下的安全使用

## 系统要求

- iOS 13.0+
- Xcode 11.0+
- Objective-C

## 许可证

Copyright © 2020 YLCHUN. All rights reserved.

## 贡献

欢迎提交Issue和Pull Request来改进这个项目。


