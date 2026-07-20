# Examples Map — Subsystem → Example Path

This maps each Switch development topic to the official example that demonstrates it.
All paths are relative to the bundled official examples root at `references/switch-examples/`, pinned to commit `669786898205b7beb25ff1731e72982e6d0397d3` from `switchbrew/switch-examples`.

When you need to see how a subsystem works in practice, read the corresponding example's `source/main.c` file.

## Graphics

| Topic | Example Path | Key Files |
|---|---|---|
| Console text (printf) | `graphics/printing/hello-world/` | `source/main.c` |
| VT52 escape codes | `graphics/printing/vt52-demo/` | `source/main.c` |
| Software framebuffer | `graphics/simplegfx/` | `source/main.c` |
| Framebuffer movie maker | `graphics/simplegfx_moviemaker/` | `source/main.c` |
| Shared system font | `graphics/shared_font/` | `source/main.c` |
| OpenGL ES2 triangle | `graphics/opengl/simple_triangle/` | `source/main.c` |
| OpenGL textured cube | `graphics/opengl/textured_cube/` | `source/main.c` |
| OpenGL es2gears | `graphics/opengl/es2gears/` | `source/main.c` |
| OpenGL GPU console | `graphics/opengl/gpu_console/` | `source/main.c` |
| OpenGL dynamic resolution | `graphics/opengl/dynamic_resolution/` | `source/main.c` |
| deko3d basic (triangle) | `graphics/deko3d/deko_basic/` | `source/main.c` |
| deko3d GPU console | `graphics/deko3d/deko_console/` | `source/main.c` |
| deko3d advanced examples | `graphics/deko3d/deko_examples/` | C++ multi-file |
| SDL2 simple | `graphics/sdl2/sdl2-simple/` | `source/main.c` |
| SDL2 demo (image, audio, font) | `graphics/sdl2/sdl2-demo/` | `source/main.c` |

## Input (HID)

| Topic | Example Path |
|---|---|
| Button reading (PadState) | `hid/read-controls/` |
| Abstracted pad (multi-controller) | `hid/abstracted-pad/` |
| Touch screen | `hid/touch-screen/` |
| Six-axis sensor (gyro/accel) | `hid/sixaxis/` |
| SevenSixAxis sensor | `hid/sevensixaxis/` |
| Vibration (HD Rumble) | `hid/vibration/` |
| Joy-Con LED colors | `hid/colors/` |
| IR sensor (right Joy-Con camera) | `hid/irsensor/` |
| Notification LED | `hid/notification-led/` |
| Ring-Con (Ring Fit) | `hid/ring-con/` |
| Handheld detection | `hid/hdls/` |

## Audio

| Topic | Example Path |
|---|---|
| Sine wave tone (audout) | `audio/playtone/` |
| Audio renderer (audren) | `audio/audren-simple/` |
| Hardware Opus decoder | `audio/hwopus-decoder/` |
| Audio echo/loopback | `audio/echo/` |
| SDL2_mixer (MP3) | `audio/sdl2-audio/` |

## Filesystem

| Topic | Example Path |
|---|---|
| SD card read/write | `fs/sdmc/` |
| RomFS (embedded data) | `fs/romfs/` |
| Save data (per-user) | `fs/save/` |

## Network

| Topic | Example Path |
|---|---|
| HTTP GET (libcurl) | `network/curl/` |
| LDN (local wireless play) | `network/ldn/` |
| LP2P (peer-to-peer) | `network/lp2p/` |
| nxlink stdio (remote debug) | `network/nxlink_stdio/` |

## Applets (System Dialogs)

| Topic | Example Path |
|---|---|
| Software keyboard | `applet/libapplets/software-keyboard/` |
| Web browser | `applet/libapplets/web/` |
| Error display | `applet/libapplets/error/` |
| Parental controls auth | `applet/libapplets/pctlauth/` |
| Light sensor | `applet/light-sensor/` |
| Sleep lock / lock-exit | `applet/lockexit/` |
| Gameplay recording | `applet/recording/` |
| Screenshot overlay | `applet/screenshot-overlay/` |
| VR mode | `applet/vrmode/` |
| Play statistics | `applet/app-playstats/` |

## System & Misc

| Topic | Example Path |
|---|---|
| User account / profile | `account/` |
| System language | `settings/get_system_language/` |
| Battery / power status | `power/` |
| Time (UTC) | `time/` |
| NFC / Amiibo | `nfc/` |
| Alarm notifications | `alarm-notifications/` |
| Application control data | `app_controldata/` |
| Exception handler (crash dump) | `exception-handler/` |
| Threads & barriers | `misc/barrier/` |
| User events (UEvent) | `misc/user_events/` |
| User timers (UTimer) | `misc/user_timers/` |
| JIT compilation | `jit/` |
| USB host | `usb/` |

## Project Templates

| Topic | Path | Notes |
|---|---|---|
| Application (NRO) | `templates/application/` | Standard homebrew app |
| Library (static .a) | `templates/library/` | Reusable library |
| Sysmodule (NSP) | `templates/sysmodule/` | System module |
