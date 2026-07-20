# Switch Homebrew Skill

[English](#english) | [中文](#中文)

## English

A self-contained Codex skill for Nintendo Switch homebrew development with libnx and devkitPro. It bundles the official libnx headers, official Switch examples, API navigation guides, and a Switch button-icon font workflow so generated C/C++ code can be checked against local, versioned references.

### Highlights

- Bundled libnx **v4.11.1 release** as the API source of truth.
- Bundled official `switchbrew/switch-examples` at commit `669786898205b7beb25ff1731e72982e6d0397d3`.
- Covers NRO applications, input, framebuffer, SDL2, OpenGL, deko3d, audio, filesystems, networking, applets, NFC, threading, and other libnx subsystems.
- Requires API signatures to be verified against the bundled headers instead of guessed.
- Includes `NintendoSwitchButtonIcons.ttf` for non-commercial Switch-style button prompts; the skill instructs agents to use font glyphs rather than redraw button artwork.
- Uses only paths relative to the skill directory. It does not depend on a specific user directory or local devkitPro installation for reference lookup.

### Installation

Clone directly into your personal Codex skills directory:

```powershell
git clone https://github.com/Lili679280/switch-homebrew.git "$env:USERPROFILE\.codex\skills\switch-homebrew"
```

If the destination already exists, update it from inside that directory:

```powershell
git pull
```

Restart Codex or begin a new task after installation so the skill can be discovered.

### Trigger examples

- “Create a Switch NRO menu with libnx.”
- “Read Joy-Con input and show the matching system button glyph.”
- “Render a custom framebuffer on Switch.”
- “Add NFC/Amiibo support to this homebrew app.”
- “Build an SDL2 interface for a Switch homebrew tool.”

### Reference flow

```text
SKILL.md
  -> references/api-index.md
  -> references/libnx/nx/include/switch/
  -> references/examples-map.md
  -> references/switch-examples/
```

Button prompts additionally use:

```text
references/button-icon-font.md
assets/fonts/NintendoSwitchButtonIcons.ttf
```

### Repository layout

```text
switch-homebrew/
├─ SKILL.md
├─ assets/fonts/
│  ├─ NintendoSwitchButtonIcons.ttf
│  ├─ LICENSE.txt
│  └─ README.md
└─ references/
   ├─ libnx/
   ├─ switch-examples/
   ├─ api-index.md
   ├─ examples-map.md
   └─ button-icon-font.md
```

### Licensing

Original skill documentation is released under the MIT License. Bundled third-party materials retain their own terms. See [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) and the license files within each bundled component.

This is an unofficial, non-commercial community project. Nintendo, Switch, Joy-Con, and related names are trademarks of their respective owners. Nintendo does not sponsor or endorse this repository.

---

## 中文

这是一个面向 Nintendo Switch 自制程序开发的自包含 Codex skill，主要用于 libnx 和 devkitPro C/C++ 项目。仓库内置官方 libnx 头文件、官方 Switch 示例、API 导航以及 Switch 按钮图标字体工作流，让模型能够依据固定版本的本地资料核对代码，而不是猜测 API。

### 主要功能

- 内置 libnx **v4.11.1 正式 Release**，作为 API 的唯一事实来源。
- 内置官方 `switchbrew/switch-examples`，固定提交为 `669786898205b7beb25ff1731e72982e6d0397d3`。
- 覆盖 NRO、手柄输入、Framebuffer、SDL2、OpenGL、deko3d、音频、文件系统、网络、系统 Applet、NFC 和线程等子系统。
- 要求模型读取内置头文件核对函数签名，不能凭记忆猜测 API。
- 包含 `NintendoSwitchButtonIcons.ttf`，用于非商业 Switch 按钮提示；skill 会要求模型使用字体字形，不自行绘制按钮图案。
- 全部参考路径均相对于 skill 根目录，不依赖某个用户名、下载目录或本机 devkitPro 安装位置。

### 安装方法

直接克隆到个人 Codex skills 目录：

```powershell
git clone https://github.com/Lili679280/switch-homebrew.git "$env:USERPROFILE\.codex\skills\switch-homebrew"
```

如果目录已经存在，请进入该目录后更新：

```powershell
git pull
```

安装后重新启动 Codex，或创建一个新任务，以便系统重新发现该 skill。

### 触发示例

- “使用 libnx 创建一个 Switch NRO 菜单。”
- “读取 Joy-Con 输入并显示对应的系统按钮图标。”
- “在 Switch 上绘制自定义 Framebuffer。”
- “给这个自制程序添加 NFC/Amiibo 支持。”
- “使用 SDL2 制作 Switch 自制工具界面。”

### 资料读取流程

```text
SKILL.md
  -> references/api-index.md
  -> references/libnx/nx/include/switch/
  -> references/examples-map.md
  -> references/switch-examples/
```

按钮提示额外使用：

```text
references/button-icon-font.md
assets/fonts/NintendoSwitchButtonIcons.ttf
```

### 目录结构

```text
switch-homebrew/
├─ SKILL.md
├─ assets/fonts/
│  ├─ NintendoSwitchButtonIcons.ttf
│  ├─ LICENSE.txt
│  └─ README.md
└─ references/
   ├─ libnx/
   ├─ switch-examples/
   ├─ api-index.md
   ├─ examples-map.md
   └─ button-icon-font.md
```

### 许可证

本项目原创的 skill 文档采用 MIT License。内置第三方内容继续遵循各自的授权条件，详情见 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) 以及各组件自带的许可证文件。

本项目是非官方、非商业的社区项目。Nintendo、Switch、Joy-Con 等名称及商标属于各自权利人。本仓库不受 Nintendo 赞助或认可。
