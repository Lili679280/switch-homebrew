---
name: switch-homebrew
description: Nintendo Switch homebrew development with libnx and devkitPro. Use this skill whenever the user asks about Switch homebrew, libnx, devkitA64, NRO/NSP, Horizon OS, joycon input, Switch graphics/audio/networking, Amiibo/NFC on Switch, or anything related to writing C/C++ code for custom Switch applications. Also trigger when the user mentions homebrew games, Switch modding development, or references any libnx API (pad, framebuffer, audren, etc.).
---

# Switch Homebrew Development

You are a Nintendo Switch homebrew development expert. Write correct, idiomatic C/C++ for the Switch platform using the skill's bundled **libnx v4.11.1** release source and the **devkitPro** toolchain (devkitA64 GCC targeting ARMv8-A / Cortex-A57).

## Authoritative References

Before writing code that uses any libnx API, **verify the API signature**. The ground truth is the header files — never guess an API name, enum value, or struct field.

| Resource | Path |
|---|---|
| **Bundled libnx v4.11.1 release headers** (ground truth) | `references/libnx/nx/include/switch/` |
| **Bundled official examples** (usage patterns) | `references/switch-examples/` at commit `669786898205b7beb25ff1731e72982e6d0397d3` |
| `references/api-index.md` | Header file locator — which header for which subsystem |
| `references/examples-map.md` | Example path locator — which example demonstrates what |
| `references/button-icon-font.md` | Bundled Switch system button font asset, private-use Unicode glyphs, rendering and licensing guidance |

**Rule**: When the user asks about a specific subsystem, consult `references/api-index.md`, then read the actual file under `references/libnx/nx/include/switch/` to confirm the API. For usage patterns, consult `references/examples-map.md`, then read the actual bundled example under `references/switch-examples/`. Do not substitute external headers or examples unless the user explicitly requests a different version.

## Core Principles

### 1. Master Header
Always `#include <switch.h>` — it includes everything. Never include individual service headers directly.

### 2. Service Lifecycle
Every service follows the pattern: **Initialize → Use → Exit**

```c
Result rc = serviceXInitialize();
if (R_FAILED(rc)) fatalThrow(rc);
// ... use service ...
serviceXExit();
```

Object types follow: **Create → Use → Close**
```c
SomeObject obj;
someCreate(&obj, params);
// ... use obj ...
someClose(&obj);
```

### 3. Error Handling
Check every `Result` return value:
```c
if (R_SUCCEEDED(rc)) { /* success */ }
if (R_FAILED(rc)) fatalThrow(rc);   // crash with error display
if (R_FAILED(rc)) return rc;        // propagate to caller
```

### 4. Applet Main Loop
Every application uses this loop — it handles sleep/wake, HOME button, focus:
```c
while (appletMainLoop()) {
    // per-frame work here
}
```

### 5. Exit Convention
Press `HidNpadButton_Plus` to exit the application (return to hbmenu):
```c
if (kDown & HidNpadButton_Plus) break;
```

### 6. Naming Conventions (CODESTYLE.md)
- **Types**: `CapitalizedLikeThis` → `PadState`, `Framebuffer`, `AudioOutBuffer`
- **Enums**: `EnumType_EnumName` → `HidNpadButton_Plus`, `PIXEL_FORMAT_RGBA_8888`
- **Struct members**: `snake_case` → `buttons_cur`, `analog_stick_l`
- **Local variables**: `snake_case` → `kDown`, `player_count`
- **Globals**: `g_variableName` → `g_exitFlag`
- **Functions**: `modulenameVerbName` → `padUpdate`, `framebufferCreate`, `audoutPlayBuffer`
- **Macros**: `LIKE_THIS` → `BIT`, `MAKERESULT`, `R_SUCCEEDED`
- **Singleton lifecycle**: `moduleInitialize` / `moduleExit`
- **Object lifecycle**: `typeCreate` / `typeClose`

For the authoritative style rules, read `references/libnx/CODESTYLE.md`.

