# Stable Diffusion WebUI reForge — Linux 설치 가이드

> **대상 환경**: Ubuntu 22.04+, Debian 11+, Fedora 34+, openSUSE Leap 15.4+  
> **권장 Python**: 3.10 ~ 3.12 (3.13은 일부 호환성 문제 있음)  
> **GPU**: NVIDIA CUDA (권장) / AMD ROCm / CPU 모드  
> **최소 VRAM**: SD1.5 — 2GB / SDXL — 4GB

---

## 목차

1. [사전 준비](#1-사전-준비)
2. [리포지토리 클론](#2-리포지토리-클론)
3. [최초 실행 및 자동 설치](#3-최초-실행-및-자동-설치)
4. [모델 다운로드 및 배치](#4-모델-다운로드-및-배치)
5. [이미지 생성하기](#5-이미지-생성하기)
6. [고급 설정 및 최적화](#6-고급-설정-및-최적화)
7. [AMD GPU (ROCm) 설정](#7-amd-gpu-rocm-설정)
8. [문제 해결](#8-문제-해결)
9. [업데이트](#9-업데이트)

---

## 1. 사전 준비

### 1.1 시스템 패키지 설치

**Ubuntu / Debian:**

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y \
  git \
  python3.10 python3.10-venv python3.10-dev \
  wget \
  curl \
  build-essential \
  cmake \
  libgl1-mesa-glx \
  libglib2.0-0 \
  libgoogle-perftools-dev \
  bc
```

**Fedora:**

```bash
sudo dnf install -y \
  git \
  python3.10 python3.10-devel \
  wget \
  curl \
  gcc-c++ \
  cmake \
  mesa-libGL \
  glib2 \
  gperftools-libs \
  bc
```

**openSUSE:**

```bash
sudo zypper install -y \
  git \
  python3.10 python3.10-devel \
  wget \
  curl \
  gcc-c++ \
  cmake \
  Mesa-libGL1 \
  glib2-devel \
  gperftools \
  bc
```

### 1.2 Git 확인

```bash
git --version
```

### 1.3 NVIDIA CUDA 설치 (NVIDIA GPU 사용 시)

**Ubuntu / Debian:**

```bash
# CUDA 저장소 추가
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt update
sudo apt install -y cuda-toolkit-12-4

# NVIDIA 드라이버 (없을 경우)
sudo apt install -y nvidia-driver-550

# 재부팅 후 확인
nvidia-smi
```

> CUDA 11.8 이상 권장. 설치 후 `nvidia-smi`로 확인.

### 1.4 AMD ROCm 설치 (AMD GPU 사용 시)

[ROCm 공식 문서](https://rocm.docs.amd.com) 참조.  
또는 이 가이드 [7장](#7-amd-gpu-rocm-설정) 참조.

---

## 2. 리포지토리 클론

### 2.1 기본 설치 (권장)

```bash
cd ~/stable-diffusion
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

### 2.3 폴더 구조 확인

```
~/stable-diffusion/
└── stable-diffusion-webui-reForge/
    ├── webui.sh              # Linux/macOS 실행 스크립트
    ├── webui-user.sh         # 사용자 설정 (수정할 파일)
    ├── launch.py             # 진입점
    ├── models/               # 모델 폴더
    │   └── Stable-diffusion/ # 체크포인트 모델
    └── requirements.txt      # 의존성 목록
```

---

## 3. 최초 실행 및 자동 설치

### 3.1 webui-user.sh 설정

`webui-user.sh`을 열고 상황에 맞게 설정:

```bash
nano webui-user.sh
```

기본 설정 (수정 없이 사용 가능):

```bash
#!/bin/bash

# Install directory
# install_dir="/home/$(whoami)"

# Name of the subdirectory
# clone_dir="stable-diffusion-webui"

# Commandline arguments
# export COMMANDLINE_ARGS=""

# python3 executable
# python_cmd="python3.10"
```

### 3.2 webui.sh 실행

```bash
cd ~/stable-diffusion/stable-diffusion-webui-reForge
chmod +x webui.sh
./webui.sh
```

처음 실행 시 **자동**으로 진행되는 작업:

1. **Python 가상환경(venv)** 생성 (`venv/` 폴더)
2. **pip 업그레이드**
3. **PyTorch** (CUDA 버전) 자동 설치
4. **requirements** 자동 설치
5. **TCMalloc** 자동 탐색 및 적용 (메모리 최적화)
6. **기타 의존성** 자동 다운로드
7. **WebUI 실행** — http://127.0.0.1:7860 자동 오픈

> **소요 시간**: 인터넷 속도에 따라 **5~20분** 정도 소요됩니다.

### 3.3 백그라운드 실행 (선택사항)

```bash
# 화면 세션 사용
screen -S sd-webui
./webui.sh
# Ctrl+A, D 로 분리

# 또는 nohup
nohup ./webui.sh > webui.log 2>&1 &
```

### 3.4 시스템 서비스 등록 (선택사항)

`/etc/systemd/system/sd-webui.service`:

```ini
[Unit]
Description=Stable Diffusion WebUI reForge
After=network.target

[Service]
Type=simple
User=your-username
WorkingDirectory=/home/your-username/stable-diffusion/stable-diffusion-webui-reForge
ExecStart=/home/your-username/stable-diffusion/stable-diffusion-webui-reForge/webui.sh
Restart=on-failure
RestartSec=10

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl enable sd-webui
sudo systemctl start sd-webui
```

---

## 4. 모델 다운로드 및 배치

### 4.1 모델 파일 위치

```bash
# 메인 체크포인트
~/stable-diffusion/stable-diffusion-webui-reForge/models/Stable-diffusion/

# LoRA
~/stable-diffusion/stable-diffusion-webui-reForge/models/Lora/

# VAE
~/stable-diffusion/stable-diffusion-webui-reForge/models/VAE/

# Embeddings
~/stable-diffusion/stable-diffusion-webui-reForge/embeddings/

# ControlNet
~/stable-diffusion/stable-diffusion-webui-reForge/models/ControlNet/
```

### 4.2 wget/curl으로 모델 다운로드

```bash
cd ~/stable-diffusion/stable-diffusion-webui-reForge/models/Stable-diffusion/

# SDXL 1.0
wget -O sd_xl_base_1.0.safetensors \
  https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/resolve/main/sd_xl_base_1.0.safetensors

# DreamShaper (SD1.5)
wget -O dreamshaper_8.safetensors \
  https://huggingface.co/Lykon/dreamshaper-8/resolve/main/dreamshaper_8.safetensors
```

### 4.3 Hugging Face CLI 사용

```bash
pip install huggingface-hub

# 로그인 (선택사항)
huggingface-cli login

# 모델 다운로드
huggingface-cli download stabilityai/stable-diffusion-xl-base-1.0 \
  --local-dir ./models/Stable-diffusion/
```

---

## 5. 이미지 생성하기

### 5.1 WebUI 접속

브라우저에서 http://127.0.0.1:7860 접속

### 5.2 기본 txt2img

1. 좌측 상단 모델 드롭다운에서 모델 선택 (없으면 새로고침 버튼 클릭)
2. **txt2img** 탭 선택
3. 프롬프트 입력 (예: `a beautiful mountain landscape, digital art, highly detailed`)
4. Negative 프롬프트 입력 (예: `blurry, low quality, distorted`)
5. 설정:
   - **Sampling method**: `DPM++ 2M Karras` (추천)
   - **Sampling steps**: `20~30`
   - **Width/Height**: `512x512` (SD1.5) / `1024x1024` (SDXL)
   - **CFG Scale**: `7`
   - **Batch count**: `1`, **Batch size**: `1`
6. **Generate** 버튼 클릭

### 5.3 img2img (이미지 변환)

1. **img2img** 탭 선택
2. 이미지 업로드
3. 프롬프트 입력
4. **Denoising strength**: `0.4~0.7` (낮을수록 원본 유지)
5. **Generate** 클릭

### 5.4 네트워크에서 접속 (원격)

```bash
export COMMANDLINE_ARGS="--listen --port 7860"
```

> `--listen`으로 외부 접속 허용. 방화벽 설정 확인 필요.

---

## 6. 고급 설정 및 최적화

### 6.1 COMMANDLINE_ARGS 가이드

`webui-user.sh`에서 설정:

```bash
export COMMANDLINE_ARGS="--cuda-malloc --cuda-stream"
```

#### VRAM 절약 모드

```bash
export COMMANDLINE_ARGS="--always-offload-from-vram --unet-in-fp8-e4m3fn"
```

#### 고성능 모드 (RTX 30/40 시리즈)

```bash
export COMMANDLINE_ARGS="--cuda-malloc --cuda-stream --pin-shared-memory --use-sage-attention"
```

### 6.2 CUDA 아키텍처별 플래그

| 플래그 | 효과 | 권장 GPU |
|--------|------|----------|
| `--cuda-malloc` | cudaMallocAsync (약간 속도 ↑) | 모든 NVIDIA |
| `--cuda-stream` | 15~25% 속도 향상 | RTX 30/40 시리즈 |
| `--pin-shared-memory` | 20%+ 속도 (위험) | RTX 30/40 6GB+ |
| `--use-sage-attention` | SageAttention (triton 필요) | NVIDIA + triton |

### 6.3 데이터 타입 플래그

| 플래그 | VRAM 절약 | 품질 영향 |
|--------|-----------|----------|
| `--unet-in-fp8-e4m3fn` | 큼 | 약간 저하 |
| `--unet-in-fp8-e5m2` | 큼 | 약간 저하 |
| `--unet-in-bf16` | 중간 | 거의 없음 |
| `--unet-in-fp16` | 중간 | 거의 없음 |
| `--clip-in-fp8-e4m3fn` | 작음 | 거의 없음 |
| `--vae-in-fp16` | 작음 | 없음 |

### 6.4 TCMalloc 최적화

Linux에서 `webui.sh`는 자동으로 TCMalloc을 탐색하여 적용합니다.  
수동으로 비활성화하려면:

```bash
export NO_TCMALLOC=True
```

### 6.5 Swap 설정 (VRAM 부족 시)

```bash
# 16GB 스왑 파일 생성
sudo fallocate -l 16G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# 영구 적용 (/etc/fstab)
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

---

## 7. AMD GPU (ROCm) 설정

### 7.1 ROCm 설치

**Ubuntu 22.04:**

```bash
# AMR GPU 감지
lspci | grep -E "VGA|Display"

# ROCm 저장소 추가
wget https://repo.radeon.com/amdgpu-install/6.0/ubuntu/jammy/amdgpu-install_6.0.60002-1_all.deb
sudo dpkg -i amdgpu-install_6.0.60002-1_all.deb
sudo apt update
sudo apt install -y amdgpu-dkms rocm

# 사용자를 render, video 그룹에 추가
sudo usermod -a -G render,video $USER

# 재부팅
sudo reboot
```

### 7.2 ROCm 확인

```bash
rocm-smi
```

### 7.3 AMD GPU별 TORCH_COMMAND

**RX 5000 시리즈 (Navi 1)** — `HSA_OVERRIDE_GFX_VERSION=10.3.0` 필요:

| Python | 명령어 |
|--------|--------|
| 3.10 | `pip install torch==2.9.0+rocm6.4 ...` |
| 3.9 | `pip install torch==2.8.0+rocm6.4 ...` |
| 3.8 | `pip install torch==2.4.0+rocm5.2 ...` |

**RX 6000 시리즈 (Navi 2)** — `HSA_OVERRIDE_GFX_VERSION=10.3.0`  
**RX 7000 시리즈 (Navi 3)**:

```bash
export TORCH_COMMAND="pip install torch torchvision --index-url https://download.pytorch.org/whl/rocm6.4"
```

**Renoir (APU)**:

```bash
export HSA_OVERRIDE_GFX_VERSION=9.0.0
# VRAM 4GB+ / RAM 10GB+ 권장
```

### 7.4 webui-user.sh AMD 설정 예시

```bash
#!/bin/bash

# Navi 2 (RX 6000) 설정
export HSA_OVERRIDE_GFX_VERSION=10.3.0
export TORCH_COMMAND="pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/rocm5.7"
export COMMANDLINE_ARGS=""
```

---

## 8. 문제 해결

### 8.1 `python3.10-venv` 오류

**증상**: `python3-venv is not installed`  
**해결**:

```bash
sudo apt install -y python3.10-venv python3.10-dev
```

### 8.2 CUDA Out of Memory

**증상**: `torch.cuda.OutOfMemoryError`  
**해결**:

```bash
export COMMANDLINE_ARGS="--always-offload-from-vram --unet-in-fp8-e4m3fn --medvram"
```

해상도 낮추기, Batch size = 1 유지.

### 8.3 검은색 이미지 (NaN 출력)

**증상**: 생성된 이미지가 완전 검은색  
**원인**: `--cuda-stream`이 구형 GPU와 충돌  
**해결**: `--cuda-stream` 제거

```bash
export COMMANDLINE_ARGS="--no-half"
```

### 8.4 TCMalloc 미탐지

**증상**: 경고: `Cannot locate TCMalloc`  
**해결**:

```bash
# Ubuntu/Debian
sudo apt install -y libgoogle-perftools-dev

# Fedora
sudo dnf install -y gperftools-libs

# openSUSE
sudo zypper install -y gperftools
```

### 8.5 libGL 에러

**증상**: `libGL: MESA-LOADER: failed to open`  
**해결**:

```bash
sudo apt install -y libgl1-mesa-glx libglib2.0-0
```

### 8.6 포트 충돌

```bash
export COMMANDLINE_ARGS="--port 7861"
```

### 8.7 WebUI를 원격으로 접속 시 안전성

```bash
# --listen은 인증 없음. 프록시나 SSH 터널 사용 권장
# SSH 터널링:
ssh -L 7860:localhost:7860 user@server
```

---

## 9. 업데이트

### 9.1 기본 업데이트

```bash
cd ~/stable-diffusion/stable-diffusion-webui-reForge
git pull
```

### 9.2 업데이트 후 문제 발생 시

reForge 메인 브랜치는 활발히 개발 중이며 불안정할 수 있습니다.  
안정적인 브랜치로 전환:

```bash
git checkout main-old
```

### 9.3 안정적인 대체 프로젝트

- **[Forge Classic](https://github.com/Haoming02/sd-webui-forge-classic)** — 안정화된 Forge 기반
- **[Forge Neo](https://github.com/Haoming02/sd-webui-forge-classic/tree/neo)** — Forge2 + Flux/FP8/GGUF
- **[ersatzForge](https://github.com/DenOfEquity/ersatzForge)** — Forge2 실험적 포크

---

## 참고 자료

- [reForge GitHub](https://github.com/Panchovix/stable-diffusion-webui-reForge)
- [Forge GitHub (원본)](https://github.com/lllyasviel/stable-diffusion-webui-forge)
- [AUTOMATIC1111 WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui)
- [ROCm 문서](https://rocm.docs.amd.com)
- [CivitAI (모델)](https://civitai.com)
- [Hugging Face (모델)](https://huggingface.co)
