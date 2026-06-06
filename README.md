# Creative AI — 로컬 생성형 AI 통합 가이드 2026

> **기준**: 2026년 6월 · 무료(Free/OSS) · 로컬 설치형 · Windows / macOS / Linux  
> **관점**: 디자인 실무자 & 엔지니어

---

## 📂 컬렉션 구조

```
CreativeAI/
├── local_image_ai_guide/          # 로컬 이미지 생성 AI 가이드
│   └── README.md                  # ComfyUI · InvokeAI · Krita+AI · SwarmUI · Fooocus
├── local_video_music_voice_ai_guide/  # 로컬 영상 · 음악 · 음성 AI 가이드
│   └── README.md                  # Wan 2.7 · HunyuanVideo · ACE-Step · Kokoro · XTTS v2
└── stable-diffusion/              # Stable Diffusion WebUI reForge 설치 가이드
    ├── README.md                  # (이 파일) — 컬렉션 최상위 요약
    ├── INSTALL_WINDOWS.md         # Windows 전용 설치 가이드
    ├── INSTALL_MAC.md             # macOS 전용 설치 가이드
    └── INSTALL_LINUX.md           # Linux 전용 설치 가이드
```

---

## 📖 가이드 목록

| # | 가이드 | 경로 | 주요 내용 |
|---|--------|------|----------|
| 1 | **로컬 이미지 생성 AI 추천 가이드 2026** | [`local_image_ai_guide/README.md`](../local_image_ai_guide/README.md) | 5개 이미지 생성 툴 비교·설치·워크플로우 |
| 2 | **로컬 영상 · 음악 · 음성 생성 AI 추천 가이드 2026** | [`local_video_music_voice_ai_guide/README.md`](../local_video_music_voice_ai_guide/README.md) | 영상 5종 · 음악 3종 · TTS 2종 |
| 3 | **Stable Diffusion WebUI reForge 설치 가이드** | [`stable-diffusion/README.md`](./README.md) | Windows / macOS / Linux 설치부터 생성까지 |

---

# 🖼️ 1. 로컬 이미지 생성 AI

> 상세: [`local_image_ai_guide/README.md`](../local_image_ai_guide/README.md)

2026년 로컬 이미지 생성 AI 도구들을 **디자인 실무자 관점**에서 정리한 가이드.  
각 툴의 장단점, 최소 VRAM, 진입장벽, 추천 사용자를 한눈에 비교한다.

### 포함된 툴

