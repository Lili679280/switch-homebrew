# libnx API Index — Subsystem → Key Headers

All paths relative to libnx root:
Bundled libnx v4.12.0 signed-tag header root: `references/libnx/nx/include/switch/`. Resolve every entry below relative to this skill directory. Do not use an external libnx installation unless the user explicitly requests another version.

The master header `<switch.h>` includes everything. This index helps locate the relevant header to verify specific API signatures before writing code.

## System & Kernel

| Subsystem | Header | Key API |
|---|---|---|
| Syscalls | `kernel/svc.h` | All kernel SVC wrappers |
| Threads | `kernel/thread.h` | `threadCreate`, `threadStart`, `threadWaitForExit` |
| Mutex | `kernel/mutex.h` | `mutexInit`, `mutexLock`, `mutexUnlock` |
| Events | `kernel/event.h` | `eventCreate`, `eventWait`, `eventFire` |
| Wait | `kernel/wait.h` | `waitMulti` — wait on multiple objects |
| Semaphore | `kernel/semaphore.h` | `semaphoreInit`, `semaphoreWait`, `semaphorePost` |
| Shared memory | `kernel/shmem.h` | `shmemCreate`, `shmemMap` |
| Virtual memory | `kernel/virtmem.h` | `virtmemReserve`, `virtmemFree` |
| Random | `kernel/random.h` | `randomGet64`, `randomGetBytes` |
| Detection | `kernel/detect.h` | `detectDebugger`, `detectGamecard` |

## Services

### Core

| Service | Header | Key API |
|---|---|---|
| Service Manager | `services/sm.h` | `smInitialize`, `smGetService` |
| Applet (app lifecycle) | `services/applet.h` | `appletMainLoop`, `appletGetOperationMode` |
| Filesystem | `services/fs.h` | `FsFileSystem`, `FsFile`, `FsDir`, save data |
| Settings | `services/set.h` | `setsysGetLanguageCode`, `setsysGetFirmwareVersion` |

### Input

| Service | Header | Key API |
|---|---|---|
| HID (controllers) | `services/hid.h` | Npad buttons, touch, sixaxis, vibration, IR |
| HID bus | `services/hidbus.h` | External sensor registration |
| HID debug | `services/hiddbg.h` | Debug pad, controller simulation |

### Graphics & Display

| Service | Header | Key API |
|---|---|---|
| VI (display) | `services/vi.h` | Display info, vsync events |
| Backlight | `services/lbl.h` | Screen backlight control |
| Framebuffer | `display/framebuffer.h` | `framebufferCreate`, `Begin`, `End` |
| Native Window | `display/native_window.h` | `NWindow`, `nwindowGetDefault` |
| NVIDIA GPU | `nvidia/gpu.h`, `nvidia/channel.h` | Low-level GPU access |

### Audio

| Service | Header | Key API |
|---|---|---|
| Audio output | `services/audout.h` | Simple PCM playback |
| Audio input | `services/audin.h` | Microphone capture |
| Audio renderer | `services/audren.h` | Multi-voice, effects, mixing |
| Audio control | `services/audctl.h` | Volume, mute |

### Network

| Service | Header | Key API |
|---|---|---|
| BSD sockets | `runtime/devices/socket.h` | `socketInitializeDefault`, `socketExit` |
| DNS resolver | `runtime/resolver.h` | DNS resolution helper |
| NIFM (WiFi) | `services/nifm.h` | Network interface, internet connection |
| SSL/TLS | `services/ssl.h` | Encrypted connections |

### Storage & Content

| Service | Header | Key API |
|---|---|---|
| NCM (content manager) | `services/ncm.h` | Content storage management |
| NS (application control) | `services/ns.h` | Title info, system update |
| Account | `services/acc.h` | User profiles, UID management |

### Communication & Peripherals

| Service | Header | Key API |
|---|---|---|
| NFC/Amiibo | `services/nfc.h` | Amiibo tag detection, reading |
| Bluetooth | `services/bt.h`, `services/btdrv.h` | BLE, pairing, GATT |
| LDN (local play) | `services/ldn.h` | Local wireless network, scan, connect |
| USB | `services/usb.h`, `services/usbds.h` | USB device/host |

### Applets (System UI)

| Service | Header | Key API |
|---|---|---|
| Applet framework | `applets/libapplet.h` | Applet lifecycle |
| Software keyboard | `applets/swkbd.h` | `SwkbdConfig`, `swkbdCreate`, `swkbdShow` |
| Error display | `applets/error.h` | `errorApplicationCreate`, `errorConfigShow` |
| Web browser | `applets/web.h` | `WebCommonConfig`, `webPageCreate` |
| Player selector | `applets/psel.h` | `pselShowUserSelector` |
| NFC applet | `applets/nfp_la.h` | Amiibo via library applet |

### Other Services

| Service | Header | Key API |
|---|---|---|
| Power/battery | `services/psm.h` | Battery charge, charger type |
| Time | `services/time.h` | System clock |
| Notifications | `services/notif.h` | Alarm notifications |
| Parental controls | `services/pctl.h` | Play time limits |
| Crypto (SPL) | `services/spl.h` | Hardware crypto / secure monitor |
| Fatal error | `services/fatal.h` | Fatal error display |

## Runtime Helpers

| Header | Key API |
|---|---|
| `runtime/pad.h` | `padConfigureInput`, `PadState`, `padUpdate`, `padGetButtons*`, `padGetStickPos` |
| `runtime/devices/console.h` | `consoleInit`, `consoleUpdate`, `consoleExit` |
| `runtime/devices/fs_dev.h` | `fsdevMountSdmc`, `fsdevMountSaveData`, `fsdevUnmountDevice` |
| `runtime/devices/romfs_dev.h` | `romfsInit`, `romfsExit` |
| `runtime/hosversion.h` | `hosversionGet`, `hosversionAtLeast`, `hosversionBefore` |
| `runtime/nxlink.h` | Network debug stdio redirection |

## Data Formats

| Header | Purpose |
|---|---|
| `types.h` | `u8`-`u64`, `s8`-`s64`, `Handle`, `Result`, `BIT`, `BITL` macros |
| `result.h` | `MAKERESULT`, `R_SUCCEEDED`, `R_FAILED`, `R_MODULE`, `R_DESCRIPTION` |
| `nro.h` | NRO executable format structures |
| `nacp.h` | Application metadata format |
