# BroSDK Browser Control SDK

English | [简体中文](README.md)

BroSDK packages [multi-version fingerprint browser cores](https://github.com/browsersdk/brosdk-core) based on Chrome, Firefox, and other browser cores. It supports Windows, macOS, Linux, proxy IP integration, Cookie management, and session persistence.

This repository provides C/C++ APIs, headers, and platform dynamic libraries for creating independent browser environments, launching browser instances, maintaining Cookie/Storage state, reusing session context, and working with automation layers such as CDP, Playwright, Puppeteer, or MCP Server.

## Feature Overview

- Create independent browser environments and manage their create, update, launch, and destroy lifecycle through environment IDs.
- Open and close browser instances, and query browser runtime status and SDK information.
- Manage Cookie, Storage state, and session persistence so the same environment can reuse login state and business context.
- Associate environment IDs, account IPs, and Cookie/Storage state with accounts, users, or task models in your application.
- Integrate browser environment management, browser control, callbacks, and error handling into C/C++ programs.
- Use CDP, Playwright, Puppeteer, or MCP Server to hand browser tasks to scripts, test systems, or AI Agents.

## Core Capabilities

### Environment Management

BroSDK uses browser environments as the core abstraction. Each environment can keep its own browser configuration, fingerprint parameters, Cookie/Storage state, and session context.

| API | Description |
|-----|-------------|
| `sdk_env_create` | Create a browser environment configuration |
| `sdk_env_update` | Update environment parameters |
| `sdk_env_page` | Execute page-level operations |
| `sdk_env_destroy` | Destroy an environment |

### Browser Control

Use the SDK to launch browser instances and close them after a task finishes. This is suitable for desktop clients, automation tasks, test jobs, and backend services.

| API | Description |
|-----|-------------|
| `sdk_browser_open` | Open a browser instance |
| `sdk_browser_close` | Close a browser instance |
| `sdk_browser_info` | Get browser runtime status |

### Cookie And Session Persistence

The SDK provides a Cookie persistence callback so the upper-layer system can save and restore browser session state. Applications can bind login state, Storage state, and environment IDs to their own users, stores, customers, or task models.

| API | Description |
|-----|-------------|
| `sdk_register_cookies_storage_cb` | Register the Cookie persistence callback |
| `sdk_register_result_cb` | Register the operation result callback |

### Authentication And Lifecycle

The SDK supports runtime token updates and provides a shutdown API for long-running clients or services that need to handle token refresh and resource cleanup.

| API | Description |
|-----|-------------|
| `sdk_token_update` | Update the authentication token |
| `sdk_shutdown` | Shut down the SDK safely |

### Utilities And Error Codes

All APIs return `int32_t` status codes. Utility functions can be used to check success, errors, warnings, completion states, and events.

| API | Description |
|-----|-------------|
| `sdk_info` | Get SDK information |
| `sdk_error_name` | Get the error code name |
| `sdk_error_string` | Get the error code description |
| `sdk_is_ok` | Check whether the operation succeeded |
| `sdk_is_error` | Check whether an error occurred |
| `sdk_is_warn` | Check whether the status is a warning |
| `sdk_is_done` | Check whether the operation is done |
| `sdk_is_event` | Check whether the status is an event |

## Use Cases

- **Multi-account operation SaaS**: Maintain independent fingerprint environments, account IPs, Cookie, and Storage state for each business account.
- **ERP / CRM / internal systems**: Integrate browser environment capabilities into internal systems and manage account environments from a unified workspace.
- **Automation testing and monitoring**: Use BroSDK for authorized testing, page inspection, account health checks, and workflow monitoring.
- **AI Agent browser tasks**: Use MCP Server, CDP, Playwright, or Puppeteer so AI Agents can open environments and execute browser tasks through the SDK.

## Related Repositories And Module Map

BroSDK is organized across multiple repositories. This repository, `brosdk`, is the native SDK entry point and provides C/C++ headers, dynamic libraries, and basic calling conventions. Other repositories cover browser cores, documentation, language bindings, demos, and AI Agent integration.

| Repository | Module | Description |
|------------|--------|-------------|
| [brosdk](https://github.com/browsersdk/brosdk) | Native SDK | C/C++ APIs, headers, and platform dynamic libraries |
| [brosdk-core](https://github.com/browsersdk/brosdk-core) | Browser Core | Browser kernels, versions, and runtime dependencies |
| [brosdk-docs](https://github.com/browsersdk/brosdk-docs) | Documentation | SDK documentation, API references, and integration guides |
| [brosdk-typescript](https://github.com/browsersdk/brosdk-typescript) | TypeScript SDK | Integration for Node.js, frontend tooling, and automation scripts |
| [brosdk-python](https://github.com/browsersdk/brosdk-python) | Python SDK | Integration for Python automation, testing, and AI workflows |
| [brosdk-rust](https://github.com/browsersdk/brosdk-rust) | Rust SDK | Integration for Rust services, CLIs, and high-performance tasks |
| [brosdk-server-go](https://github.com/browsersdk/brosdk-server-go) | Go Server | Wrap BroSDK as a server-side capability or internal HTTP API |
| [browser-demo](https://github.com/browsersdk/browser-demo) | Demo | Examples for environment creation, browser launch, and session reuse |
| [browser-cpp-demo](https://github.com/browsersdk/browser-cpp-demo) | C++ Demo | Example C++ project for linking and calling the native SDK |
| [brosdk-mcp](https://github.com/browsersdk/brosdk-mcp) | MCP Server | Allow AI Agents to call browser environment capabilities through MCP |
| [chrome-devtools-mcp](https://github.com/browsersdk/chrome-devtools-mcp) | CDP MCP | MCP browser operation module based on Chrome DevTools Protocol |

Recommended reading order:

1. Start with this repository to understand the native SDK capabilities and API boundaries.
2. Run [browser-demo](https://github.com/browsersdk/browser-demo) or [browser-cpp-demo](https://github.com/browsersdk/browser-cpp-demo) to open your first browser environment.
3. Choose the TypeScript, Python, Rust, or Go module according to your stack.
4. For AI Agent integration, see [brosdk-mcp](https://github.com/browsersdk/brosdk-mcp) and [chrome-devtools-mcp](https://github.com/browsersdk/chrome-devtools-mcp).
5. To check browser versions and core support, see [brosdk-core](https://github.com/browsersdk/brosdk-core).

## Supported Platforms

BroSDK supports Windows, macOS, and Linux. This repository currently includes the following platform dynamic libraries:

| Platform | Dynamic Library |
|----------|-----------------|
| Windows x64 | `windows/brosdk.dll` |
| macOS ARM64 | `arm64-osx/libbrosdk.dylib` |

## Supported Browser Cores

For supported browser cores and versions, see:

https://github.com/browsersdk/brosdk-core

## Quick Start

### C

```c
#include <stdio.h>
#include "brosdk.h"

int main() {
    sdk_handle_t handle = NULL;
    char *out_data = NULL;
    size_t out_len = 0;

    int32_t ret = sdk_init(&handle, NULL, 0, &out_data, &out_len);
    if (!sdk_is_ok(ret)) {
        printf("SDK init failed: %s - %s\n", sdk_error_name(ret), sdk_error_string(ret));
        return ret;
    }

    printf("SDK initialized\n");

    sdk_info(&out_data, &out_len);
    printf("SDK Info: %.*s\n", (int)out_len, out_data);
    sdk_free(out_data);

    sdk_shutdown();
    return 0;
}
```

### C++

```cpp
#include "brosdk.h"

// Implement the ISDK interface to call environment management,
// browser control, and callback capabilities from a C++ project.
```

## Build And Link

### Windows

```bash
gcc -o app app.c -L./windows -lbrosdk
```

### macOS

```bash
gcc -o app app.c -L./arm64-osx -lbrosdk
```

## Directory Layout

```text
sdk/
├── brosdk.h           # Header file
├── windows/           # Windows dynamic library
│   └── brosdk.dll
└── arm64-osx/         # macOS ARM64 dynamic library
    └── libbrosdk.dylib
```
