# 로컬 영상 · 음악 · 음성 생성 AI 추천 가이드 2026

> **기준**: 2026년 6월 · 무료(Free/OSS) · 로컬 설치형 · 디자인 실무자 관점  
> **참고**: 이미지 생성 AI 가이드와 연계하여 활용하면 전체 크리에이티브 파이프라인 구축 가능

---

## 목차

### 🎬 영상(Video) 생성 AI
1. [Wan 2.7 (Alibaba)](#1-wan-27--영상-생성-1순위)
2. [LTX-Video 2.3 (Lightricks)](#2-ltx-video-23--최고속-영상-생성)
3. [HunyuanVideo (Tencent)](#3-hunyuanvideo--시네마틱-퀄리티)
4. [CogVideoX (Zhipu AI)](#4-cogvideox--롱폼-스토리텔링)
5. [AnimateDiff + ComfyUI](#5-animatediff--저사양-입문용)

### 🎵 음악(Music) 생성 AI
1. [ACE-Step 1.5](#1-ace-step-15--음악-생성-1순위)
2. [AudioCraft / MusicGen (Meta)](#2-audiocraft--musicgen--오픈소스-표준)
3. [Stable Audio Open (Stability AI)](#3-stable-audio-open--사운드-디자인-특화)

### 🗣️ 음성(TTS / Voice) 생성 AI
1. [Kokoro-82M](#1-kokoro-82m--음성-생성-1순위)
2. [Coqui XTTS v2](#2-coqui-xtts-v2--음성-클로닝-표준)

### 📊 종합 비교표
- [영상 AI 비교표](#영상-ai-한눈에-보는-비교표)
- [음악·음성 AI 비교표](#음악--음성-ai-한눈에-보는-비교표)
- [RTX 5090 환경 추천 조합](#rtx-5090-환경-추천-조합)

---

# 🎬 영상(Video) 생성 AI

> **공통 인터페이스**: 아래 5개 모델 모두 **ComfyUI**를 통해 실행하는 것이 표준  
> **Pinokio**: ComfyUI 없이 원클릭 설치를 원한다면 [Pinokio](https://pinokio.computer) 사용 가능 (Win/Mac/Linux)

---

## 1. Wan 2.7 — 영상 생성 1순위

**개발사**: Alibaba (완 시리즈)  
**GitHub**: https://github.com/Wan-Video/Wan2.1  
**라이선스**: Apache 2.0 (상업적 사용 가능)  
**OS 지원**: Windows / macOS / Linux  
**최소 VRAM**: 6GB (GGUF 양자화 시) / 16GB+ (권장)  
**진입장벽**: ★★★☆☆ (보통 · ComfyUI 필요)

### 개요

2026년 커뮤니티가 가장 강력하게 추천하는 로컬 오픈소스 영상 생성 모델. Wan 2.7은 2026년 4월 출시된 최신 버전으로, first/last-frame 제어, 9-grid 멀티이미지 입력 등 콘텐츠 제작자 친화적 기능을 대폭 강화했다. GGUF 양자화를 통해 소비자용 GPU에서도 실행 가능하며, Apache 2.0 라이선스로 상업적 활용에 제약이 없다.

### 주요 스펙

| 항목 | 내용 |
|------|------|
| 파라미터 | 1.3B (lite) / 14B (full) |
| 출력 해상도 | 480p ~ 720p |
| 클립 길이 | 5초 (기본) |
| 인터페이스 | ComfyUI, Pinokio |
| 라이선스 | Apache 2.0 |

### 장점

- 오픈소스 영상 생성 분야 사실상 표준 모델
- GGUF 양자화로 6GB VRAM에서도 구동 가능
- 이미지→영상(I2V), 텍스트→영상(T2V) 모두 지원
- Apache 2.0으로 상업 프로젝트에 자유롭게 사용
- CivitAI, HuggingFace에 파인튜닝 모델 다수 존재

### 단점

- 14B 풀 모델은 16GB+ VRAM 필요
- 생성 시간 다소 느림 (RTX 4090 기준 5초 클립에 ~90초)
- ComfyUI 학습 필요

### 추천 대상

모션 그래픽, B-roll 영상, 소셜미디어 콘텐츠 제작 디자이너

---

## 2. LTX-Video 2.3 — 최고속 영상 생성

**개발사**: Lightricks  
**GitHub**: https://github.com/Lightricks/LTX-Video  
**라이선스**: Lightricks Open Model License (비상업 무료)  
**OS 지원**: Windows / macOS / Linux  
**최소 VRAM**: 8GB (2B 버전)  
**진입장벽**: ★★★☆☆ (보통)

### 개요

속도에 최적화된 영상 생성 모델로, RTX 4090 기준 5초 클립을 **4초** 만에 생성한다. 2026년 3월 출시된 2.3 버전은 VAE 재설계, 4배 대용량 텍스트 커넥터, **네이티브 오디오 생성** 기능을 추가했다. 빠른 콘셉트 프로토타이핑과 반복 작업에 최적화되어 있다.

### 주요 스펙

| 항목 | 내용 |
|------|------|
| 파라미터 | 2B / 13B |
| 출력 해상도 | 최대 1216×704 (720p급) |
| 클립 길이 | 최대 2분 (장점) |
| 인터페이스 | ComfyUI |
| 특이사항 | 네이티브 오디오 생성 지원 |

### 장점

- 현존 로컬 모델 중 **최고 생성 속도** (Wan 대비 3~4배)
- 8GB VRAM으로도 동작 가능 (소비자 GPU 최저 요구사양)
- 최대 2분 길이 영상 생성 지원
- 오디오 동기화 기능 내장 (v2.3)
- ComfyUI 공식 워크플로우 제공

### 단점

- 비상업 라이선스 (상업 프로젝트에는 Wan 2.7 권장)
- 퀄리티는 Wan 2.7, HunyuanVideo 대비 낮음
- 고해상도(1080p) 생성 시 VRAM 32GB+ 필요

### 추천 대상

빠른 콘셉트 검증, 반복 시안 작업, 프로토타이핑이 주목적인 디자이너

---

## 3. HunyuanVideo — 시네마틱 퀄리티

**개발사**: Tencent  
**GitHub**: https://github.com/Tencent/HunyuanVideo  
**라이선스**: Tencent Hunyuan Community License  
**OS 지원**: Windows / macOS / Linux  
**최소 VRAM**: 8GB (Q8 양자화) / 24GB (권장)  
**진입장벽**: ★★★★☆ (높음)

### 개요

오픈소스 로컬 영상 모델 중 **시네마틱 퀄리티가 가장 뛰어난** 모델. 풍부한 색감, 진짜 보케(bokeh) 효과, 영화 같은 카메라 무브먼트가 특징이다. 13B 파라미터 트랜스포머 아키텍처로, Q8 양자화 시 24GB에서 안정적으로 구동된다. ComfyUI의 Kijai HunyuanVideoWrapper 노드가 가장 활발히 유지되고 있다.

### 주요 스펙

| 항목 | 내용 |
|------|------|
| 파라미터 | 13B |
| 출력 해상도 | 720p (720×1280 세로 / 1280×720 가로) |
| 클립 길이 | 5초 |
| 생성 시간 | RTX 4090 기준 ~5분 50초 |
| 인터페이스 | ComfyUI (Kijai 노드) |

### 장점

- 로컬 모델 중 최고 수준의 시네마틱 비주얼
- 인물 표정, 얼굴 표현이 가장 자연스러움
- 도시, 숲, 수면 등 복잡한 환경 렌더링 우수
- ComfyUI 커뮤니티 지원 활발

### 단점

- 생성 속도가 가장 느림 (Wan/LTX 대비 3~5배)
- 16GB+ VRAM 권장, 8GB에서는 품질 저하 심함
- 장시간(10초+) 영상 생성 한계

### 추천 대상

고퀄리티 프레젠테이션 영상, 광고 콘셉트, 클라이언트 납품물 제작 디자이너

---

## 4. CogVideoX — 롱폼 스토리텔링

**개발사**: Zhipu AI (智谱AI)  
**GitHub**: https://github.com/THUDM/CogVideo  
**라이선스**: Apache 2.0  
**OS 지원**: Windows / macOS / Linux  
**최소 VRAM**: 8GB (5B 버전)  
**진입장벽**: ★★★☆☆ (보통)

### 개요

텍스트 프롬프트 추종 정확도와 스토리 기반 롱폼 영상 생성에 특화된 모델. 6초 클립을 720x480으로 생성하며, 시네마틱 콘텐츠와 내러티브 중심 작업에 강하다. bfloat16 및 양자화를 지원해 8GB VRAM 환경에서도 효율적으로 동작한다.

### 주요 스펙

| 항목 | 내용 |
|------|------|
| 파라미터 | 5B |
| 출력 해상도 | 720×480 |
| 클립 길이 | 6초 |
| 인터페이스 | ComfyUI, diffusers |
| 라이선스 | Apache 2.0 |

### 장점

- 텍스트 프롬프트 의미 전달 정확도 높음
- Apache 2.0으로 상업 활용 자유
- 스토리·캐릭터 중심 영상에 강점
- 8GB VRAM에서 실용적 품질 구현
- 공식 파인튜닝 지원 (자체 스타일 학습 가능)

### 단점

- 해상도가 상대적으로 낮음 (720×480 고정)
- 모션 자연스러움 면에서 Wan/Hunyuan 대비 부족
- 업데이트 주기가 타 모델보다 느림

### 추천 대상

스토리보드, 교육 영상 콘텐츠, 내러티브 중심 영상 제작자

---

## 5. AnimateDiff — 저사양 입문용

**GitHub**: https://github.com/guoyww/AnimateDiff  
**라이선스**: Apache 2.0  
**OS 지원**: Windows / macOS / Linux  
**최소 VRAM**: 4GB  
**진입장벽**: ★★☆☆☆ (낮음)

### 개요

Stable Diffusion 이미지 생성에 **모션 모듈을 추가**하는 방식으로 동작하는 영상 생성 도구. CivitAI에 수천 개의 커스텀 모션 모듈이 공유되어 있으며, 예술적 표현과 애니메이션 스타일 영상에 강하다. VRAM 요구사항이 가장 낮아 미드레인지 GPU에서도 구동된다.

### 주요 스펙

| 항목 | 내용 |
|------|------|
| 기반 | Stable Diffusion 1.5 / SDXL |
| 최소 VRAM | 4GB |
| 클립 길이 | 2~4초 (짧음) |
| 인터페이스 | ComfyUI, A1111 |
| 특이사항 | CivitAI 모션 모듈 생태계 최대 |

### 장점

- 4GB VRAM에서도 구동 (가장 낮은 진입 하드웨어 요건)
- CivitAI 커스텀 모션 모듈 수천 개 활용 가능
- SD 이미지 모델과 완전 호환 (기존 SD 사용자 즉시 활용)
- 애니메이션·예술 스타일에 특화
- 커뮤니티 튜토리얼·사례 가장 풍부

### 단점

- 클립 길이가 2~4초로 매우 짧음
- 최신 모델(Wan/LTX) 대비 영상 품질 낮음
- 시간적 일관성(temporal coherence) 한계

### 추천 대상

SD 이미지 생성에 익숙한 디자이너 / 저사양 GPU 환경 / 애니메이션·루프 영상 제작

---

# 🎵 음악(Music) 생성 AI

---

## 1. ACE-Step 1.5 — 음악 생성 1순위

**개발사**: ACE Studio + StepFun  
**GitHub**: https://github.com/ace-step/ACE-Step-1.5  
**라이선스**: Apache 2.0  
**OS 지원**: Windows / macOS / Linux  
**최소 VRAM**: 4GB (기본) / 12GB (XL 버전)  
**진입장벽**: ★★☆☆☆ (낮음)

### 개요

2026년 1월 출시된 오픈소스 음악 생성의 **"Stable Diffusion 모멘트"**. RTX 3090에서 풀 곡 한 곡을 **10초 이내**에 생성하며, 4GB VRAM으로도 구동된다. 텍스트-투-뮤직은 물론 커버 생성, 리페인팅, 보컬-to-BGM 변환 등 상업 도구 수준의 기능을 무료·로컬에서 제공한다. Suno, Udio의 실질적 무료 대안으로 평가받는다.

### 주요 기능

- **텍스트-투-뮤직**: 장르, 분위기, 악기 스타일 태그로 제어
- **가사 포함 생성**: 50개 이상 언어 지원 (한국어 포함)
- **커버 생성**: 기존 곡의 스타일을 유지하며 재해석
- **보컬-to-BGM**: 보컬 트랙에서 배경음악 자동 생성
- **LoRA 학습**: 자신만의 음악 스타일 파인튜닝 가능
- **ComfyUI 통합**: 이미지/영상 파이프라인과 연동 가능

### 장점

- RTX 3090 기준 10초, A100 기준 2초 만에 풀 곡 생성
- 4GB VRAM으로 기본 사용 가능
- Mac (Apple Silicon), AMD ROCm, Intel, NVIDIA CUDA 전부 지원
- Apache 2.0으로 상업 프로젝트 무제한 활용
- Pinokio로 원클릭 설치 지원

### 단점

- 보컬 품질은 Suno v5, Udio 상용 서비스 대비 다소 낮음
- XL 버전(4B)은 12GB+ VRAM 필요
- 아직 발전 중인 프로젝트로 일부 버그 존재

### 추천 대상

배경음악 제작, 광고 사운드, 게임 OST, 유튜브·SNS 콘텐츠 음악이 필요한 디자이너/크리에이터

---

## 2. AudioCraft / MusicGen — 오픈소스 표준

**개발사**: Meta AI  
**GitHub**: https://github.com/facebookresearch/audiocraft  
**라이선스**: MIT (MusicGen), CC-BY-NC (일부 모델)  
**OS 지원**: Windows / macOS / Linux  
**최소 VRAM**: 8GB (Small) / 12GB (Large)  
**진입장벽**: ★★★☆☆ (보통 · Python 지식 필요)

### 개요

Meta가 개발한 오픈소스 오디오 생성 프레임워크. MusicGen(음악), AudioGen(효과음), EnCodec(오디오 압축)으로 구성된다. MIT 라이선스로 완전 무료이며, 음악 연구자와 기술적인 사용자에게 최대의 유연성을 제공한다. 텍스트 프롬프트 외 멜로디 컨디셔닝(허밍·멜로디 입력)도 지원한다.

### 구성 모델

| 모델 | 용도 | VRAM |
|------|------|------|
| MusicGen Small | 짧은 루프·배경음악 | 8GB |
| MusicGen Large (3.3B) | 고품질 음악 생성 | 12GB+ |
| AudioGen | 효과음, 환경음 | 8GB |
| EnCodec | 오디오 압축·복원 | CPU 가능 |

### 장점

- MIT 라이선스 — 상업·비상업 제한 없음
- 멜로디 컨디셔닝 지원 (허밍으로 스타일 유도)
- 효과음(AudioGen) + 음악(MusicGen) 통합 프레임워크
- 연구·파인튜닝 생태계 가장 성숙
- Hugging Face 인터페이스로 빠른 시작 가능

### 단점

- ACE-Step 대비 생성 속도 느림
- 가사 포함 보컬 생성 미지원 (기악 특화)
- Large 모델은 12GB+ VRAM 필요
- CLI/Python 인터페이스 위주 (GUI 불편)

### 추천 대상

기악 BGM 제작, 효과음 제작, 음악 AI 파인튜닝 연구, 기술적 제어가 필요한 개발자형 디자이너

---

## 3. Stable Audio Open — 사운드 디자인 특화

**개발사**: Stability AI  
**GitHub**: https://github.com/Stability-AI/stable-audio-tools  
**라이선스**: MIT  
**OS 지원**: Windows / macOS / Linux  
**최소 VRAM**: 8GB  
**진입장벽**: ★★★☆☆ (보통)

### 개요

Stability AI가 공개한 오디오 생성 모델. 상용 버전과 달리 오픈소스 버전(Stable Audio Open 1.0)은 **루프, 텍스처, 앰비언트, 효과음** 생성에 특화되어 있다. 보컬 없는 배경음악, 게임 효과음, 영화 사운드스케이프 제작에 강점을 보인다. MIT 라이선스로 상업적 활용에 완전 자유롭다.

### 장점

- 루프·텍스처·앰비언트 음악 품질 최고 수준
- MIT 라이선스 (상업 완전 자유)
- 게임 효과음, 영상 배경음 제작에 최적화
- Stable Diffusion 생태계와 철학 일치 (디자이너 친화)

### 단점

- 보컬/가사 포함 완성곡 생성 불가 (기악·효과음 전용)
- 상용 버전 대비 품질 격차 존재
- 클립 최대 길이 제한 있음

### 추천 대상

UI/UX 효과음, 게임 사운드 디자인, 영상 배경음, 앰비언트 트랙이 필요한 디자이너

---

# 🗣️ 음성(TTS / Voice) 생성 AI

---

## 1. Kokoro-82M — 음성 생성 1순위

**개발사**: Hexgrad  
**GitHub**: https://github.com/hexgrad/kokoro  
**라이선스**: Apache 2.0  
**OS 지원**: Windows / macOS / Linux  
**최소 VRAM**: CPU 구동 가능 (GPU 선택)  
**진입장벽**: ★★☆☆☆ (낮음)

### 개요

82M 파라미터라는 극소 모델 크기임에도 상용 TTS 서비스와 대등한 품질을 제공하는 경량 TTS 모델. GPU 없이 **CPU만으로도 구동**되며, GPU 사용 시 30초 음성을 1초 이내에 생성한다. Apache 2.0 라이선스로 상업 프로젝트에 무제한 활용 가능. 한국어를 포함한 다국어를 지원하며 50개 이상의 음성 팩을 제공한다.

### 주요 스펙

| 항목 | 내용 |
|------|------|
| 파라미터 | 82M (초경량) |
| 지원 언어 | 영어(미/영), 한국어, 일본어, 프랑스어, 중국어 등 |
| 생성 속도 | GPU 210× 실시간 / CPU 3~11× 실시간 |
| 음성 팩 | 50개 이상 |
| 라이선스 | Apache 2.0 |

### 장점

- CPU만으로 실시간 이상 속도 구현 — GPU 불필요
- 50개 이상 음성 프리셋 제공
- 장문 텍스트도 어색한 멈춤 없이 자연스럽게 합성
- Apache 2.0 상업 활용 무제한
- pip install 한 줄로 즉시 설치
- 접근성 도구, 영상 나레이션, 팟캐스트 제작에 실용적

### 단점

- 음성 클로닝(Voice Cloning) 기능 미지원
- 감정 표현·억양 제어가 XTTS v2 대비 단순
- 한국어 억양 자연스러움은 영어 대비 낮음

### 추천 대상

영상 나레이션, 접근성 도구, 프레젠테이션 음성, GPU 없이 TTS가 필요한 모든 디자이너

```python
# 설치 및 기본 사용 예시
pip install kokoro>=0.8 soundfile

from kokoro import KPipeline
import soundfile as sf

pipe = KPipeline(lang_code="a")  # 'a' = American English, 'k' = Korean
samples = pipe("안녕하세요, Kokoro TTS입니다.", voice="af_heart", speed=1.0)
for i, (gs, ps, audio) in enumerate(samples):
    sf.write(f"output_{i}.wav", audio, 24000)
```

---

## 2. Coqui XTTS v2 — 음성 클로닝 표준

**개발사**: Coqui AI (Mozilla TTS 계보)  
**GitHub**: https://github.com/coqui-ai/TTS  
**라이선스**: Coqui Public Model License (CPML) — 비상업 무료, 상업 별도 계약  
**OS 지원**: Windows / macOS / Linux  
**최소 VRAM**: CPU 구동 가능 (GPU 5~10배 속도 향상)  
**진입장벽**: ★★★☆☆ (보통)

### 개요

**음성 클로닝의 업계 표준** 오픈소스 모델. 단 6~10초의 레퍼런스 오디오 샘플만으로 임의의 목소리를 정밀하게 복제한다. 16개 언어를 지원하며 한국어도 포함된다. 467M 파라미터로 Kokoro 대비 크지만 음성 클로닝, 다국어 합성, 크로스-언어 합성(한국어 목소리로 영어 발화 등) 기능을 지원한다.

### 주요 스펙

| 항목 | 내용 |
|------|------|
| 파라미터 | 467M |
| 지원 언어 | 16개 (한국어, 영어, 일본어, 중국어 등) |
| 클로닝 샘플 | 6초 최소 / 20초 권장 |
| 크로스-언어 | 지원 (A의 목소리로 B 언어 발화) |
| 라이선스 | CPML (비상업 무료) |

### 장점

- 6초 샘플로 고품질 음성 클로닝
- 16개 언어 지원 + 크로스-언어 합성
- CPU 구동 가능 (GPU 없어도 사용 가능)
- 강의·팟캐스트·더빙 작업에 즉시 활용
- 커뮤니티 튜토리얼·사례 풍부

### 단점

- 상업 사용 시 CPML 별도 라이선스 계약 필요
- Kokoro 대비 무거움 (467M vs 82M)
- 음성 클로닝 오남용(딥페이크) 윤리 주의 필요
- 생성 속도 Kokoro 대비 느림

### 추천 대상

광고 더빙, 교육 콘텐츠 나레이션, 다국어 영상 제작, 특정 인물 목소리 스타일 모사가 필요한 디자이너

> ⚠️ **윤리 주의**: 동의 없이 타인의 목소리를 클로닝하는 것은 법적·윤리적 문제를 야기할 수 있습니다. 반드시 권한을 가진 목소리만 클로닝하세요.

---

# 📊 종합 비교표

## 영상 AI 한눈에 보는 비교표

| 툴 | Win | Mac | Linux | 최소 VRAM | 속도 | 퀄리티 | 라이선스 | 특화 |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|---|
| **Wan 2.7** | ✅ | ✅ | ✅ | 6GB | 보통 | ★★★★☆ | Apache 2.0 | 범용, 상업 |
| **LTX-Video 2.3** | ✅ | ✅ | ✅ | 8GB | **최고** | ★★★☆☆ | 비상업 무료 | 속도, 오디오 |
| **HunyuanVideo** | ✅ | ✅ | ✅ | 8GB* | 느림 | ★★★★★ | 커뮤니티 | 시네마틱 |
| **CogVideoX** | ✅ | ✅ | ✅ | 8GB | 보통 | ★★★☆☆ | Apache 2.0 | 스토리텔링 |
| **AnimateDiff** | ✅ | ✅ | ✅ | **4GB** | 보통 | ★★☆☆☆ | Apache 2.0 | 저사양, 루프 |

\* HunyuanVideo: 8GB 구동 가능하나 24GB 권장

## 음악 · 음성 AI 한눈에 보는 비교표

| 툴 | 분류 | Win | Mac | Linux | 최소 VRAM | 특화 | 라이선스 |
|---|---|:---:|:---:|:---:|:---:|---|:---:|
| **ACE-Step 1.5** | 음악 | ✅ | ✅ | ✅ | 4GB | 풀 곡+보컬, 초고속 | Apache 2.0 |
| **AudioCraft/MusicGen** | 음악 | ✅ | ✅ | ✅ | 8GB | 기악, 효과음 | MIT |
| **Stable Audio Open** | 음악 | ✅ | ✅ | ✅ | 8GB | 루프, 사운드 디자인 | MIT |
| **Kokoro-82M** | 음성(TTS) | ✅ | ✅ | ✅ | **CPU 가능** | 경량, 다국어 | Apache 2.0 |
| **Coqui XTTS v2** | 음성(TTS) | ✅ | ✅ | ✅ | CPU 가능 | 음성 클로닝, 16개 언어 | CPML |

---

## RTX 5090 환경 추천 조합

RTX 5090 (VRAM 24GB) 환경에서는 모든 모델이 여유 있게 구동되므로, 용도별로 아래 조합을 권장한다.

| 작업 | 추천 툴 | 인터페이스 |
|------|---------|-----------|
| 고품질 광고·프레젠테이션 영상 | HunyuanVideo | ComfyUI |
| 빠른 콘셉트 시안 영상 | LTX-Video 2.3 | ComfyUI |
| 소셜미디어·B-roll 영상 (상업) | Wan 2.7 | ComfyUI / Pinokio |
| 애니메이션·루프 클립 | AnimateDiff | ComfyUI |
| 배경음악·광고 BGM | ACE-Step 1.5 | ComfyUI / Pinokio |
| 게임 효과음·앰비언트 | Stable Audio Open | Python / Gradio |
| 영상 나레이션 (GPU 무관) | Kokoro-82M | Python / API |
| 다국어 더빙·음성 클로닝 | Coqui XTTS v2 | Python / Gradio |

---

## 추천 보조 도구

### Pinokio — 원클릭 AI 앱 런처

**사이트**: https://pinokio.computer  
**OS**: Windows / macOS / Linux  
**무료**: 완전 무료

Python, Node.js, 의존성 패키지, 모델 다운로드를 모두 자동 처리하는 원클릭 설치 런처. 위 목록의 대부분 툴을 버튼 한 번으로 설치·실행할 수 있어, 기술적 진입장벽을 크게 낮춰준다. Stability Matrix(이미지 AI용)와 함께 사용하면 전체 크리에이티브 AI 환경을 단일 런처로 관리할 수 있다.

---

## 참고 링크

### 영상 생성
- Wan 2.7: https://github.com/Wan-Video/Wan2.1
- LTX-Video: https://github.com/Lightricks/LTX-Video
- HunyuanVideo: https://github.com/Tencent/HunyuanVideo
- CogVideoX: https://github.com/THUDM/CogVideo
- AnimateDiff: https://github.com/guoyww/AnimateDiff

### 음악 생성
- ACE-Step 1.5: https://github.com/ace-step/ACE-Step-1.5
- AudioCraft (MusicGen): https://github.com/facebookresearch/audiocraft
- Stable Audio Open: https://github.com/Stability-AI/stable-audio-tools

### 음성(TTS) 생성
- Kokoro-82M: https://github.com/hexgrad/kokoro
- Coqui XTTS v2: https://github.com/coqui-ai/TTS

### 통합 인터페이스 및 도구
- ComfyUI: https://github.com/comfyanonymous/ComfyUI
- Pinokio (원클릭 런처): https://pinokio.computer
- Stability Matrix (이미지 AI 관리): https://github.com/LykosAI/StabilityMatrix
- Hugging Face (모델 다운로드): https://huggingface.co
- CivitAI (커스텀 모델): https://civitai.com

---

*작성일: 2026년 6월 | 기준: 디자인 실무자용 무료 로컬 영상·음악·음성 생성 AI*  
*관련 가이드: 로컬 이미지 생성 AI 추천 가이드 2026 (`local_image_ai_guide.md`) 참조*
