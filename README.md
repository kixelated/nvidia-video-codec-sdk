# NVIDIA Video Codec SDK

[![crates.io](https://img.shields.io/crates/v/nvidia-video-codec-sdk?style=for-the-badge)](https://crates.io/crates/nvidia-video-codec-sdk)
[![docs.rs](https://img.shields.io/docsrs/nvidia-video-codec-sdk?label=docs.rs%20latest&style=for-the-badge)](https://docs.rs/nvidia-video-codec-sdk)

Rust bindings for [NVIDIA Video Codec SDK](https://developer.nvidia.com/video-codec-sdk).

The documentation is also hosted on GitHub Pages
[here](https://viliamvadocz.github.io/nvidia-video-codec-sdk/nvidia_video_codec_sdk/).

Versions:
- NVIDIA Video Codec SDK 12.1.14
- CUDA 12.2 (older CUDA versions should also work)

## Installation

The build script will try to automatically locate your NVIDIA Video Codec SDK installation.
You can help it by setting the environment variable `NVIDIA_VIDEO_CODEC_SDK_PATH` to the directory containing the library files. 
- `nvEncodeAPI.lib` and `nvcuvid.lib` on Windows,
- `libnvidia-encode.so` and `libnvcuvid.so` on Linux.

### Dynamic loading

Enabling the `dynamic-loading` feature `dlopen`s `libnvidia-encode` at runtime
instead of linking it. The build then needs neither the driver library nor a CUDA
toolkit present, and the resulting binary can be built on a GPU-less machine and
shipped to one that may or may not have an NVIDIA driver (so a caller can fall
back to another encoder when the library is absent). This mirrors cudarc's own
`dynamic-loading` feature.

Pick a CUDA version explicitly when using it, since build-system detection is
off in a toolkit-less build:

```toml
[dependencies]
nvidia-video-codec-sdk = { version = "0.4", default-features = false, features = ["dynamic-loading"] }
cudarc = { version = "0.19", default-features = false, features = ["driver", "cuda-12020", "dynamic-loading"] }
```

Supported on Linux and Windows. Only the two NVENC bootstrap entry points are
loaded dynamically; NVDEC (`cuvid`) symbols are neither linked nor `dlopen`'d
under this feature.