| 툴 | GitHub | OS | 최소 VRAM | 진입장벽 | 핵심 특징 |
|----|--------|:--:|:---------:|:--------:|-----------|
| **ComfyUI** | [comfyanonymous/ComfyUI](https://github.com/comfyanonymous/ComfyUI) | W/M/L | 4GB | ★★★★☆ | 노드 그래프 파이프라인, 최신 모델 day-0 지원 |
| **InvokeAI** | [invoke-ai/InvokeAI](https://github.com/invoke-ai/InvokeAI) | W/M/L | 6GB | ★★★☆☆ | 무한 캔버스, 인페인팅·아웃페인팅, Photoshop 방식 |
| **Krita + AI Diffusion** | [Acly/krita-ai-diffusion](https://github.com/Acly/krita-ai-diffusion) | W/M/L | 4GB* | ★★★☆☆ | 페인팅 통합, Adobe Firefly 무료 대체 |
| **SwarmUI** | [mcmonkeyprojects/SwarmUI](https://github.com/mcmonkeyprojects/SwarmUI) | W/M/L | 4GB | ★★★☆☆ | ComfyUI 백엔드 + 단순 UI, 멀티 GPU · 팀 공유 |
| **Fooocus** | [lllyasviel/Fooocus](https://github.com/lllyasviel/Fooocus) | W/M/L | 4GB | ★★☆☆☆ | Midjourney 스타일, 초간단 UX |

\* Krita AI Diffusion은 Interstice 클라우드 연결 시 GPU 불필요

### 🎯 용도별 추천

| 하고 싶은 작업 | 추천 툴 |
|----------------|---------|
| 파이프라인 자동화, 배치 생성, 최신 모델 테스트 | **ComfyUI** |
| 포토샵 방식 편집, 인페인팅·아웃페인팅 | **InvokeAI** |
| 드로잉과 AI 생성 병행 | **Krita + AI Diffusion** |
| 팀 GPU 서버 공유, 교육 환경 | **SwarmUI** |
| 가장 쉽게 로컬 AI 시작 | **Fooocus** |
| 여러 툴 전환하며 실험 | **Stability Matrix** (런처) |

---

# 🎬 2. 로컬 영상 · 음악 · 음성 생성 AI

> 상세: [`local_video_music_voice_ai_guide/README.md`](../local_video_music_voice_ai_guide/README.md)

영상 생성, 음악 생성, 음성(TTS) 생성까지 **전체 크리에이티브 AI 파이프라인**을 다루는 가이드.  
이미지 AI 가이드와 연계하여 End-to-End 워크플로우 구성이 가능하다.

## 🎥 영상 생성 AI

| 모델 | 개발사 | OS | 최소 VRAM | 속도 | 퀄리티 | 라이선스 | 특화 분야 |
|------|--------|:--:|:---------:|:----:|:------:|:--------:|-----------|
| **Wan 2.7** | Alibaba | W/M/L | 6GB | 보통 | ★★★★☆ | Apache 2.0 | 범용, 상업 활용 |
| **LTX-Video 2.3** | Lightricks | W/M/L | 8GB | ⚡최고 | ★★★☆☆ | 비상업 무료 | 속도, 네이티브 오디오 |
| **HunyuanVideo** | Tencent | W/M/L | 8GB* | 느림 | ★★★★★ | 커뮤니티 | 시네마틱 퀄리티 |
| **CogVideoX** | Zhipu AI | W/M/L | 8GB | 보통 | ★★★☆☆ | Apache 2.0 | 스토리텔링 |
| **AnimateDiff** | 커뮤니티 | W/M/L | **4GB** | 보통 | ★★☆☆☆ | Apache 2.0 | 저사양, 애니메이션 루프 |

\* HunyuanVideo: 8GB 구동 가능하나 24GB 권장

> **공통 인터페이스**: 위 5개 모델 모두 **ComfyUI**로 실행 가능.  
> **Pinokio**(원클릭 런처)로도 설치 가능.

## 🎵 음악 생성 AI

| 모델 | 개발사 | OS | 최소 VRAM | 속도 | 라이선스 | 특화 분야 |
|------|--------|:--:|:---------:|:----:|:--------:|-----------|
| **ACE-Step 1.5** | ACE Studio + StepFun | W/M/L | **4GB** | ⚡10초 | Apache 2.0 | 풀 곡+보컬, 초고속 |
| **AudioCraft / MusicGen** | Meta | W/M/L | 8GB | 보통 | MIT | 기악, 효과음, 연구 |
| **Stable Audio Open** | Stability AI | W/M/L | 8GB | 보통 | MIT | 루프, 사운드 디자인 |

## 🗣️ 음성(TTS) 생성 AI

| 모델 | 개발사 | OS | 최소 VRAM | 속도 | 라이선스 | 특화 분야 |
|------|--------|:--:|:---------:|:----:|:--------:|-----------|
| **Kokoro-82M** | Hexgrad | W/M/L | **CPU 가능** | ⚡210× | Apache 2.0 | 초경량, 다국어, 50+ 음성 |
| **Coqui XTTS v2** | Coqui AI | W/M/L | CPU 가능 | 보통 | CPML | 6초 샘플 음성 클로닝, 16개 언어 |

### 🎯 용도별 추천

| 하고 싶은 작업 | 추천 툴 | 인터페이스 |
|----------------|---------|-----------|
| 상업용 영상 (B-roll, 소셜미디어) | **Wan 2.7** | ComfyUI |
| 빠른 콘셉트 시안 영상 | **LTX-Video 2.3** | ComfyUI |
| 고품질 시네마틱 영상 | **HunyuanVideo** | ComfyUI |
| 저사양 PC 영상 생성 | **AnimateDiff** | ComfyUI / A1111 |
| 배경음악 · 광고 BGM | **ACE-Step 1.5** | ComfyUI / Pinokio |
| 게임 효과음 · 앰비언트 | **Stable Audio Open** | Python / Gradio |
| 영상 나레이션 (GPU 불필요) | **Kokoro-82M** | Python / API |
| 다국어 더빙 · 음성 클로닝 | **Coqui XTTS v2** | Python / Gradio |

---

# ⚙️ 3. Stable Diffusion WebUI reForge — 설치 가이드

> 상세: [`stable-diffusion/`](./)

**reForge**는 AUTOMATIC1111 WebUI 기반 포크로, **SDXL 4GB · SD1.5 2GB VRAM**에서 구동되는 최적화된 WebUI입니다.

### 플랫폼별 설치 가이드

| 플랫폼 | 바로가기 | 핵심 내용 |
|--------|----------|-----------|
| ![Windows](https://img.shields.io/badge/Windows-0078D6?style=flat-square) | [`INSTALL_WINDOWS.md`](./INSTALL_WINDOWS.md) | NVIDIA CUDA, AMD DirectML, webui-user.bat 설정, VRAM 최적화 |
| ![macOS](https://img.shields.io/badge/macOS-000000?style=flat-square) | [`INSTALL_MAC.md`](./INSTALL_MAC.md) | Apple Silicon MPS, Intel Mac, webui-macos-env.sh |
| ![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat-square) | [`INSTALL_LINUX.md`](./INSTALL_LINUX.md) | NVIDIA CUDA, AMD ROCm, TCMalloc, systemd 서비스 |

### 빠른 시작

```bash
# 클론
git clone https://github.com/Panchovix/stable-diffusion-webui-reForge.git
cd stable-diffusion-webui-reForge

# Windows
.\webui-user.bat

# macOS / Linux
chmod +x webui.sh
./webui.sh
```

> 최초 실행 시 모든 의존성을 자동 설치하며, 설치 후 http://127.0.0.1:7860 에서 WebUI에 접속할 수 있습니다.

### 핵심 최적화 플래그

| 목적 | 플래그 |
|------|--------|
| VRAM 절약 | `--always-offload-from-vram --unet-in-fp8-e4m3fn` |
| 성능 향상 (RTX 30/40) | `--cuda-malloc --cuda-stream --use-sage-attention` |
| macOS 필수 | `--skip-torch-cuda-test --upcast-sampling --no-half-vae --use-cpu interrogate` |

---

# 🔗 통합 파이프라인 구성

이미지 + 영상 + 음악 + 음성을 **하나의 파이프라인**으로 연결하는 방법.

```
[ComfyUI]
  │
  ├── 이미지 생성 (SDXL / FLUX)
  ├── 영상 생성 (Wan / HunyuanVideo / AnimateDiff)
  ├── 음악 생성 (ACE-Step → ComfyUI 노드)
  └── 음성 생성 (Kokoro / XTTS → Python API)
```

**추천 도구**:
- **Stability Matrix**: 이미지 AI WebUI 통합 런처
- **Pinokio**: 영상/음악/음성 AI 원클릭 설치 런처
- **ComfyUI**: 모든 생성 AI를 하나의 인터페이스로 연결

---

# 📊 한눈에 비교

## 전체 툴 비교

| 도메인 | 1순위 (범용) | 2순위 (전문) | 저사양 대안 |
|--------|-------------|-------------|------------|
| 🖼️ 이미지 생성 | **ComfyUI** | InvokeAI / Krita+AI | Fooocus |
| 🎬 영상 생성 | **Wan 2.7** | HunyuanVideo / LTX-Video | AnimateDiff |
| 🎵 음악 생성 | **ACE-Step 1.5** | AudioCraft / Stable Audio | — |
| 🗣️ 음성 생성 | **Kokoro-82M** | Coqui XTTS v2 | Kokoro (CPU) |
| 🚀 설치·관리 | **Stability Matrix** / **Pinokio** | 수동 설치 | — |

## 플랫폼 호환성

| 툴 | Windows | macOS | Linux | GPU 필요 | 권장 VRAM |
|----|:-------:|:-----:|:-----:|:--------:|:---------:|
| ComfyUI | ✅ | ✅ | ✅ | 선택 | 4GB+ |
| InvokeAI | ✅ | ✅ | ✅ | ✅ | 6GB+ |
| Krita + AI Diffusion | ✅ | ✅ | ✅ | 선택* | 4GB+ |
| SwarmUI | ✅ | ✅ | ✅ | ✅ | 4GB+ |
| Fooocus | ✅ | ✅ | ✅ | ✅ | 4GB+ |
| Wan 2.7 | ✅ | ✅ | ✅ | ✅ | 6GB+ |
| LTX-Video 2.3 | ✅ | ✅ | ✅ | ✅ | 8GB+ |
| HunyuanVideo | ✅ | ✅ | ✅ | ✅ | 8GB+ |
| ACE-Step 1.5 | ✅ | ✅ | ✅ | 선택 | 4GB+ |
| Kokoro-82M | ✅ | ✅ | ✅ | ❌ (CPU OK) | 0GB |
| Coqui XTTS v2 | ✅ | ✅ | ✅ | ❌ (CPU OK) | 0GB |

\* Krita AI Diffusion은 Interstice 클라우드 연결 시 GPU 불필요

---

# 🛠️ 보조 도구

| 도구 | GitHub / 사이트 | 용도 |
|------|----------------|------|
| **Stability Matrix** | [LykosAI/StabilityMatrix](https://github.com/LykosAI/StabilityMatrix) | 이미지 AI WebUI 통합 런처 |
| **Pinokio** | [pinokio.computer](https://pinokio.computer) | AI 앱 원클릭 설치 런처 |
| **CivitAI** | [civitai.com](https://civitai.com) | 모델 · LoRA · 워크플로우 마켓플레이스 |
| **Hugging Face** | [huggingface.co](https://huggingface.co) | 모델 · 데이터셋 허브 |

---

# 📚 전체 참고 링크

### 이미지 생성
- [ComfyUI](https://github.com/comfyanonymous/ComfyUI)
- [InvokeAI](https://github.com/invoke-ai/InvokeAI)
- [Krita AI Diffusion](https://github.com/Acly/krita-ai-diffusion)
- [SwarmUI](https://github.com/mcmonkeyprojects/SwarmUI)
- [Fooocus](https://github.com/lllyasviel/Fooocus)

### 영상 생성
- [Wan 2.7](https://github.com/Wan-Video/Wan2.1)
- [LTX-Video](https://github.com/Lightricks/LTX-Video)
- [HunyuanVideo](https://github.com/Tencent/HunyuanVideo)
- [CogVideoX](https://github.com/THUDM/CogVideo)
- [AnimateDiff](https://github.com/guoyww/AnimateDiff)

### 음악 생성
- [ACE-Step 1.5](https://github.com/ace-step/ACE-Step-1.5)
- [AudioCraft (MusicGen)](https://github.com/facebookresearch/audiocraft)
- [Stable Audio Open](https://github.com/Stability-AI/stable-audio-tools)

### 음성(TTS) 생성
- [Kokoro-82M](https://github.com/hexgrad/kokoro)
- [Coqui XTTS v2](https://github.com/coqui-ai/TTS)

### Stable Diffusion WebUI reForge
- [reForge GitHub](https://github.com/Panchovix/stable-diffusion-webui-reForge)
- [Forge GitHub (원본)](https://github.com/lllyasviel/stable-diffusion-webui-forge)
- [AUTOMATIC1111 WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui)

### 대체 프로젝트 (reForge 안정성 이슈 시)
- [Forge Classic](https://github.com/Haoming02/sd-webui-forge-classic) — 안정화된 Forge 기반
- [Forge Neo](https://github.com/Haoming02/sd-webui-forge-classic/tree/neo) — Forge2 + Flux/FP8/GGUF
- [ersatzForge](https://github.com/DenOfEquity/ersatzForge) — Forge2 실험적 포크

---

> **문서 위치**: `C:\Users\Administrator\Desktop\stable-diffusion\README.md`  
> **컬렉션 위치**: `C:\Users\Administrator\Downloads\CreativeAI\`  
> **생성일**: 2026-06-06  
> **관련 가이드**: 
> - [`local_image_ai_guide/README.md`](../local_image_ai_guide/README.md)
> - [`local_video_music_voice_ai_guide/README.md`](../local_video_music_voice_ai_guide/README.md)
> - [`stable-diffusion/INSTALL_WINDOWS.md`](./INSTALL_WINDOWS.md)
> - [`stable-diffusion/INSTALL_MAC.md`](./INSTALL_MAC.md)
> - [`stable-diffusion/INSTALL_LINUX.md`](./INSTALL_LINUX.md)
