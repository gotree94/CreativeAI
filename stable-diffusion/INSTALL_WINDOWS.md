# Stable Diffusion WebUI reForge — Windows 설치 가이드

> **대상 환경**: Windows 10/11 (64bit)  
> **권장 Python**: 3.10 ~ 3.12 (3.13은 일부 호환성 문제 있음)  
> **GPU**: NVIDIA CUDA (권장), AMD DirectML, 또는 CPU 모드  
> **최소 VRAM**: SD1.5 — 2GB / SDXL — 4GB

---

## 목차

1. [사전 준비](#1-사전-준비)
2. [리포지토리 클론](#2-리포지토리-클론)
3. [최초 실행 및 자동 설치](#3-최초-실행-및-자동-설치)
4. [모델 다운로드 및 배치](#4-모델-다운로드-및-배치)
5. [이미지 생성하기](#5-이미지-생성하기)
6. [고급 설정 및 최적화](#6-고급-설정-및-최적화)
7. [문제 해결](#7-문제-해결)
8. [업데이트](#8-업데이트)

---

## 1. 사전 준비

### 1.1 Git 설치

1. [Git for Windows](https://git-scm.com/download/win) 다운로드
2. 설치 옵션은 기본값 유지 (PATH에 Git 추가 옵션 확인)
3. 설치 확인:
   ```cmd
   git --version
   ```

### 1.2 Python 설치

1. [Python 3.10.11 권장](https://www.python.org/downloads/release/python-31011/) 다운로드
   - 또는 Python 3.11, 3.12 사용 가능
2. 설치 시 **"Add Python to PATH"** 반드시 체크
3. **"Install launcher for all users"** 체크
4. 설치 확인:
   ```cmd
   python --version
   ```

### 1.3 NVIDIA CUDA (NVIDIA GPU 사용 시)

1. [CUDA 11.8 이상](https://developer.nvidia.com/cuda-downloads) 설치
2. [cuDNN](https://developer.nvidia.com/cudnn) (선택사항, 성능 향상)
3. 설치 확인:
   ```cmd
   nvidia-smi
   ```

> **AMD GPU 사용자**: DirectML 지원 예정 — `--directml` 플래그 사용  
> **GPU가 없는 경우**: CPU 모드로 실행 가능 (매우 느림)

---

## 2. 리포지토리 클론

### 2.1 기본 설치 (권장)

```cmd
cd C:\Users\Administrator\Desktop\stable-diffusion
git clone https://github.com/Panchovix/stable-diffusion-webui-reForge.git
cd stable-diffusion-webui-reForge
git checkout main
```

### 2.2 A1111 WebUI가 이미 있는 경우

기존 AUTOMATIC1111 설치 디렉토리에서 reForge를 브랜치로 추가:

```cmd
cd C:\path\to\existing\stable-diffusion-webui
git remote add reForge https://github.com/Panchovix/stable-diffusion-webui-reForge.git
git fetch reForge
git checkout -b reForge-main reForge/main
```

> 원래 A1111로 돌아가려면: `git checkout master`

### 2.3 Windows 7 또는 CUDA 11.x 사용 시

CUDA 11.x 호환을 위해 requirements 파일 교체:

```cmd
cd stable-diffusion-webui-reForge
ren requirements_versions.txt requirements_versions_backup.txt
copy requirements_versions_legacy.txt requirements_versions.txt
```

---

## 3. 최초 실행 및 자동 설치

### 3.1 webui-user.bat 실행

```cmd
cd C:\Users\Administrator\Desktop\stable-diffusion\stable-diffusion-webui-reForge
.\webui-user.bat
```

처음 실행 시 **자동**으로 진행되는 작업:

1. **Python 가상환경(venv)** 생성 (`venv\` 폴더)
2. **pip 업그레이드**
3. **PyTorch** (CUDA 버전) 자동 설치
4. **requirements** (xformers, transformers 등) 자동 설치
5. **기타 의존성** (GFPGAN, CLIP 등) 자동 다운로드
6. **WebUI 실행** — http://127.0.0.1:7860 자동 오픈

> **소요 시간**: 인터넷 속도에 따라 **5~20분** 정도 소요됩니다.

### 3.2 webui-user.bat 설정 파일

`webui-user.bat`을 편집하여 설정 변경:

```bat
@echo off

set PYTHON=
set GIT=
set VENV_DIR=
set COMMANDLINE_ARGS=

@REM 기존 A1111 체크아웃 참조 (선택사항)
@REM set A1111_HOME=C:\path\to\A1111
@REM set VENV_DIR=%A1111_HOME%\venv
@REM set COMMANDLINE_ARGS=%COMMANDLINE_ARGS% ^
@REM  --ckpt-dir %A1111_HOME%\models\Stable-diffusion ^
@REM  --hypernetwork-dir %A1111_HOME%\models\hypernetworks ^
@REM  --embeddings-dir %A1111_HOME%\embeddings ^
@REM  --lora-dir %A1111_HOME%\models\Lora

call webui.bat
```

주요 설정 변수:

| 변수 | 설명 | 예시 |
|------|------|------|
| `PYTHON` | Python 실행 파일 경로 | `set PYTHON=C:\Python310\python.exe` |
| `GIT` | Git 실행 파일 경로 | `set GIT=C:\Program Files\Git\bin\git.exe` |
| `VENV_DIR` | 가상환경 경로 | `set VENV_DIR=venv` |
| `COMMANDLINE_ARGS` | WebUI 실행 인자 | `set COMMANDLINE_ARGS=--medvram` |

---

## 4. 모델 다운로드 및 배치

### 4.1 모델 파일 위치

CLI나 브라우저로 모델을 다운로드하여 아래 폴더에 넣습니다.

| 모델 종류 | 폴더 위치 |
|-----------|----------|
| Checkpoint (메인 모델) | `models\Stable-diffusion\` |
| LoRA | `models\Lora\` |
| VAE | `models\VAE\` |
| Embeddings | `embeddings\` |
| Hypernetwork | `models\hypernetworks\` |
| ControlNet | `models\ControlNet\` |

### 4.2 추천 모델 (2026년 기준)

- **SD1.5**: [realisticVision](https://civitai.com/models/4201), [DreamShaper](https://civitai.com/models/4384)
- **SDXL**: [SDXL 1.0 (official)](https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0), [Juggernaut XL](https://civitai.com/models/133005)
- **FLUX (Forge2 브랜치)**: [FLUX.1-dev](https://huggingface.co/black-forest-labs/FLUX.1-dev)

### 4.3 Hugging Face에서 직접 다운로드

```cmd
cd models\Stable-diffusion\
curl -L -o sd_xl_base_1.0.safetensors ^
  https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/resolve/main/sd_xl_base_1.0.safetensors
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

### 5.4 주요 기능 탭

| 탭 | 설명 |
|----|------|
| **txt2img** | 텍스트로 이미지 생성 |
| **img2img** | 이미지 기반 변환 |
| **Extras** | 이미지 업스케일, 노이즈 제거 |
| **PNG Info** | PNG 메타데이터 확인 |
| **Checkpoint Merger** | 모델 병합 |
| **Train** | Textual Inversion / DreamBooth 학습 |

---

## 6. 고급 설정 및 최적화

### 6.1 COMMANDLINE_ARGS 최적화

`webui-user.bat`의 `COMMANDLINE_ARGS`에 아래 플래그를 추가할 수 있습니다.

#### 메모리 최적화

```bat
set COMMANDLINE_ARGS=^
--always-offload-from-vram
```

> 모델을 항상 VRAM에서 언로드. 느려지지만 여러 프로그램 동시 사용 시 유용.

#### 성능 최적화

```bat
set COMMANDLINE_ARGS=^
--cuda-malloc ^
--cuda-stream
```

> `--cuda-malloc`: cudaMallocAsync 사용 (약간의 성능 향상, 불안정 가능성)  
> `--cuda-stream`: CUDA 스트림 사용 (15~25% 속도 향상, RTX 30/40 시리즈)

#### 주의: 위험 플래그

```bat
set COMMANDLINE_ARGS=^
--cuda-stream ^
--pin-shared-memory
```

> `--pin-shared-memory`는 공유 GPU 메모리 사용. GTX 1060 등 구형 GPU에서 크래시 가능성 높음.

#### 저사양 최적화 예시

```bat
set COMMANDLINE_ARGS=^
--always-offload-from-vram ^
--attention-split ^
--unet-in-fp8-e4m3fn ^
--disable-xformers
```

### 6.2 데이터 타입 플래그

| 플래그 | 설명 |
|--------|------|
| `--all-in-fp32` | 모든 연산 FP32 (정확도 ↑, VRAM ↑) |
| `--all-in-fp16` | 모든 연산 FP16 |
| `--unet-in-bf16` | UNet만 BF16 |
| `--unet-in-fp8-e4m3fn` | UNet FP8 (VRAM ↓↓, 화질 ↓ 약간) |
| `--unet-in-fp8-e5m2` | UNet FP8 (대안) |
| `--vae-in-fp16` | VAE FP16 |
| `--clip-in-fp8-e4m3fn` | CLIP FP8 (VRAM ↓) |

### 6.3 Attention 최적화

| 플래그 | 설명 |
|--------|------|
| `--use-sage-attention` | SageAttention 사용 (triton 필요, 고속) |
| `--attention-split` | Split attention (호환성 ↑) |
| `--attention-pytorch` | PyTorch 2.0 attention |
| `--disable-xformers` | xformers 비활성화 → SDP 사용 |

### 6.4 기타 플래그

| 플래그 | 설명 |
|--------|------|
| `--always-gpu` | 모든 모델 GPU에 유지 |
| `--always-high-vram` | VRAM에 모델 상주 |
| `--always-low-vram` | UNet 분할 → VRAM 절약 |
| `--always-cpu` | CPU 모드 (매우 느림) |
| `--gpu-device-id 0` | 특정 GPU 선택 |

---

## 7. 문제 해결

### 7.1 Python 인식 불가

**증상**: `python` 명령어가 실행되지 않음  
**해결**: Python 재설치 + "Add Python to PATH" 체크 확인  
`webui-user.bat`에서 PYTHON 경로 직접 지정:

```bat
set PYTHON=C:\Python310\python.exe
```

### 7.2 CUDA Out of Memory (OOM)

**증상**: `CUDA out of memory` 에러  
**해결**:

```bat
set COMMANDLINE_ARGS=--always-offload-from-vram --unet-in-fp8-e4m3fn
```

또는 이미지 해상도를 낮추고 Batch size를 1로 설정.

### 7.3 xformers 설치 실패

**증상**: xformers 빌드 에러  
**해결**: `--disable-xformers` 추가:

```bat
set COMMANDLINE_ARGS=--disable-xformers
```

reForge는 기본 SDP attention을 사용하므로 xformers 없이도 정상 작동.

### 7.4 검은색 이미지 (NaN 출력)

**증상**: 생성된 이미지가 완전 검은색  
**해결**: `--cuda-stream` 제거, `--no-half` 추가 (성능 저하)

### 7.5 포트 충돌

**증상**: `Address already in use` 에러  
**해결**: 포트 변경:

```bat
set COMMANDLINE_ARGS=--port 7861
```

---

## 8. 업데이트

### 8.1 기본 업데이트

```cmd
cd C:\Users\Administrator\Desktop\stable-diffusion\stable-diffusion-webui-reForge
git pull
```

### 8.2 업데이트 후 문제 발생 시

reForge 메인 브랜치는 안정적이지 않을 수 있음.  
더 안정적인 브랜치로 전환:

```cmd
git checkout main-old
```

### 8.3 안정적인 대체 프로젝트

만약 reForge가 불안정하다면 아래 프로젝트 추천:

- **[Forge Classic](https://github.com/Haoming02/sd-webui-forge-classic)** — reForge/Forge 기능 + 최적화, 기존 Forge 백엔드
- **[Forge Neo](https://github.com/Haoming02/sd-webui-forge-classic/tree/neo)** — Forge2 기반, Flux/FP8/GGUF 지원
- **[ersatzForge](https://github.com/DenOfEquity/ersatzForge)** — Forge2 기반 실험적 포크

---

## 참고 자료

- [reForge GitHub](https://github.com/Panchovix/stable-diffusion-webui-reForge)
- [Forge GitHub (원본)](https://github.com/lllyasviel/stable-diffusion-webui-forge)
- [AUTOMATIC1111 WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui)
- [CivitAI (모델)](https://civitai.com)
- [Hugging Face (모델)](https://huggingface.co)
