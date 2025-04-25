# VRSplat

## Fast and Robust Gaussian Splatting for Virtual Reality

[Xuechang Tu](https://scholar.google.com/citations?user=LGmQ3bUAAAAJ)
[Lukas Radl](https://r4dl.github.io/), 
[Michael Steiner](https://steimich96.github.io),
[Markus Steinberger](https://www.markussteinberger.net/),
[Bernhard Kerbl](https://snosixtyboo.github.io/),
[Fernando de la Torre]()

| [Webpage](https://cekavis.github.io/VRSplat/) 
| [Full Paper](https://r4dl.github.io/data/vrsplat_authorversion.pdf) 
| [Pre-trained Models (1.04 GB)](https://drive.google.com/file/d/1RSJjlAwUs_3LTxWX7l221HsVLsmOFfE0/view?usp=sharing) 
<br>

This repository contains the official authors implementation associated with the paper "VRSplat: Fast and Robust Gaussian Splatting for Virtual Reality".

<img height="100" src="assets/pku-logo.jpg">　<a href="https://www.tugraz.at/en/home"><img height="100" src="assets/tugraz-logo.jpg"></a>　<img height="100" src="assets/huawei-logo.jpg">　<img height="100" src="assets/cmu-logo.jpg">

Abstract: *3D Gaussian Splatting (3DGS) has rapidly become a leading technique for novel-view synthesis, providing exceptional performance through efficient software-based GPU rasterization. Its versatility enables real-time applications, including on mobile and lower-powered devices. However, 3DGS faces key challenges in virtual reality (VR): (1) temporal artifacts, such as popping during head movements, (2) projection-based distortions that result in disturbing and view-inconsistent floaters, and (3) reduced framerates when rendering large numbers of Gaussians, falling below the critical threshold for VR. Compared to desktop environments, these issues are drastically amplified by large field-of-view, constant head movements, and high resolution of head-mounted displays (HMDs). In this work, we introduce VRSplat: we combine and extend several recent advancements in 3DGS to address challenges of VR holistically. We show how the ideas of Mini-Splatting, StopThePop, and Optimal Projection can complement each other, by modifying the individual techniques and core 3DGS rasterizer. Additionally, we propose an efficient foveated rasterizer that handles focus and peripheral areas in a single GPU launch, avoiding redundant computations and improving GPU utilization. Our method also incorporates a fine-tuning step that optimizes Gaussian parameters based on StopThePop depth evaluations and Optimal Projection. We validate our method through a controlled user study with 25 participants, showing a strong preference for VRSplat over other configurations of Mini-Splatting. VRSplat is the first, systematically evaluated 3DGS approach capable of supporting modern VR applications, achieving 72+ FPS while eliminating popping and stereo-disrupting floaters.*

<section class="section" id="BibTeX">
  <div class="container is-max-desktop content">
    <h2 class="title">BibTeX</h2>
    <pre><code>@article{Tu2025VRSplat,
  author    = {Tu, Xuechang and Radl, Lukas and Steiner, Michael and Steinberger, Markus and Kerbl, Bernhard and de la Torre, Fornando},
  title     = {{VRsplat: Fast and Robust Gaussian Splatting for Virtual Reality}},
  journal   = {Proc. ACM Comput. Graph. Interact. Tech.},
  volume    = {8},
  number    = {1},
  articleno = {1},
  year      = {2025},
}</code></pre>
  </div>
</section>

## Overview
Our repository is built on [3D Gaussian Splatting](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/):
For a full breakdown on how to get the code running, please consider [3DGS's Readme](https://github.com/graphdeco-inria/gaussian-splatting/blob/main/README.md).

The project is split into submodules, each maintained in a separate github repository:

* [VRSplat-Rasterization](https://github.com/Cekavis/VRSplat-Rasterization): A clone of the original [diff-gaussian-rasterization](https://github.com/graphdeco-inria/diff-gaussian-rasterization) that contains our CUDA rasterizer implementation
* [SIBR_VRSplat](https://github.com/Cekavis/SIBR_VRSplat): A clone of the [SIBR Core](https://gitlab.inria.fr/sibr/sibr_core) project, containing an adapted VR viewer with our additional settings and functionalities

## Licensing

The majority of the projects is licensed under the ["Gaussian-Splatting License"](LICENSE.md), with the exception of:

* [StopThePop header files](submodules/diff-gaussian-rasterization/cuda_rasterizer/stopthepop): MIT License
* [FLIP](utils/flip.py): BSD-3 license

There are also several changes in the original source code.
If you use any of our additional functionalities, please cite our paper and link to this repository.

## Cloning the Repository

The repository contains submodules, thus please check it out with 
```shell
git clone https://github.com/Cekavis/VRSplat --recursive
```

## Setup

### Local Setup

Our default, provided install method is based on Conda package and environment management:
```shell
SET DISTUTILS_USE_SDK=1 # Windows only
conda env create --file environment.yml
conda activate vrsplat
```

> **Note:** This process assumes that you have CUDA SDK **11** installed.
Optionally, you can use CUDA **12** and Pytorch **2.1**, by using `environment_cuda12.yml` instead of `environment.yml`.

Subsequently, install the CUDA rasterizer:
```shell
pip install submodules/diff-gaussian-rasterization
```

> **Note:** This can take several minutes. If you experience unreasonably long build times, consider using [StopThePop's fast build mode](https://github.com/r4dl/StopThePop#stopthepop_fastbuild).

### Running

To train a model from scratch, run:

```shell
python train.py --splatting_config configs/hierarchical.json -s <path to COLMAP or NeRF Synthetic dataset>
```

To fine-tune the output of another 3DGS training method (e.g. [MiniSplatting](https://github.com/fatPeter/mini-splatting)), first ensure that a compatible checkpoint is available (e.g. trained with `--checkpoint_iterations 30000`). Then, run:
```shell
python train.py --splatting_config configs/hierarchical.json -s <path to COLMAP or NeRF Synthetic dataset> --start_checkpoint <checkpoint path> --iterations 35000
```

For a full explanation of the training configuration, please refer to [StopThePop](https://github.com/r4dl/StopThePop).

## Interactive VR Viewer

See [SIBR_VRSplat](https://github.com/Cekavis/SIBR_VRSplat).

## FAQ

Please refer to 3DGS's FAQ, contained in [their README](https://github.com/graphdeco-inria/gaussian-splatting/blob/main/README.md). In addition, several issues are also covered on [3DGS's issues page](https://github.com/graphdeco-inria/gaussian-splatting/issues).