## Application Boilerplate

Every Switch homebrew application starts from this template:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <switch.h>

int main(int argc, char* argv[]) {
    consoleInit(NULL);

    padConfigureInput(1, HidNpadStyleSet_NpadStandard);
    PadState pad;
    padInitializeDefault(&pad);

    printf("Hello World!\n");

    while (appletMainLoop()) {
        padUpdate(&pad);
        u64 kDown = padGetButtonsDown(&pad);

        if (kDown & HidNpadButton_Plus) break;

        // Your per-frame logic here

        consoleUpdate(NULL);
    }

    consoleExit(NULL);
    return 0;
}
```

For projects using other graphics backends (framebuffer, OpenGL, deko3d), replace `consoleInit`/`consoleUpdate`/`consoleExit` with the appropriate rendering API.

## Project Setup and Makefile

### Required Toolchain
- **devkitPro** with `devkitA64`: `export DEVKITPRO=/opt/devkitpro` (or equivalent)
- Additional packages via `dkp-pacman`: `switch-mesa switch-glad switch-glm`, `switch-deko3d`, `switch-sdl2`, `switch-curl`, `switch-opusfile`, etc.

### Makefile Structure
Every project Makefile includes the Switch build rules and links against libnx:

```makefile
TARGET   := $(notdir $(CURDIR))
BUILD    := build
SOURCES  := source
DATA     := data
INCLUDES := include
#ROMFS   := romfs          # uncomment to embed RomFS assets

ARCH  := -march=armv8-a+crc+crypto -mtune=cortex-a57 -mtp=soft -fPIE
CFLAGS := -g -Wall -O2 -ffunction-sections $(ARCH) $(DEFINES)
CFLAGS += $(INCLUDE) -D__SWITCH__
CXXFLAGS := $(CFLAGS) -fno-rtti -fno-exceptions
ASFLAGS := -g $(ARCH)
LDFLAGS = -specs=$(DEVKITPRO)/libnx/switch.specs -g $(ARCH) -Wl,-Map,$(notdir $*.map)

LIBS    := -lnx
LIBDIRS := $(PORTLIBS) $(LIBNX)

include $(DEVKITPRO)/libnx/switch_rules
```

**Common LIBS additions:**
- OpenGL: `LIBS := -lglad -lEGL -lglapi -ldrm_nouveau -lnx`
- deko3d: `LIBS := -ldeko3d -lnx`
- SDL2: `LIBS := $(shell $(PORTLIBS)/bin/sdl2-config --libs) -lnx`
- libcurl: `LIBS := -lcurl -lnx`
- libopusfile: `LIBS := -lopusfile -lnx`

### Build Output
- Default: `.nro` file (homebrew executable)
- With `.json` config: `.nsp` file (sysmodule package)
- Build: `make`
- Clean: `make clean`

## Input — Reading Controllers

Use the high-level **Pad API** (`runtime/pad.h`) for controller input — it abstracts away the raw HID shared memory:

```c
// One-time setup
padConfigureInput(1, HidNpadStyleSet_NpadStandard);  // 1 player, standard styles
PadState pad;
padInitializeDefault(&pad);  // Player 1 + Handheld

// Per-frame (inside main loop)
padUpdate(&pad);
u64 kDown = padGetButtonsDown(&pad);   // newly pressed this frame
u64 kHeld = padGetButtons(&pad);       // currently held
u64 kUp   = padGetButtonsUp(&pad);     // newly released this frame

// Analog sticks
HidAnalogStickState stick_l = padGetStickPos(&pad, 0);  // left, range ±0x7FFF
HidAnalogStickState stick_r = padGetStickPos(&pad, 1);  // right
// stick_l.x, stick_l.y are s32 values

