# VexlonAI

> 你的 AI，住在你的设备里。丰富的功能带来一站式 AI 解决方案，会聊、会写，更会替你动手。

VexlonAI 是一款**本地优先**的多功能 AI 软件，装在你的电脑或手机上。它会帮你回答问题、写文章、整理资料等等，开启 Agent 模式后还能真正动手——创建/修改文件、执行操作、上网查资料。

数据在**你自己的设备上**，AI 要操作文件或执行任务前都会先征求你的同意。

***

## 特性

- **任意模型** —— 内置 DeepSeek / 豆包 / 小米 MiMo / 百度千帆（注意是以上模型通过兼容测试，为官方推荐模型，模型不是由软件提供），并支持任意 **OpenAI 兼容**服务

- **多智能体** —— 创建多个智能体，各自拥有独立设定、模型与记忆；支持让它们一同讨论的**群聊模式**。

- **Agent 模式** —— 会聊、会写，更会替你动手

- **长期记忆** —— 跨会话记住关于你和对话的重要信息

- **定时任务** —— 例如「每天早上 8 点给我一份新闻摘要」，到点自动执行。

- **消息渠道**—— 绑定微信/飞书/钉钉（部分渠道正在开发中）

- **扩展系统** —— 导入 `.zip` 扩展包即可增加新功能，无需等待官方把功能加进去

- **技能系统** —— 导入技能（文件夹 / 单文件 / 压缩包），对话中用手动激活或由 AI 调用。

- **知识库** —— 保存文件供 AI 搜索、读取与写入。

- **设备控制** —— 让 AI 操作已连接或本机的手机 / 电脑。

- **内置浏览器** —— 面板式网页浏览与AI控制

- **语音识别与语音生成**—— 支持豆包 / 硅基流动两种语音识别服务。

- **生图 / 生视频** —— 可调用 AI 生成图片与视频。

- **自定义外观** —— 这个没啥好说的

- **丰富的文档** —— 即使你不懂API也能五分钟上手-<https://docs.vexlon.qulin.xyz/>

***

##  平台支持

| 平台            | 状态  | 目录            | 产物     |
| ------------- | --- | ------------- | ------ |
| Windows       | 已支持 | `app/windows` | `.exe` |
| macOS         | 已支持 | `app/macos`   | `.app` |
| Android（安卓）   | 已支持 | `app/android` | `.apk` |
| iOS           | 计划中 | —             | `.ipa` |
| HarmonyOS（鸿蒙） | 计划中 | —             | `.hap` |

> 鸿蒙用户当前可先用系统自带的「卓易通」运行安卓版。早期（2026年3月的）网页版已不再对外提供。

***

## 工程结构

本仓库为 Dart **monorepo**：`app` 是 Flutter 主应用，`packages/` 下按职责拆分为 7 个可独立测试的包（命名统一为 `vexlonai_*`）。

```
VexlonAI/
├── app/                     # Flutter 主应用（vexlonai）
│   ├── lib/
│   │   ├── browser/         # 内置浏览器控制
│   │   ├── data/            # 数据目录、第三方用途声明
│   │   ├── device/          # 设备控制（桥、控制器、工具）
│   │   ├── extensions/      # 扩展管理与加载
│   │   ├── messaging/       # 消息渠道（含微信 iLink）
│   │   ├── pages/           # 各功能页面（聊天 / 群聊 / 设置 / 知识库 / 技能 …）
│   │   ├── services/        # 系统服务（语音识别等）
│   │   ├── skills/          # 技能管理与工具
│   │   ├── state/           # 状态管理（chat_controller、task_runner）
│   │   ├── widgets/         # UI 组件（各类 sheet / 气泡 / markdown 等）
│   │   ├── app.dart         # 应用入口与主题、系统栏适配
│   │   └── main.dart
│   └── test/                # 组件与逻辑测试
├── packages/                # 核心能力包
│   ├── core/                # 数据模型与 JSON 文件存储、HTTP 客户端
│   ├── ai/                  # AI 层：Agent 循环、LLM 传输、系统提示、工具协议、读图
│   ├── tools/               # 内置工具：工作区 / 知识库 / 记忆 / 生图 / 生视频
│   ├── device/              # 设备控制协议与桥接
│   ├── scheduler/           # 定时任务调度
│   ├── mc/                  # MCP 协议桥（Numen MCP 客户端）
│   └── wechat/              # 微信 iLink 机器人接入
├── tool/                    # 维护脚本
│   ├── add_license_headers.sh      # 批量补齐源码 SPDX 版权头
│   └── gen_third_party_notices.sh  # 生成第三方许可清单
├── LICENSE                  # BSD 3-Clause
└── THIRD_PARTY_NOTICES.md   # 第三方依赖许可（自动生成）
```

包的依赖方向：`ai / tools / device / scheduler / mc / wechat` → `core`（`core` 无内部依赖）。

***

## 环境要求

- **Flutter**：stable 渠道

- **Dart SDK**：`^3.13.0`

***

## 快速开始

```bash
cd VexlonAI/app

# 拉取依赖
flutter pub get

# 运行（按需选择设备）
flutter run -d macos
flutter run -d windows
flutter run -d <android-device-id>
```

首次启动后，进入「设置 → API 设置」选择服务商并填写密钥；若使用其他服务商，服务商选 **OpenAI 兼容**，手动填写接口地址、密钥与模型名即可。

新手吗？超详细官方文档<https://docs.vexlon.qulin.xyz/>

***

## 构建

```bash
cd VexlonAI/app

flutter build macos
flutter build windows
flutter build apk
```

***

## 测试与静态检查

主应用使用 `flutter analyze` / `flutter test`；`packages/` 下为纯 Dart 包，使用 `dart test`：

```bash
# 主应用
cd VexlonAI/app
flutter analyze
flutter test

# 各核心包
cd VexlonAI/packages/ai && dart test
cd VexlonAI/packages/core && dart test
# 其余包同理：tools / device / scheduler / mc
```

***

## 文档

- 包含介绍、快速开始、多智能体、群聊、Agent 模式、记忆、定时任务、微信接入、扩展、技能、知识库、架构、LLM 传输层、Agent 循环、MCP、扩展 API、数据管理、常见问题等

- 在线文档：<https://docs.vexlon.qulin.xyz/>

***

## 第三方许可

本项目依赖的第三方开源库及其许可证汇总见 [THIRD\_PARTY\_NOTICES.md](THIRD_PARTY_NOTICES.md)（由脚本自动生成）。应用内也可通过「设置 → 关于 → 查看第三方许可」查看同样的内容。

***

## 许可证

本项目基于 **BSD 3-Clause** 许可证开源，详见 [LICENSE](LICENSE)。

Copyright (c) 2026, 屈霖 (Qulin) <qulin@qulin.xyz>
