# Switch Homebrew Skill

[![Codex Skill](https://img.shields.io/badge/Codex-Skill-111827?logo=openai&logoColor=white)](https://github.com/Lili679280/switch-homebrew)
[![libnx](https://img.shields.io/badge/libnx-v4.11.1-2563eb)](https://github.com/switchbrew/libnx/releases/tag/v4.11.1)
[![许可证：MIT](https://img.shields.io/badge/许可证-MIT-green.svg)](LICENSE)

[English](README.md) | **简体中文**

这是一个面向 Nintendo Switch 自制程序开发的自包含 Codex Skill，适用于 libnx 与 devkitPro C/C++ 项目。它为 Codex 提供带有明确版本的本地参考资料，让模型根据真实头文件编写和检查代码，不依赖猜测的 API 或某台电脑上的固定路径。

> 本仓库是非官方社区项目，与 Nintendo 没有隶属、赞助或认可关系。

## 快速开始

把仓库克隆到个人 Codex Skills 目录：

```powershell
git clone https://github.com/Lili679280/switch-homebrew.git "$env:USERPROFILE\.codex\skills\switch-homebrew"
```

安装后重新启动 Codex，或者新建一个任务，让 Codex 重新发现这个 Skill。之后正常描述需求即可，例如：

```text
使用 libnx 创建一个 Switch NRO 菜单，并显示系统风格的 A/B 按钮图标。
```

当需求涉及 Switch 自制程序、libnx、devkitA64、NRO、Joy-Con 输入、图形、音频、网络或 NFC/Amiibo 等内容时，这个 Skill 会自动适用。

## 为什么要使用这个 Skill

Switch 自制程序 API 很容易记错，而且在线资料未必与项目使用的版本一致。这个 Skill 把参考资料直接保存在仓库中，并要求 Codex 在生成代码前核对内置头文件中的真实函数签名。

- 不写死任何人的电脑路径
- 使用内置资料时不需要联网查询
- 日常使用不依赖可能随时变化的外部仓库分支
- libnx 头文件与官方示例组成固定、可复现的参考组合

## 工作方式

1. `SKILL.md` 告诉 Codex 何时以及如何处理 Switch 自制程序任务。
2. `references/api-index.md` 把模型引导到相应的内置 libnx 头文件。
3. `references/libnx/` 中的头文件是 API 的事实依据。
4. 需要实际用法时，`references/examples-map.md` 会把模型引导到对应的官方内置示例。
5. 需要系统按钮提示时，使用内置 TTF 与映射说明，不让模型自行绘制按钮图案。

## 内置内容

| 组件 | 内置版本 | 用途 |
|---|---:|---|
| [libnx](https://github.com/switchbrew/libnx) | v4.11.1 Release | 权威头文件、源码与代码风格规范 |
| [switch-examples](https://github.com/switchbrew/switch-examples) | `669786898205b7beb25ff1731e72982e6d0397d3` | 官方使用示例 |
| `NintendoSwitchButtonIcons.ttf` | 内置资源 | Switch 风格的系统按钮提示 |
| API 与示例索引 | 当前仓库版本 | 快速找到正确的内置文件 |

这些参考资料是有意固定版本的，不会自行更新。这样可以保证模型生成的代码与文档注明的版本保持一致。

## 支持范围

- NRO 程序结构与 devkitPro Makefile
- Pad、HID 与 Joy-Con/控制器输入
- Console、软件 Framebuffer、SDL2、OpenGL 与 deko3d
- Audio Output 与 Audio Renderer
- SD 卡、RomFS 与存档数据
- BSD Socket、libcurl 与本地无线通信
- 系统 Applet、软键盘、浏览器与用户选择器
- NFC/Amiibo、系统信息、电池状态、线程与同步

## 按钮图标字体

显示 Switch 系统按钮提示时，Skill 会使用：

```text
assets/fonts/NintendoSwitchButtonIcons.ttf
references/button-icon-font.md
```

这个字体使用 Unicode 私用区字符。项目需要通过支持 TTF 的渲染器加载字体，并采用经过验证的码位映射。用于显示的字体字形与用于读取输入的 `HidNpadButton_*` 掩码是两套不同的概念，Skill 不允许模型凭空猜测映射关系。

## 仓库结构

```text
switch-homebrew/
├── SKILL.md
├── README.md
├── README.zh-CN.md
├── assets/
│   └── fonts/
│       ├── NintendoSwitchButtonIcons.ttf
│       ├── LICENSE.txt
│       └── README.md
└── references/
    ├── libnx/
    ├── switch-examples/
    ├── api-index.md
    ├── examples-map.md
    └── button-icon-font.md
```

## 更新方式

更新已经安装的副本：

```powershell
Set-Location "$env:USERPROFILE\.codex\skills\switch-homebrew"
git pull
```

内置的上游资料只应在明确需要时手动替换。更新 libnx 或 switch-examples 时，需要固定准确的 Release 或提交号、保留许可证信息，并同步修改仓库中所有版本说明。

## 参与贡献

欢迎提交能够提高正确性、导航体验或子系统覆盖范围的改进。提交 Pull Request 前请阅读 [CONTRIBUTING.md](CONTRIBUTING.md)，安全相关问题请按照 [SECURITY.md](SECURITY.md) 说明报告。

## 许可证与第三方内容

本项目原创的 Skill 指令和仓库文档采用 [MIT License](LICENSE)。内置的第三方内容继续遵循各自的许可条款，详情请查看 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) 以及各组件目录中的许可证文件。

Nintendo、Nintendo Switch、Joy-Con 及相关名称和标志归各自权利人所有。使用者需要自行确认对内置资源的使用与再分发符合适用的许可条款和法律规定。
