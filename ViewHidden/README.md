# ViewHidden

一个功能完善的 iOS 视图隐藏管理组件，提供基于键值对的视图隐藏状态管理，支持多状态控制、KVO 监听和智能隐藏逻辑。解决到处Hidden导致的结果混乱。

## 功能特性

- **多状态管理**: 支持通过不同的 key 来管理视图的隐藏状态
- **智能隐藏逻辑**: 提供 `onlyKey` 机制，支持独占式隐藏控制
- **KVO 支持**: 自动监听视图的 hidden 属性变化，支持双向绑定
- **内存安全**: 使用弱引用避免循环引用，自动管理内存
- **简单易用**: 通过 UIView 分类提供便捷的访问方式

## 工作原理流程图

```mermaid
flowchart TD
    A[获取ViewHidden实例] --> B[初始化隐藏状态管理]
    B --> C[创建隐藏键值集合]
    C --> D[设置KVO监听器]
    
    D --> E[等待隐藏状态设置请求]
    E --> F{请求类型判断}
    
    F -->|设置隐藏状态| G[调用setHidden:forKey:]
    F -->|查询隐藏状态| H[调用hiddenForKey:]
    F -->|设置独占键值| I[设置onlyKey属性]
    
    G --> J[验证键值有效性]
    J --> K{键值是否有效?}
    
    K -->|否| L[忽略无效键值]
    K -->|是| M[检查独占键值设置]
    
    M --> N{是否设置了独占键值?}
    N -->|是| O{当前键值是否为独占键值?}
    N -->|否| P[允许所有键值控制]
    
    O -->|是| Q[允许设置隐藏状态]
    O -->|否| R[忽略非独占键值设置]
    
    P --> Q
    Q --> S[更新隐藏键值集合]
    S --> T[计算最终隐藏状态]
    
    T --> U{是否有隐藏键值?}
    U -->|是| V[设置视图为隐藏]
    U -->|否| W[设置视图为显示]
    
    V --> X[触发KVO通知]
    W --> X
    
    X --> Y[更新视图hidden属性]
    Y --> Z[完成隐藏状态设置]
    
    H --> AA[查询指定键值的隐藏状态]
    AA --> BB[返回布尔值结果]
    
    I --> CC[设置独占键值]
    CC --> DD[更新独占控制逻辑]
    
    L --> EE[记录无效键值日志]
    R --> FF[记录权限拒绝日志]
    
    EE --> GG[返回设置失败]
    FF --> GG
    Z --> GG
    BB --> GG
    DD --> GG
    
    GG --> E
    
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

## 安装使用

将 `ViewHidden.h`