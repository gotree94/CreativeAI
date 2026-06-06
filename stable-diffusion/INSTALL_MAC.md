# Stable Diffusion WebUI reForge — macOS 설치 가이드

> **대상 환경**: macOS 12+ (Monterey 이상)  
> **권장 Python**: 3.10 ~ 3.11 (3.12 실험적, 3.13은 일부 문제 있음)  
> **Apple Silicon**: M1/M2/M3/M4 — MPS 가속 지원  
> **Intel Mac**: CPU 모드 (제한적 CUDA 불가)  
> **최소 RAM**: 16GB (권장 32GB)

---

## 목차

1. [사전 준비](#1-사전-준비)
2. [리포지토리 클론](#2-리포지토리-클론)
3. [최초 실행 및 자동 설치](#3-최초-실행-및-자동-설치)
4. [모델 다운로드 및 배치](#4-모델-다운로드-및-배치)
5. [이미지 생성하기](#5-이미지-생성하기)
6. [고급 설정 및 최적화](#6-고급-설정-및-최적화)
7. [Apple Silicon 최적화](#7-apple-silicon-최적화)
8. [문제 해결](#8-문제-해결)
9. [업데이트](#9-업데이트)

---

## 1. 사전 준비

### 1.1 Xcode Command Line Tools 설치

Xcode Command Line Tools는 Git, 컴파일러 등 필수 도구를 제공합니다.

```bash
xcode-select --install
```

팝업 창이 뜨면 "Install" 클릭, 설치 완료까지 기다립니다 (보통 5~10분).

설치 확인:
```bash
git --version
```

### 1.2 Homebrew 설치 (권장)

[Homebrew](https://brew.sh)는 macOS 패키지 관리자로 Python 등 설치에 편리합니다.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 1.3 Python 설치

**방법 A — Homebrew (권장)**:

```bash
brew install python@3.10
```

설치 후 PATH에 추가 (Apple Silicon):
```bash
echo 'export PATH="/opt/homebrew/opt/python@3.10/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

**방법 B — 공식 인스톨러**:

[Python 3.10.11](https://www.python.org/downloads/release/python-31011/) 다운로드 및 설치

설치 확인:
```bash
python3.10 --version
```

### 1.4 CMake 설치 (컴파일 필요 시)

```bash
brew install cmake
```

---

## 2. 리포지토리 클론

### 2.1 기본 설치 (권장)

```bash
cd ~/Desktop/stable-diffusion
git clone https://github.com/Panchovix/stable-diffusion-webui-reForge.git
cd stable-diffusion-webui-reForge
git checkout main
```

### 2.2 A1111 WebUI가 이미 있는 경우

```bash
cd /path/to/existing/stable-diffusion-webui
git remote add reForge https://github.com/Panchovix/stable-diffusion-webui-reForge.git
git fetch reForge
git checkout -b reForge-main reForge/main
```

> 원래 A1111로 돌아가려면: `git checkout master`

---

## 3. 최초 실행 및 자동 설치

### 3.1 webui.sh 실행

```bash
cd ~/Desktop/stable-diffusion/stable-diffusion-webui-reForge
bash webui.sh
```

처음 실행 시 **자동**으로 진행되는 작업:

1. **Python 가상환경(venv)** 생성 (`venv/` 폴더)
2. **pip 업그레이드**
3. **PyTorch** (macOS: MPS 지원 CPU 버전) 자동 설치
4. **requirements** 자동 설치
5. **기타 의존성** 자동 다운로드
6. **WebUI 실행** — http://127.0.0.1:7860 자동 오픈

> **소요 시간**: 인터넷 속도에 따라 **10~30분** 정도 소요됩니다.

### 3.2 macOS 자동 설정 (webui-macos-env.sh)

macOS에서 `webui.sh` 실행 시 자동으로 `webui-macos-env.sh`가 로드됩니다.  
이 파일의 기본 설정:

```bash
export install_dir="$HOME"
export COMMANDLINE_ARGS="--skip-torch-cuda-test --upcast-sampling --no-half-vae --use-cpu interrogate"
export PYTORCH_ENABLE_MPS_FALLBACK=1

# Intel Mac
if [[ "$(sysctl -n machdep.cpu.brand_string)" =~ ^.*"Intel".*$ ]]; then
    export TORCH_COMMAND="pip install torch==2.3.1 torchvision==0.18.1"
# Apple Silicon
else
    export TORCH_COMMAND="pip install torch==2.4.0 torchvision==0.19.0"
fi
```

### 3.3 webui-user.sh 사용자 설정

`webui-user.sh`에서 위 설정을 덮어쓸 수 있습니다.

```bash
#!/bin/bash

# Commandline arguments
export COMMANDLINE_ARGS="--skip-torch-cuda-test --upcast-sampling --no-half-vae --use-cpu interrogate --medvram"

# python3 executable
python_cmd="python3.10"

# Requirements file
export REQS_FILE="requirements_versions.txt"
```

---

## 4. 모델 다운로드 및 배치

### 4.1 모델 파일 위치

| 모델 종류 | 폴더 위치 |
|-----------|----------|
| Checkpoint (메인 모델) | `models/Stable-diffusion/` |
| LoRA | `models/Lora/` |
| VAE | `models/VAE/` |
| Embeddings | `embeddings/` |
| ControlNet | `models/ControlNet/` |

### 4.2 추천 모델 (macOS)

macOS는 VRAM 제한이 없지만 통합 메모리를 공유하므로,  
SD1.5 기반 모델 또는 SDXL 중 최적화된 모델을 권장합니다.

- **SD1.5**: [realisticVision](https://civitai.com/models/4201), [DreamShaper](https://civitai.com/models/4384)
- **SDXL**: [SDXL 1.0](https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0) (M1/M2 16GB 이상 권장)
- **터보/라이트 모델**: [SDXL Turbo](https://huggingface.co/stabilityai/sdxl-turbo), [LCM-LoRA](https://huggingface.co/latent-consistency/lcm-lora-sdxl)

### 4.3 Hugging Face에서 직접 다운로드

```bash
cd models/Stable-diffusion/
curl -L -o sd_xl_base_1.0.safetensors \
  https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/resolve/main/sd_xl_base_1.0.safetensors
```

---

## 5. 이미지 생성하기

### 5.1 WebUI 접속

브라우저에서 http://127.0.0.1:7860 접속

### 5.2 기본 txt2img

1. 좌측 상단 모델 드롭다운에서 모델 선택
2. **txt2img** 탭 선택
3. 프롬프트 입력 (예: `a beautiful mountain landscape, digital art, highly detailed`)
4. Negative 프롬프트 입력 (예: `blurry, low quality, distorted`)
5. 설정:
   - **Sampling method**: `DPM++ 2M Karras` 또는 `LCM` (터보 모델)
   - **Sampling steps**: `20~30` (일반) / `4~8` (터보/LCM)
   - **Width/Height**: `512x512` (SD1.5) / `1024x1024` (SDXL, 16GB+ RAM)
   - **CFG Scale**: `7` (일반) / `2~4` (터보/LCM)
   - **Batch count**: `1`, **Batch size**: `1`
6. **Generate** 버튼 클릭

### 5.3 Apple Silicon MPS 참고사항

- MPS 백엔드는 PyTorch 2.0+에서 안정화됨
- 첫 생성 시 셰이더 컴파일로 인해 **첫 이미지가 느릴 수 있음** (이후 빨라짐)
- `PYTORCH_ENABLE_MPS_FALLBACK=1`이 자동 설정되어 일부 연산이 CPU로 폴백

---

## 6. 고급 설정 및 최적화

### 6.1 COMMANDLINE_ARGS

`webui-user.sh`에서 설정:

```bash
export COMMANDLINE_ARGS="--skip-torch-cuda-test --upcast-sampling --no-half-vae --use-cpu interrogate"
```

#### macOS 주요 플래그

| 플래그 | 설명 | macOS 적용 |
|--------|------|-----------|
| `--skip-torch-cuda-test` | CUDA 테스트 건너뜀 | **필수** |
| `--upcast-sampling` | 샘플링 정밀도 향상 | 권장 |
| `--no-half-vae` | VAE FP32 강제 (화질 향상) | 권장 |
| `--use-cpu interrogate` | interrogate(CLIP)를 CPU에서 실행 | 권장 |
| `--medvram` | 중간 VRAM 모드 | 메모리 부족 시 |
| `--precision full` | 전체 정밀도 | M1/M2 호환성 ↑ |
| `--no-half` | half precision 비활성화 | MPS 문제 시 |

#### 메모리 부족 시 최소 설정

```bash
export COMMANDLINE_ARGS="--skip-torch-cuda-test --no-half-vae --medvram --unet-in-fp8-e4m3fn"
```

### 6.2 Apple Silicon MPS 참고

reForge의 `--cuda-stream`, `--cuda-malloc`, `--pin-shared-memory` 등은  
**CUDA 전용**이므로 macOS에서 작동하지 않습니다.

---

## 7. Apple Silicon 최적화

### 7.1 PyTorch MPS 백엔드

macOS 12.3+ (Monterey) 이상에서 MPS 지원:

```bash
# Python에서 MPS 사용 가능 확인
python3.10 -c "import torch; print(torch.backends.mps.is_available())"
```

`True` 반환 시 MPS 가속 사용 가능.

### 7.2 성능 팁

| 설정 | 권장값 | 이유 |
|------|--------|------|
| 이미지 해상도 | 512x512 이하 | 통합 메모리 사용량 감소 |
| Batch size | 1 | 메모리 경합 방지 |
| Sampling steps | 20~25 | 품질과 속도 균형 |
| 터보 모델 사용 | LCM / SDXL Turbo | 4~8 스텝으로 고속 생성 |

### 7.3 Intel Mac 참고사항

- Intel Mac은 **Metal Performance Shaders**를 통해 가속
- NVIDIA GPU가 없으므로 CUDA 관련 옵션 모두 비활성화
- Intel Mac + AMD GPU (일부 모델)는 추가 설정 필요 없음

---

## 8. 문제 해결

### 8.1 `python3` 명령어 인식 불가

**해결**:

```bash
# Python 3.10 경로 직접 지정
python_cmd="/opt/homebrew/bin/python3.10"
```

`webui-user.sh`에 위 내용 추가.

### 8.2 MPS 에러 / Fallback 경고

**증상**: `MPS does not support and will fallback to CPU` 경고  
**해결**: `PYTORCH_ENABLE_MPS_FALLBACK=1` 설정 확인 (자동 설정됨).  
문제 지속 시 CPU 모드 사용:

```bash
export COMMANDLINE_ARGS="--skip-torch-cuda-test --use-cpu all --no-half"
```

### 8.3 메모리 부족 (Swap 사용)

**증상**: 이미지 생성 중 시스템이 극도로 느려짐  
**해결**:

1. 이미지 해상도 낮추기 (512x512)
2. `--medvram` 플래그 추가
3. 다른 앱 종료
4. macOS 메모리 압축 비활성화 고려 (`sudo nvram boot-args="vm_compressor=2"` — **주의 필요**)

### 8.4 가상환경 생성 실패

**증상**: `python3-venv is not installed`  
**해결**:

```bash
brew install python@3.10
# 또는
pip3 install virtualenv
```

### 8.5 포트 충돌

```bash
export COMMANDLINE_ARGS="--port 7861"
```

---

## 9. 업데이트

### 9.1 기본 업데이트

```bash
cd ~/Desktop/stable-diffusion/stable-diffusion-webui-reForge
git pull
```

### 9.2 안정적인 브랜치로 전환

```bash
git checkout main-old
```

### 9.3 안정적인 대체 프로젝트 (macOS)

- **[Forge Classic](https://github.com/Haoming02/sd-webui-forge-classic)** — 안정적인 Forge 기반
- **[Forge Neo](https://github.com/Haoming02/sd-webui-forge-classic/tree/neo)** — Forge2 + 최신 기능
- **[Draw Things](https://drawthings.ai)** — macOS 네이티브앱 (GUI, WebUI 불필요)

---

## 참고 자료

- [reForge GitHub](https://github.com/Panchovix/stable-diffusion-webui-reForge)
- [Forge GitHub (원본)](https://github.com/lllyasviel/stable-diffusion-webui-forge)
- [PyTorch MPS 문서](https://pytorch.org/docs/stable/notes/mps.html)
- [CivitAI (모델)](https://civitai.com)
- [Hugging Face (모델)](https://huggingface.co)
