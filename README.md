# BroSDK Browser Control SDK

[English](README_EN.md) | 简体中文

BroSDK 封装了 Chrome、Firefox 等[多版本指纹浏览器内核](https://github.com/browsersdk/brosdk-core)，适配 Windows、macOS、Linux 等操作系统，并提供代理 IP 集成、Cookie 管理和会话持久化能力。

当前仓库提供 C/C++ API、头文件和平台动态库，用于创建独立浏览器环境、启动浏览器实例、维护 Cookie/Storage 状态、复用会话上下文，并与 CDP、Playwright、Puppeteer 或 MCP Server 等自动化链路配合使用。

## 功能概览

- 创建独立浏览器环境，并用环境 ID 管理后续启动、更新和销毁。
- 启动和关闭浏览器实例，获取浏览器运行状态和 SDK 信息。
- 管理 Cookie、Storage state 和会话持久化，使同一环境可以复用登录状态和业务上下文。
- 将环境 ID、账号 IP、Cookie/Storage 状态与业务系统中的账号、用户或任务模型关联。
- 在 C/C++ 程序中接入浏览器环境管理、浏览器控制、回调和错误码处理。
- 结合 CDP、Playwright、Puppeteer 或 MCP Server，将浏览器任务交给脚本、测试系统或 AI Agent 执行。

## 核心功能

### 环境管理

BroSDK 以环境为核心抽象。每个环境可保存独立的浏览器配置、指纹参数、Cookie/Storage 状态和会话上下文。

| API | 说明 |
|-----|------|
| `sdk_env_create` | 创建浏览器环境配置 |
| `sdk_env_update` | 更新环境参数 |
| `sdk_env_page` | 执行页面级操作 |
| `sdk_env_destroy` | 销毁环境 |

### 浏览器控制

通过 SDK 启动浏览器实例，并在任务结束后关闭浏览器。适合桌面客户端、自动化任务、测试任务和后台服务。

| API | 说明 |
|-----|------|
| `sdk_browser_open` | 启动浏览器实例 |
| `sdk_browser_close` | 关闭浏览器 |
| `sdk_browser_info` | 获取浏览器运行状态 |

### Cookie 与会话持久化

SDK 提供 Cookie 持久化回调，便于业务系统保存和恢复浏览器会话状态。上层系统可以将登录状态、Storage state 和环境 ID 绑定到自己的用户、店铺、客户或任务模型中。

| API | 说明 |
|-----|------|
| `sdk_register_cookies_storage_cb` | 注册 Cookie 持久化回调 |
| `sdk_register_result_cb` | 注册操作结果回调 |

### 认证与生命周期

SDK 支持运行时更新认证 Token，并提供关闭接口，便于长期运行的服务或客户端处理认证刷新和资源释放。

| API | 说明 |
|-----|------|
| `sdk_token_update` | 更新认证 Token |
| `sdk_shutdown` | 安全关闭 SDK |

### 工具函数与错误码

所有 API 返回 `int32_t` 状态码，可通过工具函数判断成功、错误、警告、完成和事件状态。

| API | 说明 |
|-----|------|
| `sdk_info` | 获取 SDK 信息 |
| `sdk_error_name` | 获取错误码名称 |
| `sdk_error_string` | 获取错误码描述 |
| `sdk_is_ok` | 判断操作是否成功 |
| `sdk_is_error` | 判断是否发生错误 |
| `sdk_is_warn` | 判断是否为警告 |
| `sdk_is_done` | 判断操作是否完成 |
| `sdk_is_event` | 判断是否为事件 |

## 适用场景

- **多账号运营 SaaS**：为每个业务账号维护独立指纹环境、账号 IP、Cookie 和 Storage state。
- **ERP / CRM / 内部系统**：把浏览器环境能力集成到企业内部系统，通过统一工作台打开和管理账号环境。
- **自动化测试与监测**：用于授权测试、页面巡检、账号健康检查和流程监测等任务。
- **AI Agent 浏览器任务**：结合 MCP Server、CDP、Playwright 或 Puppeteer，让 AI Agent 调用 SDK 打开环境并执行浏览器任务。

## 相关仓库与模块关系

BroSDK 由多个仓库组成。当前仓库 `brosdk` 是原生 SDK 入口，提供 C/C++ 头文件、动态库和基础调用方式；其他仓库分别覆盖浏览器内核、文档、多语言接入、示例项目和 AI Agent 集成。

| 仓库 | 模块 | 说明 |
|------|------|------|
| [brosdk](https://github.com/browsersdk/brosdk) | Native SDK | C/C++ API、头文件和平台动态库 |
| [brosdk-core](https://github.com/browsersdk/brosdk-core) | Browser Core | 浏览器内核、版本和运行依赖 |
| [brosdk-docs](https://github.com/browsersdk/brosdk-docs) | Documentation | SDK 文档、API 说明和接入指南 |
| [brosdk-typescript](https://github.com/browsersdk/brosdk-typescript) | TypeScript SDK | Node.js、前端工具链和自动化脚本接入 |
| [brosdk-python](https://github.com/browsersdk/brosdk-python) | Python SDK | Python 自动化、测试和 AI 工作流接入 |
| [brosdk-rust](https://github.com/browsersdk/brosdk-rust) | Rust SDK | Rust 服务、CLI 和高性能任务接入 |
| [brosdk-server-go](https://github.com/browsersdk/brosdk-server-go) | Go Server | 将 BroSDK 封装为服务端能力或内部 HTTP API |
| [browser-demo](https://github.com/browsersdk/browser-demo) | Demo | 环境创建、浏览器启动和会话复用示例 |
| [browser-cpp-demo](https://github.com/browsersdk/browser-cpp-demo) | C++ Demo | C++ 工程链接和调用原生 SDK 示例 |
| [brosdk-mcp](https://github.com/browsersdk/brosdk-mcp) | MCP Server | AI Agent 通过 MCP 调用浏览器环境能力 |
| [chrome-devtools-mcp](https://github.com/browsersdk/chrome-devtools-mcp) | CDP MCP | 基于 Chrome DevTools Protocol 的 MCP 浏览器操作模块 |

推荐阅读顺序：

1. 先阅读当前仓库，了解 BroSDK 的原生能力和 API 边界。
2. 再查看 [browser-demo](https://github.com/browsersdk/browser-demo) 或 [browser-cpp-demo](https://github.com/browsersdk/browser-cpp-demo)，跑通第一个浏览器环境。
3. 根据技术栈选择 TypeScript、Python、Rust 或 Go 接入模块。
4. 如果要接入 AI Agent，再查看 [brosdk-mcp](https://github.com/browsersdk/brosdk-mcp) 和 [chrome-devtools-mcp](https://github.com/browsersdk/chrome-devtools-mcp)。
5. 如需确认浏览器版本和内核支持，查看 [brosdk-core](https://github.com/browsersdk/brosdk-core)。

## 支持平台

BroSDK 适配 Windows、macOS、Linux。当前仓库已包含以下平台动态库：

| 平台 | 动态库 |
|------|--------|
| Windows x64 | `windows/brosdk.dll` |
| macOS ARM64 | `arm64-osx/libbrosdk.dylib` |

## 支持的浏览器内核

浏览器内核和版本支持请访问：

https://github.com/browsersdk/brosdk-core

## 快速开始

### C 语言调用

```c
#include <stdio.h>
#include "brosdk.h"

int main() {
    sdk_handle_t handle = NULL;
    char *out_data = NULL;
    size_t out_len = 0;

    int32_t ret = sdk_init(&handle, NULL, 0, &out_data, &out_len);
    if (!sdk_is_ok(ret)) {
        printf("SDK 初始化失败: %s - %s\n", sdk_error_name(ret), sdk_error_string(ret));
        return ret;
    }

    printf("SDK 初始化成功\n");

    sdk_info(&out_data, &out_len);
    printf("SDK Info: %.*s\n", (int)out_len, out_data);
    sdk_free(out_data);

    sdk_shutdown();
    return 0;
}
```

### C++ 调用

```cpp
#include "brosdk.h"

// 实现 ISDK 接口后，可在 C++ 项目中调用环境管理、浏览器控制和回调能力。
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

```text
sdk/
├── brosdk.h           # 头文件
├── windows/           # Windows 动态库
│   └── brosdk.dll
└── arm64-osx/         # macOS ARM64 动态库
    └── libbrosdk.dylib
```