// Button repeat (for menus)
PadRepeater repeater;
padRepeaterInitialize(&repeater, 30, 5);  // 30 frame delay, 5 frame repeat
padRepeaterUpdate(&repeater, kHeld);
u64 kRepeat = padRepeaterGetButtons(&repeater);
u64 kCombined = kDown | kRepeat;
```

**Touch screen:**
```c
hidInitializeTouchScreen();
HidTouchScreenState state = {0};
hidGetTouchScreenStates(&state, 1);
for (s32 i = 0; i < state.count; i++) {
    u32 x = state.touches[i].x;
    u32 y = state.touches[i].y;
}
```

For button masks and input structs, read the bundled `references/libnx/nx/include/switch/services/hid.h` and `references/libnx/nx/include/switch/runtime/pad.h` headers.

## Graphics — Three Tiers

### Tier 1: Console Text (printf)
Simplest approach — good for tools, debug, text-heavy apps:
```c
consoleInit(NULL);
printf("Hello World!\n");
consoleUpdate(NULL);  // flush to display (call once per frame)
consoleExit(NULL);
```

### Tier 2: Software Framebuffer
Direct pixel access, no GPU — good for 2D pixel art, software rendering:
```c
NWindow* win = nwindowGetDefault();
Framebuffer fb;
framebufferCreate(&fb, win, 1280, 720, PIXEL_FORMAT_RGBA_8888, 2);
framebufferMakeLinear(&fb);

while (appletMainLoop()) {
    u32 stride;
    u32* buf = (u32*)framebufferBegin(&fb, &stride);
    // Pixel (x,y): buf[y * (stride/4) + x] = RGBA8(r,g,b,a);
    framebufferEnd(&fb);
}
framebufferClose(&fb);
```

For pixel format and framebuffer details, read the bundled `references/libnx/nx/include/switch/display/framebuffer.h` header.

### Tier 3: GPU-Accelerated (OpenGL, deko3d, SDL2)
Use for games with complex rendering. See examples at:
- OpenGL: `graphics/opengl/simple_triangle/` — EGL context, GLES2
- deko3d: `graphics/deko3d/deko_basic/` — low-level modern GPU API
- SDL2: `graphics/sdl2/sdl2-simple/` — cross-platform abstraction

**Note**: Console text and GPU rendering cannot share the same display layer. For GPU projects, use GPU-based text or nxlink for debug output.

### Switch System Button Prompts

When an NRO UI needs Nintendo Switch system button prompts, read `references/button-icon-font.md`. Use the bundled `assets/fonts/NintendoSwitchButtonIcons.ttf` glyphs through the project's TTF-compatible renderer instead of drawing, tracing, or generating replacement button artwork. Keep the displayed glyph mapping separate from `HidNpadButton_*` input handling and never invent an unverified codepoint mapping.

## Audio — Two Approaches

### Simple Playback (audout)
For tone generation, simple PCM playback:
```c
audoutInitialize();
audoutStartAudioOut();

AudioOutBuffer buf = {
    .next = NULL,
    .buffer = audio_data,
    .buffer_size = total_size,
    .data_size = data_size,
    .data_offset = 0,
};
AudioOutBuffer* released = NULL;
audoutPlayBuffer(&buf, &released);
// Wait for buffer to release before reusing:
// audoutWaitPlayFinish(&released, &released_count, U64_MAX);

audoutStopAudioOut();
audoutExit();
```

### Audio Renderer (audren)
For multi-voice mixing, effects, streaming:
- See example: `audio/audren-simple/`
- Supports voices, effects, mixing, sink configuration
- Output rates: 32000 Hz or 48000 Hz

## Filesystem — Three Sources

### SD Card
```c
fsdevMountSdmc();
FILE* f = fopen("sdmc:/data.txt", "r");
// ... use file ...
fclose(f);
fsdevUnmountDevice("sdmc");
```

### RomFS (Embedded Assets)
In Makefile: `ROMFS := romfs` — place files in `romfs/` directory.
```c
romfsInit();
FILE* f = fopen("romfs:/asset.bin", "rb");
// ... use file ...
fclose(f);
romfsExit();
```

### Save Data
```c
// Get user account
AccountUid uid;
accountInitialize(AccountServiceType_Application);
accountGetPreselectedUser(&uid);
accountExit();

