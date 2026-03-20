# Brosdk 浏览器控制 SDK

轻量级浏览器自动化控制 SDK，支持 Windows/macOS/Linux，提供 C 和 C++ API。

## 支持平台

| 平台 | 动态库 |
|------|--------|
| Windows x64 | `windows/brosdk.dll` |
| macOS ARM64 | `arm64-osx/libbrosdk.dylib` |

## 核心功能

### 环境管理
- `sdk_env_create` - 创建浏览器环境配置
- `sdk_env_update` - 更新环境参数
- `sdk_env_page` - 页面级操作
- `sdk_env_destroy` - 销毁环境

### 浏览器控制
- `sdk_browser_open` - 启动浏览器实例
- `sdk_browser_close` - 关闭浏览器
- `sdk_browser_info` - 获取浏览器状态

### 认证与回调
- `sdk_token_update` - 更新认证 Token
- `sdk_register_result_cb` - 注册操作结果回调
- `sdk_register_cookies_storage_cb` - 注册 Cookie 持久化回调

### 工具函数
- `sdk_info` / `sdk_browser_info` - 获取 SDK 和浏览器信息
- `sdk_shutdown` - 安全关闭 SDK
- `sdk_error_name` / `sdk_error_string` - 错误码转义

## 快速开始

### C 语言调用

```c
#include "brosdk.h"

int main() {
    sdk_handle_t handle = NULL;
    char *out_data = NULL;
    size_t out_len = 0;

    // 初始化 SDK
    int32_t ret = sdk_init(&handle, NULL, 0, &out_data, &out_len);
    if (ret == 0) {
        printf("SDK 初始化成功\n");
    }

    // 获取 SDK 信息
    sdk_info(&out_data, &out_len);
    printf("SDK Info: %.*s\n", (int)out_len, out_data);
    sdk_free(out_data);

    // 关闭 SDK
    sdk_shutdown();
    return 0;
}
```

### C++ 调用

```cpp
#include "brosdk.h"

// 实现 ISDK 接口进行调用
```

## 错误码

所有 API 返回 `int32_t`，使用以下函数判断状态：

| 函数 | 用途 |
|------|------|
| `sdk_is_ok(code)` | 操作成功 |
| `sdk_is_error(code)` | 发生错误 |
| `sdk_is_warn(code)` | 警告 |
| `sdk_is_done(code)` | 操作完成 |
| `sdk_is_event(code)` | 事件触发 |

获取错误信息：
```c
const char *name = sdk_error_name(code);
const char *msg = sdk_error_string(code);
```

## 编译链接

### Windows
```bash
gcc -o app app.c -L./windows -lbrosdk
```

### macOS
```bash
gcc -o app app.c -L./arm64-osx -lbrosdk
```

## 目录结构

```
sdk/
├── brosdk.h           # 头文件
├── windows/           # Windows 动态库
│   └── brosdk.dll
└── arm64-osx/         # macOS ARM64 动态库
    └── libbrosdk.dylib
```
