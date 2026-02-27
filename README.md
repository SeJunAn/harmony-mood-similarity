# Music-Similarity-Analyzer: A Deep Learning-based MIR System

Contributors : [안세준](https://github.com/SeJunAn), [박정원](https://github.com/rnrdus5642) <br>
Institution : [영상처리연구실 / 순천향대학교](https://ipl.sch.ac.kr)

본 프로젝트는 딥러닝을 활용하여 두 곡 간의 음악적 유사도를 정밀하게 측정하기 위한 **음악 정보 검색(MIR, Music Information Retrieval)** 파이프라인입니다. 졸업 작품 연구의 일환으로 개발되었습니다.

> **Abstract**
> 본 시스템은 오디오 데이터를 두 가지 독립적인 특징(Feature)으로 나누어 병렬 분석하는 듀얼 트랙(Dual-track) 구조를 제안합니다. **Track A**에서는 CQT(Constant-Q Transform) 및 크로마그램(Chromagram)을 활용해 곡의 화성(Harmony) 전개를 분석하며, **Track B**에서는 곡의 전반적인 분위기(Mood)를 추출합니다. 대조 학습(Contrastive Learning) 기법을 통해 각 트랙의 특징 표현(Representation)을 고도화하고, 이를 융합하여 최종적인 음원 유사도를 객관적으로 도출하는 것을 목표로 합니다.

---

## 1. Data & Preprocessing

모델 훈련 및 검증에 사용되는 오디오 데이터 전처리와 특징 추출 파이프라인입니다. 저작권 문제로 원본 오디오 파일은 레포지토리에 포함되지 않으며, 제공되는 데이터 로더 스크립트를 통해 로컬의 `.wav` 또는 `.mp3` 파일에서 특징을 동적으로 추출합니다.

* **Dataset**: [FMA (Free Music Archive) Dataset](https://github.com/mdeff/fma?tab=readme-ov-file)을 기본으로 활용합니다.
* **Harmony Features (Track A)**: 원본 오디오에 CQT(Constant-Q Transform)를 적용한 후, 화성 진행을 직관적으로 나타내는 크로마그램(Chromagram)으로 변환하여 모델의 입력으로 사용합니다.
* **Mood Features (Track B)**: 곡의 전반적인 분위기와 감정적 질감을 추출하기 위해 Mel-spectrogram 등의 음향 특징을 전처리하여 사용합니다.

---

## 2. Directory Structure

본 단일 레포지토리(Monorepo)는 전처리, 모델 훈련, 평가를 위한 모듈로 구성되어 있습니다.

```text
├── track_a_harmony/      # 대조 학습(Contrastive Learning) 기반 화성 유사도 측정 인코더
├── track_b_mood/         # 음악의 무드(Mood) 특징 추출 및 학습 신경망 모델
├── core/                 # 두 트랙(A, B)의 임베딩을 결합하여 최종 유사도(Similarity)를 계산하는 메인 파이프라인
├── utils/                
│   └── audio_prep.py     # CQT 변환, 크로마그램 추출 등 오디오 전처리 헬퍼 함수
├── usage.ipynb           # 모델 로드 및 임의의 두 곡 간 유사도 측정 데모 노트북
├── evaluation.ipynb      # 평가 지표 산출 및 모델 성능 시각화 분석 노트북
└── requirements.txt      # 프로젝트 의존성 패키지 목록

---

## Usage

**1. 환경 세팅 (Install dependencies)**

```bash
# 가상환경 생성 및 활성화
python -m venv venv
source venv/bin/activate  # Mac/Linux 환경

# 의존성 패키지 설치
pip install --upgrade pip setuptools wheel
pip install -r requirements.txt
