# Stable Diffusion WebUI reForge — 설치 & 사용 가이드

[![GitHub stars](https://img.shields.io/github/stars/Panchovix/stable-diffusion-webui-reForge?style=flat-square)](https://github.com/Panchovix/stable-diffusion-webui-reForge)
[![License](https://img.shields.io/github/license/Panchovix/stable-diffusion-webui-reForge?style=flat-square)](https://github.com/Panchovix/stable-diffusion-webui-reForge/blob/main/LICENSE.txt)

**Stable Diffusion WebUI reForge**는 [AUTOMATIC1111 WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui)를 기반으로 [lllyasviel/stable-diffusion-webui-forge](https://github.com/lllyasviel/stable-diffusion-webui-forge)에서 포크된 프로젝트로, **향상된 리소스 관리**와 **최신 기능**을 제공합니다.

> ⚠️ **참고**: reForge는 실험적인 포크입니다. 안정성이 중요하다면 [Forge Classic](https://github.com/Haoming02/sd-webui-forge-classic) 또는 [Forge Neo](https://github.com/Haoming02/sd-webui-forge-classic/tree/neo)를 고려하세요.

---

## 플랫폼별 설치 가이드

| 플랫폼 | 가이드 | 주요 특징 |
|--------|--------|----------|
| ![Windows](https://img.shields.io/badge/Windows-0078D6?style=flat-square) | [INSTALL_WINDOWS.md](./INSTALL_WINDOWS.md) | NVIDIA CUDA, AMD DirectML |
| ![macOS](https://img.shields.io/badge/macOS-000000?style=flat-square) | [INSTALL_MAC.md](./INSTALL_MAC.md) | Apple Silicon MPS 가속 |
| ![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat-square) | [INSTALL_LINUX.md](./INSTALL_LINUX.md) | NVIDIA CUDA, AMD ROCm |

---

## 주요 기능

### 🚀 리소스 관리 혁신
- **더 적은 VRAM**: SDXL 4GB, SD1.5 2GB VRAM에서 실행 가능
- **자동 최적화**: `--medvram`, `--lowvram` 같은 복잡한 플래그 불필요
- **스마트 오프로딩**: 필요 시 모델을 자동으로 CPU/GPU 간 이동

### 🎨 최신 모델 지원
- **SD1.5**, **SDXL** 완벽 지원
- **FLUX** (Forge2 브랜치)
- **SD3**, **PixArt-Sigma**, **Stable Cascade**, **Hunyuan-DiT** 지원 (확장 설치)

### ⚡ 성능 최적화
- CUDA 스트림을 통한 15~25% 속도 향상 (`--cuda-stream`)
- FP8 양자화 지원 (`--unet-in-fp8-e4m3fn`)
- SageAttention 지원 (`--use-sage-attention`)

### 🔧 다양한 확장 기능
- LoRA Block Control (Lora ctl)
- StyleAlign, Differential Diffusion, Automatic CFG 등
- ControlNet 완벽 지원

---

## 시스템 요구사항

| 구성 요소 | 최소 사양 | 권장 사양 |
|-----------|----------|----------|
| **Python** | 3.10 | 3.10~3.12 |
| **RAM** | 8GB | 16GB+ |
| **VRAM (SD1.5)** | 2GB | 4GB+ |
| **VRAM (SDXL)** | 4GB | 8GB+ |
| **디스크** | 10GB | 50GB+ (모델 포함) |
| **GPU** | CUDA/MPS/ROCm 호환 | NVIDIA RTX 3060+ |

---

## 빠른 시작

### Windows
```cmd
git clone https://github.com/Panchovix/stable-diffusion-webui-reForge.git
cd stable-diffusion-webui-reForge
.\webui-user.bat
```

### macOS / Linux
```bash
git clone https://github.com/Panchovix/stable-diffusion-webui-reForge.git
cd stable-diffusion-webui-reForge
chmod +x webui.sh
./webui.sh
```

> 첫 실행 시 필요한 의존성을 자동으로 다운로드 및 설치합니다.  
> 설치 완료 후 http://127.0.0.1:7860 에서 WebUI에 접속할 수 있습니다.

---

## 주요 브랜치

| 브랜치 | 설명 | 안정성 |
|--------|------|--------|
| `main` | 최신 변경사항 포함 | ⚠️ 중간 |
| `main-old` | 기존 Forge 백엔드 (가장 안정적) | ✅ 높음 |
| `dev` | ComfyUI 백엔드 실험 | 🔬 낮음 |
| `dev2` | Gradio 4 실험 | 🔬 낮음 |

> 안정성을 원하면 `git checkout main-old`로 전환하세요.

---

## 유용한 플래그

### VRAM 절약
```
--always-offload-from-vram --unet-in-fp8-e4m3fn
```

### 성능 향상 (RTX 30/40)
```
--cuda-malloc --cuda-stream --use-sage-attention
```

### macOS 필수
```
--skip-torch-cuda-test --upcast-sampling --no-half-vae --use-cpu interrogate
```

---

## 관련 프로젝트

| 프로젝트 | 설명 |
|----------|------|
| [Forge Classic](https://github.com/Haoming02/sd-webui-forge-classic) | reForge/Forge 기능 안정화, 구형 백엔드 |
| [Forge Neo](https://github.com/Haoming02/sd-webui-forge-classic/tree/neo) | Forge2 기반 최신 기능 + Flux/GGUF |
| [ersatzForge](https://github.com/DenOfEquity/ersatzForge) | Forge2 기반 실험적 포크 |
| [AUTOMATIC1111 WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) | 원본 WebUI |
| [ComfyUI](https://github.com/comfyanonymous/ComfyUI) | 노드 기반 SD UI |

---

## 라이선스

이 프로젝트는 [AGPL-3.0](./INSTALL_LINUX.md) 라이선스를 따릅니다.  
원본 AUTOMATIC1111 WebUI와 Forge 프로젝트의 라이선스를 계승합니다.

---

> **문서 위치**: `C:\Users\Administrator\Desktop\stable-diffusion\`  
> **생성일**: 2026-06-06