// Mount save data
fsdevMountSaveData("save", application_id, uid);
FILE* f = fopen("save:/progress.dat", "r+b");
// ... use file ...
fsdevCommitDevice("save");   // persist changes
fsdevUnmountDevice("save");
```

## Networking

### BSD Sockets
```c
socketInitializeDefault();
int sock = socket(AF_INET, SOCK_STREAM, 0);
// Standard POSIX socket API
close(sock);
socketExit();
```

### HTTP with libcurl
Makefile: `LIBS := -lcurl -lnx`
```c
socketInitializeDefault();
curl_global_init(CURL_GLOBAL_DEFAULT);
CURL* curl = curl_easy_init();
curl_easy_setopt(curl, CURLOPT_URL, "https://example.com/");
curl_easy_perform(curl);
curl_easy_cleanup(curl);
curl_global_cleanup();
socketExit();
```

### Local Wireless (LDN)
For local multiplayer — see example: `network/ldn/`

## Applets — System UI Dialogs

Use these to launch official system UIs:

**Software Keyboard:**
```c
SwkbdConfig kbd;
swkbdCreate(&kbd, 0);
swkbdConfigMakePresetDefault(&kbd);
swkbdConfigSetGuideText(&kbd, "Enter name:");
char buf[256] = {0};
Result rc = swkbdShow(&kbd, buf, sizeof(buf));
swkbdClose(&kbd);
// User input is now in buf (if R_SUCCEEDED(rc))
```

**Web Browser:**
```c
WebCommonConfig config;
webPageCreate(&config, "https://example.com");
webConfigShow(&config, NULL);
```

**Error Display:**
```c
errorApplicationCreate(&config, "Error Title", "Error description text");
errorConfigShow(&config);
```

**Player Selector:**
```c
pselShowUserSelector();  // Let user pick an account
```

## Threading and Synchronization

```c
// Thread creation
Thread thread;
threadCreate(&thread, threadFunc, arg, NULL, 0x8000, 0x2C, -2);
threadStart(&thread);
threadWaitForExit(&thread);
threadClose(&thread);

// Synchronization primitives (all in kernel/*.h):
// Mutex, Event, UEvent, CondVar, RWLock, Semaphore, Barrier

Mutex mtx;
mutexInit(&mtx);
mutexLock(&mtx);
// critical section
mutexUnlock(&mtx);
```

## Other Subsystems

**NFC/Amiibo** — See `services/nfc.h` and example `nfc/`:
```c
nfcInitialize(NfcServiceType_User);
// nfcListDevices, nfcStartDetection, nfcMount, nfcGetTagInfo, etc.
```

**System Info** — Use `set` and `hosversion`:
```c
setInitialize();
u64 langCode = 0;
setsysGetLanguageCode(&langCode);
setExit();

u32 ver = hosversionGet();
if (hosversionAtLeast(14,0,0)) { /* feature available */ }
```

**Battery/Power** — Use `psm`:
```c
psmInitialize();
u32 charge_pct = 0;
psmGetBatteryChargePercentage(&charge_pct);
psmExit();
```

**Exception Handler (Crash Dump)** — See example `exception-handler/`:
```c
// Write crash info to SD card for debugging
```

## When to Read Reference Files

- **Before writing HID/input code** → `references/api-index.md`, then the indexed bundled headers and examples
- **Before writing framebuffer code** → `references/api-index.md`, then `references/libnx/nx/include/switch/display/framebuffer.h` and the indexed example
- **Before checking results** → `references/libnx/nx/include/switch/result.h`
- **Before version-gating features** → `references/libnx/nx/include/switch/runtime/hosversion.h`
- **Before displaying Switch system button icons** → `references/button-icon-font.md` for the bundled TTF, Unicode glyph workflow, and mapping checks
- **To find the right example** → `references/examples-map.md`
- **To find the right header** → `references/api-index.md`

For any API not covered in this skill, read the actual header file — the ground truth is always the source.
