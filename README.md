# Colmap-to-Blender Camera Tracking Workflow

A specialized utility for automating the COLMAP-to-Blender camera tracking process. This project is a refactored and performance-optimized version of the original workflow shared by **[Polyfjord](https://gist.github.com/polyfjord)**, updated for **COLMAP 4.0** and **NVIDIA GPU acceleration**.

## 🚀 Key Improvements in this Fork

*   **NVENC Acceleration**: Utilizes **NVENC** for the image extraction phase, which provides a significant speed boost over CPU-based extraction, especially when processing longer video files.
*   **Faster & More Accurate Solves**: The script has been refactored to use modern COLMAP syntax, resulting in much faster and more reliable tracking.
*   **Proxy-Based Tracking**: The primary performance improvement comes from performing the tracking on **proxy images** rather than full-resolution frames, dramatically reducing processing time without sacrificing solve quality.
*   **Graph Calibration**: Enabled the **graph calibration** step during tracks to improve the overall precision and stability of the 3D camera solve.
*   **COLMAP 4.0 Support**: Logic has been updated to maintain compatibility with the latest binary requirements and folder structures.
*   **Terminal Compatibility**: Swapped out custom symbols for standard ASCII to ensure the interface renders correctly in Windows CMD and PowerShell.

## ⚠️ Known Limitations & Roadmap

*   **Global Mapping (CPU Reliance)**: Currently triggers a "CPU warning" during the process. This is because the current **COLMAP** release lacks the necessary CUDA flags for the **Ceres Solver** to utilize the GPU. Full acceleration for this stage is dependent on the release of **Ceres Solver 2.3** and its subsequent integration into official COLMAP builds.
    *   **Source Compilation Note**: While re-building COLMAP from source with a patched Ceres solver may theoretically resolve the CPU fallback, it is currently **untested** in this specific workflow.
*   **Feature Extraction Method**: This version currently utilizes standard **SIFT** for feature extraction. Integration of the newer **SIFT-LightGlue** method is planned for a future release to improve solve stability in challenging or low-texture shots.


### 🛠️ Prerequisites & Environment Setup

This workflow is optimized for **NVIDIA hardware** to take advantage of NVENC and CUDA. It has **not** been tested on AMD systems and likely will not function as intended without an NVIDIA GPU.

#### 1. Core Dependencies
*   **[COLMAP](https://github.com/colmap/colmap/releases)**: Download the latest **CUDA-enabled release** from the official GitHub.
*   **[FFmpeg](https://www.gyan.dev/ffmpeg/builds/)**: Download the "Essentials" or "Full" build.
*   **Setup**: Once downloaded, place these binaries into their respective folders within the project directory.

#### 2. GPU & Driver Configuration
*   **NVIDIA Drivers**: Ensure you have the latest drivers installed for your specific GPU.
*   **[CUDA Toolkit](https://developer.nvidia.com/cuda/toolkit)**: Ensure the CUDA Toolkit is installed on your system and correctly added to your **System PATH**.
*   **[cuDNN Libraries](https://developer.nvidia.com/cudnn)**: Download the latest cuDNN libraries and copy the files into the COLMAP `/bin` directory.

#### 3. Hardware Requirements & Compatibility
*   **Architecture**: Designed specifically for NVIDIA GPUs with **NVENC** (for lightning-fast image extraction) and **CUDA** support (for high-accuracy solves).
*   **Recommended GPU**: An NVIDIA RTX 20-series or newer is recommended for optimal performance.
*   **VRAM**: A minimum of 6GB–8GB of VRAM is suggested, especially when processing high-resolution footage or long sequences to prevent memory-related crashes.

### 📂 Project Structure
After downloading the **[latest release](https://github.com/Ex1tZ3r0/structure-from-motion-script/releases/latest)**, ensure your project directory is organized as follows:
```
├── 01 COLMAP/   # Place extracted COLMAP binaries here
├── 02 VIDEOS/   # Drop your raw video or image files here
├── 03 FFMPEG/   # Place extracted FFMPEG binaries here
├── 04 SCENES/   # Output folder for solves and image sequences
└── 05 SCRIPT/   # Contains the tracker script (Double-click to run)
```

> [!IMPORTANT]
> **GPU Acceleration**: You must copy the `cuDNN.dll` files directly into the `/01 COLMAP/bin` folder. If these are missing, COLMAP will default to CPU tracking, significantly increasing solve times.

## 📝 Usage

1. **Video Placement**: Place your video file or files in the `/02 VIDEOS` folder.
2. **Execution**: Run the script to begin the automated tracking process.
3. **Blender Import**: Import the resulting data into Blender using the [Photogrammetry-Importer](https://github.com/SBCV/Blender-Addon-Photogrammetry-Importer) addon.
    * **Note**: Image sequences will not auto-import and will trigger an error within Blender. 
    * **Workaround**: This warning can be safely ignored. You will still have the ability to manually import the proxy or full-resolution images yourself once the camera data is loaded.

## ❤️ Credits
Original methodology and script concept by **[Polyfjord](https://gist.github.com/polyfjord)**. This fork was developed to automate manual setup steps and ensure compatibility with current COLMAP binaries.
