# Switch System Button Icon Font

## Required asset

Use the bundled font:

```text
assets/fonts/NintendoSwitchButtonIcons.ttf
```

It is a renamed, byte-identical copy of the supplied `NintendoExt003.ttf`. It is approximately 99 KiB and contains Switch-related icon glyphs in Unicode private-use codepoints, primarily within `U+E000` through `U+E152` with gaps.

## Core rule

For Nintendo Switch system button prompts, use glyphs from this font instead of drawing, tracing, approximating, or image-generating replacement button icons. Reuse an existing project's established button-icon system when it already has one.

The font provides appearance only. A string such as `"Press \uE0E2"` selects a font glyph; it does not read controller input. Use the corresponding verified `HidNpadButton_*` value separately for input.

Never guess which private-use codepoint represents A, B, X, Y, Plus, Minus, sticks, shoulders, or Joy-Con variants. Inspect the font or use a mapping already verified by the target project.

## Project integration

Copy the asset into the NRO project, preserving the English filename:

```text
romfs/fonts/NintendoSwitchButtonIcons.ttf
```

Enable RomFS in the Makefile:

```makefile
ROMFS := romfs
```

Initialize RomFS before loading the font and close it during shutdown. Load the TTF with the project's existing TTF-compatible renderer such as SDL_ttf, FreeType, stb_truetype, or an existing font-atlas pipeline.

`consoleInit` and `printf` do not load arbitrary TTF files. A console-only UI needs a custom renderer or pre-existing icon support before this font can be used.

## Text and fallback

The icon font does not replace the normal UI font. Use the normal font for Latin/CJK text and this font for private-use button glyphs.

If the renderer supports fallback fonts, register this font as the fallback for its private-use glyphs and keep prompts readable:

```cpp
const char *prompt = u8"请按下 \uE0E2 键来开始。";
```

If the renderer does not support fallback, split the run:

```text
normal font: 请按下
icon font:   U+E0E2
normal font: 键来开始。
```

For C++20 code, remember that `u8"..."` has type `const char8_t*`. Adapt it to the renderer's API deliberately rather than applying an unsafe cast. A UTF-8 byte sequence may be used when the API expects `const char*`.

## Mapping verification

Before implementing a prompt:

1. Inspect the target codepoint in `NintendoSwitchButtonIcons.ttf` with FontForge, fontTools, or the project's existing glyph viewer.
2. Confirm the displayed glyph matches the intended controller style.
3. Confirm the input branch uses the intended `HidNpadButton_*` mask.
4. Test handheld and supported controller configurations where appearance differs.

Do not assume mappings from another Nintendo font are identical.

## Source and redistribution

Source package: OnlineWebFonts download for `NintendoExt003`, originally attributed to Nintendo. The supplied download notice requests attribution to OnlineWebFonts and also warns that some hosted fonts may be trial versions or may prohibit embedding without a commercial license.

This skill asset is for the user's local development workflow. Before publicly distributing an NRO that embeds the font, verify redistribution and embedding rights. Do not describe the font as open source or commercially licensed without independent evidence.
