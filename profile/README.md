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

Each package is added with `eskiuc add NAME --git https://github.com/eskiu-lang/NAME --rev v0.1.0`.

| Package | What it does |
|---|---|
| [gpu](https://github.com/eskiu-lang/gpu) | GPU compute and rendering over WebGPU (Metal, Vulkan, DirectX 12), glTF and OBJ models with PBR, windows and input over SDL3, and machine learning: Llama inference from GGUF and safetensors (Llama 2 7B at 45 tokens per second on an M4 Pro) and training |
| [onnx](https://github.com/eskiu-lang/onnx) | ONNX Runtime inference on the CPU, Core ML, NNAPI and WebGPU |
| [tflite](https://github.com/eskiu-lang/tflite) | TensorFlow Lite inference with the XNNPACK, GPU and Core ML delegates |
| [compress](https://github.com/eskiu-lang/compress) | Deflate, zlib, gzip and zip, written in Eskiu |
| [audio](https://github.com/eskiu-lang/audio) | WAV files, a mixer and sound output over SDL3 |
| [font](https://github.com/eskiu-lang/font) | TrueType fonts: glyph rendering, text layout and atlases |
| [jni](https://github.com/eskiu-lang/jni) | Native methods for Android and the JVM, written in Eskiu with no C glue |
| [nds](https://github.com/eskiu-lang/nds) | Nintendo DS file formats: ROMs, NARC archives, SDAT sound, 2D graphics and 3D textures |
| vision (private for now) | Corners, optical flow, multi-view geometry and planes in pure Eskiu, from ReactVision's tinyvio; it opens together with tinyvio |

## Tools

The `eskiuc` binary includes a formatter (`eskiuc fmt`), a REPL (`eskiuc repl`) and a package
manager with git and path dependencies (`eskiuc new`, `add`, `build`, `test`). Editor support
comes as an extension for VS Code and the editors built on it
([Open VSX](https://open-vsx.org/extension/doranteseduardo/eskiu-language)) and through LSP4IJ
for JetBrains IDEs.

## In production

[ReactVision](https://reactvision.xyz) runs Eskiu in ViroReact: rendering modules moved from
C++ use about 85% less memory, and the web AR tracker (tinyvio) and on-device object detection
run on Eskiu engines that compute the same bits as the C++ they replaced
([case study](https://eskiu-lang.org/case-study-reactvision.html)). For fun, Eskiu also runs an
AR demo on the Nintendo 3DS ([case study](https://eskiu-lang.org/case-study-3ds.html)).

## Contributing

Issues and pull requests are welcome in the [main repository](https://github.com/eskiu-lang/eskiu).
Eskiu is released under the MIT license.
