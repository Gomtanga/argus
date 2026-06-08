# Argus — 심층 웹 리서치 스킬 🔍

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Compatibility: Hermes Agent + OpenCode](https://img.shields.io/badge/Compatibility-Hermes%20Agent%20|%20OpenCode-blue)]()
[![Version](https://img.shields.io/badge/version-4.5-brightgreen)]()

**Argus**는 AI 에이전트를 위한 체계적인 심층 웹 리서치 스킬입니다. 단순한 단일 검색을 넘어 **4단계 검색 프레임워크(4-Phase Framework)** 로 다중 출처를 탐색하고, 근거 수준을 보수적으로 분류하여 신뢰할 수 있는 종합 정보를 제공합니다.

[Hermes Agent](https://hermes-agent.nousresearch.com) 및 [OpenCode](https://github.com/opencode-ai/opencode)에서 사용 가능합니다.

---

## 🔑 주요 특징

- **4단계 검색 프레임워크:** 광범위 탐색 → 타겟 조사 → 교차 검증 → 비교 분석
- **출처 투명성:** 모든 주장을 `[web:N]` 형태로 인용, 출처 투명성 보고서 제공
- **증거 보정:** 주요 주장을 Verified / Supported / Anecdotal / Unverified / Speculative로 분류
- **정량적 증거:** 직접 관련 있고 출처 검증 가능한 수치만 사용
- **강한 표현 가드레일:** “표준”, “프로덕션 검증”, “커뮤니티 컨센서스” 같은 표현을 근거 기준에 맞게 제한
- **3가지 Firecrawl 접근법:** CLI(권장) / Python SDK / Direct HTTP — 모든 환경에서 동작
- **오류 복원력:** 검색 도구 실패 시 4단계 Fallback 시스템
- **품질 평가:** 출처 계층(S/A/B/C/D) + Claim Coverage Matrix + E-E-A-T 기준

---

## 🚀 빠른 시작

```bash
# Hermes Agent
hermes skills install research:argus

# OpenCode
# SKILL.md와 references/ 디렉토리를 스킬 디렉토리에 복사
```

### 사전 요구사항

Argus는 웹 검색과 콘텐츠 추출을 위해 [Firecrawl](https://firecrawl.dev)을 사용합니다:

```bash
# 방법 1: CLI (권장)
npm install -g firecrawl-cli
firecrawl login --api-key fc-your-api-key

# 방법 2: Python SDK
pip install firecrawl-py
export FIRECRAWL_API_KEY="your-api-key"
```

---

## 📖 작동 방식

### v4.5 Evidence Calibration Edition

v4.5의 목표는 검색량을 늘리는 것이 아니라, 검색 결과를 더 정확한 자신감으로 분류하는 것입니다.

| 근거 수준 | 의미 |
|-----------|------|
| **Verified** | 공식/1차 출처가 정확한 주장을 확인 |
| **Supported** | 신뢰 가능한 2차 출처가 여럿 있으나 1차 확인은 없음 |
| **Anecdotal** | 커뮤니티, 포럼, 개인 경험 중심 |
| **Unverified** | 약한 출처 하나 또는 직접 확인 불가 |
| **Speculative** | 제한된 근거에서 나온 추론 |

숫자는 더 이상 할당량처럼 강제하지 않습니다. 수치가 핵심 주장과 직접 관련 있고 출처 위치/측정일/현재성까지 확인될 때만 사용합니다.

### 4단계 검색 프레임워크

```
Phase A: 광범위 탐색  ──→  Phase B: 타겟 조사  ──→  Phase C: 교차 검증  ──→  Phase D: 비교 분석
  (1-3회 검색)            (4-6회 검색)              (7-8회 검색)              (9-10+회 검색)
```

| 단계 | 목적 | 주요 활동 |
|------|------|-----------|
| **A** | 전체 개념 파악 | 핵심 개념 식별, 최신 동향, 주요 플레이어 파악 |
| **B** | 구체적 정보 수집 | 수치 데이터, 구현 세부사항, 일반적인 문제점 수집 |
| **C** | 모든 정보 검증 | 2+ 독립 출처 교차 검증, 충돌 정보 식별 및 표기 |
| **D** | 비교 및 종합 | 대안 비교, 정보 격차 식별, 향후 전망 |

### 응답 모드

| 모드 | 트리거 | 구조 |
|------|--------|------|
| **A: 구현** | "how to", "setup" | 빠른 시작 → 단계 → 설정 → 트러블슈팅 |
| **B: 아키텍처** | "how it works", "compare" | 직접 답변 → 구성요소 → 비교 → 심층 분석 |
| **C: 리서치** | "analyze", "recommend" | 요약 → 분석 → 대안 → 로드맵 |

---

## ⚙️ 오류 처리

검색 도구 실패 시 단계적으로 대응합니다:

| 단계 | 상태 | 최소 검색 수 |
|------|------|-------------|
| 1: 정상 | 모든 도구 작동 | 기본 |
| 2: 부분 | 일부 도구 실패 | 5회 |
| 3: 최소 | 대부분 실패 | 0회 (내장 지식) |
| 4: 불가 | 외부 접근 불가 | 0회 (사용자 안내) |

---

## 📂 저장소 구조

```
argus/
├── SKILL.md                  ← 메인 에이전트 스킬 파일 (라우터)
├── LICENSE                   ← MIT 라이선스
├── README.md                 ← 이 파일
└── references/
    ├── tool-setup.md         ← Firecrawl 설정 및 사용법
    ├── search-framework.md   ← 4단계 검색 프레임워크 전체
    ├── quality-standards.md  ← 품질 평가 및 증거 보정 기준
    └── firecrawl-python-sdk-quirks.md  ← SDK 주의사항 참고
```

---

## 📄 라이선스

이 프로젝트는 MIT 라이선스를 따릅니다 — 자세한 내용은 [LICENSE](LICENSE) 파일을 참고하세요.

---

**버전:** 4.5
