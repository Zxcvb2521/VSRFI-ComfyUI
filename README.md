# VSRFI - Video Super Resolution + Frame Interpolation on arbitrary length video for ComfyUI


## Features

- **Video Super Resolution**: Uses Flash VSR to upscale video.
- **Frame Interpolation**: Generate smooth intermediate frames using your choice of GIMM-VFI, RIFE, or FILM.
- **Smart Tile Processing**: Automatically calculates the optimal way to tile a video based on your available VRAM. Uses non-square tiles for non-square videos. Can be significantly faster than traditional tiling.
- **Stream Processing**: Upscale any length video without running into memory issues.

![Demo](Examples/newsom2.gif)
![Demo](Examples/parachute2.gif)

## Installation

### 1. Clone this repository into your ComfyUI custom nodes directory:

```bash
cd /path/to/ComfyUI/custom_nodes
git clone https://github.com/neilthefrobot/VSRFI.git
```

### 2. Install dependencies:

```bash
cd VSRFI
pip install -r requirements.txt
```

**Note**: CuPy (required for GIMM-VFI) must be installed separately with the version matching your CUDA toolkit:
```bash
# Check your CUDA version first:
nvcc --version

# Then install the matching CuPy package:
pip install cupy-cuda12x   # For CUDA 12.x
# OR
pip install cupy-cuda11x   # For CUDA 11.x
```
Install ComfyUI-Frame-Interpolation if using RIFE or FILM - https://github.com/Fannovel16/ComfyUI-Frame-Interpolation

### 3. Restart ComfyUI




## Model Downloads

On first use, the node will automatically download:

1. **FlashVSR-v1.1** from [JunhaoZhuang/FlashVSR-v1.1](https://huggingface.co/JunhaoZhuang/FlashVSR-v1.1)
   - Downloaded to: `ComfyUI/models/FlashVSR-v1.1/`

2. **GIMM-VFI** from [Kijai/GIMM-VFI_safetensors](https://huggingface.co/Kijai/GIMM-VFI_safetensors)
   - Downloaded to: `ComfyUI/models/interpolation/gimm-vfi/`

## Usage
There are two different versions of the node:

*Frames* - This node works like most comfy nodes and takes in video frames as input and returns upscaled video frames as output. All frames need to be in memory at the same time.

*Stream* - This version streams chunks of frames from a video file, upscales them, saves them into the output video file, and repeats. The maximum number of frames ever in memory at once is determined by frames_per_chunk parameter. This allows processing of long videos and even movies.

### Parameters

| Parameter | Description |
|-----------|-------------|
| **video_path** | Path to input video file |
| **output_path** | Path for output video (auto-generated if empty) |
| **scale** | Super-resolution scale factor (1-8x, using 1x is valid and can be useful to clean up videos that are already large) 0 to skip VSR |
| **interpolation_factor** | Multiplier for frame interpolation. 2 = double FPS. Values less than 2 will skip VFI |
| **VFI_method** | Method for interpolating frames - GIMM, RIFE, or FILM |
| **frames_per_chunk** | Number of frames processed at once (reduce for lower VRAM) |
| **max_tile_kilopixels** | Determines tile size when upscaling. Set this as high as your VRAM allows, or leave at 0 to auto calculate |
| **max_GIMM_kilopixels** | Input size for GIMM flow estimation. Leave at 0 to auto calculate |

## Acknowledgments
- ComfyUI-FlashVSR_Ultra_Fast - https://github.com/lihaoyun6/ComfyUI-FlashVSR_Ultra_Fast
- ComfyUI-GIMM-VFI - https://github.com/kijai/ComfyUI-GIMM-VFI
- ComfyUI-Frame-Interpolation - https://github.com/Fannovel16/ComfyUI-Frame-Interpolation
