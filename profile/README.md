<p align="center">
  <img src="https://raw.githubusercontent.com/eskiu-lang/eskiu/main/assets/logo.png" alt="Eskiu" width="120">
</p>

<h3 align="center">The Eskiu Programming Language</h3>

<p align="center">
  A systems language with C-style syntax, Go-style interfaces and explicit memory.
  <br>
  <a href="https://eskiu-lang.org">Website</a> ·
  <a href="https://eskiu-lang.org/docs/getting-started.html">Getting started</a> ·
  <a href="https://eskiu-lang.org/the-book-of-eskiu.html">The Book of Eskiu</a> ·
  <a href="https://github.com/eskiu-lang/eskiu/releases">Releases</a>
</p>

---

Eskiu compiles to native code through LLVM and calls C libraries directly, with structs
passed in each target's C ABI. It adds generics, structural interfaces, sum types with
exhaustive `match`, closures, `defer`, slices, checked nullable pointers and `async`/`await`,
and it has no garbage collector. A source file can also run as a script with
`eskiuc run file.esk`.

```c
import <io>;
import <fs>;
import <string>;

Result<int, string> count_lines(string path) {
    String text = fs_read_text_result(path)?;
    defer text.free();
    int n = 0;
    for (i in 0..text.len) {
        if (text.data[i] == '\n') n++;
    }
    return Ok<int, string>(n);
}

Result<int, string> r = count_lines(argc > 1 ? argv[1] : "notes.txt");
if (r.ok) printf("%d lines\n", r.value);
else printf("error: %s\n", r.error);
```

## Install

```bash
curl -fsSL https://eskiu-lang.org/install.sh | sh     # macOS (Apple Silicon), Linux x86-64 and arm64
brew install eskiu-lang/eskiu/eskiu                   # Homebrew
```

Windows x86-64 builds are on the [releases page](https://github.com/eskiu-lang/eskiu/releases).
The compiler cross-compiles to 64-bit and 32-bit ARM and x86, WebAssembly (emscripten and
WASI), iOS and Android, and bare-metal targets, and writes static and shared libraries for
an app's own build.

## Repositories

| Repository | What it holds |
|---|---|
| [eskiu](https://github.com/eskiu-lang/eskiu) | The compiler, the standard library, the self-hosted compiler written in Eskiu, editor extensions, docs and tests |
| [homebrew-eskiu](https://github.com/eskiu-lang/homebrew-eskiu) | The Homebrew tap |

## Packages

Each package is added with `eskiuc add NAME --git https://github.com/eskiu-lang/REPO --rev TAG`, at the
tag its README gives.

| Package | What it does |
|---|---|
| [gpu](https://github.com/eskiu-lang/gpu) | GPU compute and rendering over WebGPU (Metal, Vulkan, DirectX 12), glTF and OBJ models with PBR, windows and input over SDL3, and machine learning: Llama inference from GGUF and safetensors (Llama 2 7B at 45 tokens per second on an M4 Pro) and training |
| [onnx](https://github.com/eskiu-lang/onnx) | ONNX Runtime inference on the CPU, Core ML, NNAPI and WebGPU |
| [tflite](https://github.com/eskiu-lang/tflite) | TensorFlow Lite inference with the XNNPACK, GPU and Core ML delegates |
| [compress](https://github.com/eskiu-lang/compress) | Deflate, zlib, gzip and zip, written in Eskiu |
| [audio](https://github.com/eskiu-lang/audio) | WAV files, a mixer with gapless playlists, and sound output: SDL3 on the desktop, RemoteIO and AAudio on iOS and Android, where MP3, AAC and FLAC decode through the system |
| [font](https://github.com/eskiu-lang/font) | TrueType fonts: glyph rendering, text layout and atlases |
| [jni](https://github.com/eskiu-lang/jni) | Native methods for Android and the JVM, written in Eskiu with no C glue |
| [nds](https://github.com/eskiu-lang/nds) | Nintendo DS file formats: ROMs, NARC archives, SDAT sound, 2D graphics and 3D textures |
| vision (private for now) | Corners, optical flow, multi-view geometry and planes in pure Eskiu, from ReactVision's tinyvio; it opens together with tinyvio |

## Apps

[ui](https://github.com/eskiu-lang/ui) makes native apps for iOS, Android, macOS, Linux, Windows
and the web from one Eskiu codebase: components drawn by each platform's own views, signals,
navigation, a kit of themed components, and the `ui` tool that builds, reloads and ships them
to the stores. Packages bring device services and drawing to a ui app:

| Package | What it does |
|---|---|
| [ui-location](https://github.com/eskiu-lang/ui-location) | Where the device is |
| [ui-notifications](https://github.com/eskiu-lang/ui-notifications) | Local notifications, shown also with the app closed |
| [ui-camera](https://github.com/eskiu-lang/ui-camera) | Photos from the library and the camera, and a live camera view |
| [ui-barcode](https://github.com/eskiu-lang/ui-barcode) | QR codes and barcodes read live from the camera |
| [ui-pedometer](https://github.com/eskiu-lang/ui-pedometer) | Steps walked, live and between two times |
| [ui-securestore](https://github.com/eskiu-lang/ui-securestore) | Small secrets in the Keychain and the Android Keystore |
| [ui-svg](https://github.com/eskiu-lang/ui-svg) | SVG drawings, tinted by the style's color |
| [ui-canvas](https://github.com/eskiu-lang/ui-canvas) | A 2D canvas drawn every frame, for sprites and pixel art |

[ui-starter](https://github.com/eskiu-lang/ui-starter) is an app to start from, and
[ui-package-template](https://github.com/eskiu-lang/ui-package-template) a package to start from.

## Tools

The `eskiuc` binary includes a formatter (`eskiuc fmt`), a REPL (`eskiuc repl`) and a package
manager with git and path dependencies (`eskiuc new`, `add`, `build`, `test`). Editor support
comes as an extension for VS Code and the editors built on it
([Open VSX](https://open-vsx.org/extension/doranteseduardo/eskiu-language)) and through LSP4IJ
for JetBrains IDEs.

## In production

[ReactVision](https://reactvision.xyz) runs Eskiu in ViroReact: rendering modules moved from
C++ use about 85% less memory, the web AR tracker (tinyvio) runs on an Eskiu engine that
computes the same bits as the C++ it replaced, and on-device object detection runs on one Eskiu
core for iOS and Android ([case study](https://eskiu-lang.org/case-study-reactvision.html)). For fun, Eskiu also runs an
AR demo on the Nintendo 3DS ([case study](https://eskiu-lang.org/case-study-3ds.html)).

## Contributing

Issues and pull requests are welcome in the [main repository](https://github.com/eskiu-lang/eskiu).
Eskiu is released under the MIT license.
