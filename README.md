# Argus - Deep Web Research Skill 👁️

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Compatibility: Hermes Agent + OpenCode](https://img.shields.io/badge/Compatibility-Hermes%20%2B%20OpenCode-blue.svg)]()
[![Version](https://img.shields.io/badge/version-5.2-brightgreen.svg)]()

**Argus**는 Perplexity 스타일의 심층 웹 리서치 스킬(Agent Skill)입니다. 단순한 단일 검색을 넘어, 다중 출처를 교차 검증하고 신뢰할 수 있는 종합적인 정보 수집과 분석을 자율적으로 수행합니다.

> **지원 환경**: Hermes Agent ✅ | OpenCode ✅

---

## 🌟 주요 특징 (Key Features)

- **체계적인 4단계 검색 프레임워크 (4-Phase Framework)**: 광범위한 탐색(Phase A) → 타겟 조사(Phase B) → 교차 검증(Phase C) → 비교 분석(Phase D)
- **엄격한 출처 검증 (Source Transparency)**: 모든 주요 주장에 대해 `[web:N]` 형태의 출처 표기 의무화 및 출처 투명성 보고서 제공
- **정량적 데이터 지향**: 모호한 표현을 배제하고 구체적인 수치와 벤치마크 지향
- **Firecrawl 3-way 연동**: CLI / Python SDK / Direct HTTP (`urllib`) — 환경 제약 없이 사용 가능
- **최신 Firecrawl v3.x 포맷 지원**: Question Format (100x 적은 토큰), Highlights Format (그대로 인용), Lockdown Mode (보안 스크래핑)
- **Firecrawl web-agent 통합**: `firecrawl create agent`로 플랜-액트 루프 + 병렬 서브 에이전트 스캐폴딩
- **SOTA 리서치 참조 통합**: LiteResearcher (RL 훈련), FS-Researcher (파일시스템 이중 에이전트), WebSeer (자기반성 RL) 등 최신 연구 반영
- **오류 복원력 (Graceful Degradation)**: API 제한이나 검색 실패 시 4단계 Fallback 시스템
- **SourceBench 8-Metric 평가 프레임워크**: 출처 품질을 콘텐츠 품질 + 메타 속성 8개 지표로 평가
- **RACE 평가 프레임워크 + ECC/CA 인용 정확도**: 최종 리서치 보고서 품질 평가

---

## 🚀 설치 방법

### Hermes Agent

```bash
# Hermes 스킬 저장소에서 설치
hermes skills install research:argus

# 또는 직접 SKILL.md URL로 설치
hermes skills install https://gitlab.jiminbox.com/ParkGeonYoung/argus/-/raw/main/SKILL.md

# 설치 확인
hermes skills list | grep argus
```

### OpenCode
SKILL.md 파일을 OpenCode 스킬 디렉토리에 복사합니다.

---

## 사전 요구사항 (Prerequisites)

Argus는 3가지 방법으로 Firecrawl에 접근할 수 있습니다 (우선순위 순):

### 방법 1: Firecrawl CLI (권장)
```bash
npm install -g firecrawl-cli
firecrawl login --api-key fc-YOUR-API-KEY
```

### 방법 2: Python SDK (Fallback)
```bash
pip install firecrawl-py
export FIRECRAWL_API_KEY="your-api-key"
export FIRECRAWL_BASE_URL="https://firecrawl.jiminbox.com"
```

### 방법 3: Direct HTTP (No Dependencies)
```python
# urllib + json만으로 모든 Firecrawl API 호출 가능
# pip/npm 필요 없음!
```

> 💡 **커스텀 엔드포인트**: `FIRECRAWL_BASE_URL=https://firecrawl.jiminbox.com` 환경 변수 설정

---

## 모델 호환성 (Model Compatibility)

Argus는 다음 모델에서 정상 작동이 검증되었습니다:
- **DeepSeek V4 Flash** ✅ (기본)
- **GLM-5-Turbo** ✅
- **GLM-4.7** 이상 ✅
- **Claude Sonnet/Opus 4.5** 이상 ✅
- **Kimi K2.6** ✅
- **GPT-5** ✅ (Deep Research Bench 최고 점수, SourceBench 89.1점)

---

## 트리거 (Triggers)

사용자가 다음과 같은 의도나 키워드를 사용할 때 Argus 스킬이 자동으로 개입합니다.
> "검색해줘", "찾아줘", "조사해줘", "비교해줘", "분석해줘", "알아봐줘", "웹 검색", "리서치"
> *(English triggers: "search", "find", "investigate", "compare", "analyze", "look up", "web search", "research")*

---

## 📖 작동 방식 (How it Works)

Argus는 질문의 복잡도(Complexity 1~5)에 따라 1회에서 14회 이상의 다중 검색을 수행하여 정보의 포화 상태(Saturation)에 도달할 때까지 조사합니다.

### 1. 다단계 리서치 프로세스

```
Phase A: 광범위 탐색 ────→ Phase B: 타겟 조사 ────→ Phase C: 교차 검증 ────→ Phase D: 비교 분석
  (1~3회 검색)              (4~6회 검색)              (7~8회 검색)              (9~10+회 검색)
```

| 단계 | 목적 | 핵심 활동 |
|------|------|-----------|
| **A** | 핵심 개념 + 최신 동향 파악 | 3-5개 핵심 개념 식별, 채택율/통계 확보 |
| **B** | 구체적 수치 + 문제점 수집 | 5+ 정량 지표, 3+ 공통 문제점 문서화 |
| **C** | 2+ 독립 출처 교차 검증 | 다중 홉 증거 추적, 출처 충돌 식별 및 해결 |
| **D** | 대안 비교 + 향후 전망 | 3+ 대안 비교, 미래 로드맵 파악 |

> **Plan → Execute → Verify → Replan** 사이클: Phase C 검증 후 정보 격차 발견 시 Phase B로 복귀

### 2. 최적화된 응답 모드 (Response Modes)

| 모드 | 트리거 | 구조 |
|------|--------|------|
| **A (구현)** | "how to", "setup" | 빠른 시작 → 단계 → 설정 → 트러블슈팅 |
| **B (아키텍처)** | "how it works", "architecture" | 직접 답변 → 컴포넌트 → 비교표 → 심층 |
| **C (리서치)** | "analyze", "pros cons" | 요약 → 분석 → 대안 → 로드맵 + 투명성 보고서 |

### 3. 검색 종료 조건

**중단 조건 (ALL 충족):**
- ✅ 핵심 질문에 대한 명확한 답변 확보
- ✅ 주요 주장 2+ 독립 출처 검증 완료
- ✅ 반론 및 한계점 정보 확보
- ✅ 연속 2회 검색에서 새로운 정보 없음

**계속 조건 (ANY 충족):**
- ❌ 핵심 수치 데이터 부족
- ❌ 단일 관점만 수집됨
- ❌ 최신 정보 미확보
- ❌ 명백한 정보 격차 존재

---

## 🔬 검증 및 품질 평가

### 출처 투명성 보고서 (Source Transparency Report)

모든 심층 리서치 결과(Mode B, C)에는 반드시 출처 투명성 보고서가 하단에 포함됩니다:

```markdown
### 📚 Source Transparency Report

#### Sources: [X] total
- Tier 1 (Official/Academic): [count]
- Tier 2 (Expert Analysis): [count]
- Tier 3 (Community): [count]

#### Key Sources:
| Ref | Source | Type | Why Trusted |
|-----|--------|------|-------------|
| [web:1] | [Name] | Official Doc | Primary source |

#### Conflicting Information:
| Topic | Conflict | Resolution |
|-------|----------|------------|

#### Information Gaps:
- [What couldn't be verified]
```

### SourceBench 8-Metric (출처 품질 평가)

| 차원 | 지표 | 설명 |
|------|------|------|
| **콘텐츠 품질** | Relevance (CR) | 사용자 니즈 직접 해결? |
| | Factual Accuracy (FA) | 검증 가능한 주장? 1차 출처 우선? |
| | Objectivity (NE) | 중립적이고 임상적인 어조? |
| **메타 속성** | Freshness (FR) | 적시성? 구식 데이터 패널티 |
| | Author Accountability (AA) | 명명된 저자 + 검증 가능한 자격? |
| | Ownership (OA) | 자금/위치 투명성? |
| | Domain Authority (DA) | 공인 기관(.gov, .edu) 또는 인정 브랜드? |
| | Layout Clarity (LC) | SEO 농장 아님? 소비 용이성 |

### RACE 평가 프레임워크 (리서치 보고서 품질)

| 차원 | 목표 |
|------|------|
| **R**elevance (관련성) | 모든 주장이 원질문 직접 해결? |
| **A**ccuracy (정확성) | 모든 사실이 정확하고 출처 있음? |
| **C**ompleteness (완전성) | 모든 핵심 측면 커버됨? 정보 격차 인정? |
| **E**vidence (증거) | 증거가 충분하고 다양하며 추적 가능? |

**목표: 전 항목 4/5 이상**

### 인용 정확도 평가

| 지표 | 설명 |
|------|------|
| **ECC (Effective Citation Count)** | 관련성 있고 올바르게 귀속된 인용 수 |
| **CA (Citation Accuracy)** | ECC / 전체 인용 수 |

---

## 🔧 Firecrawl CLI 명령어 요약

| 명령어 | 용도 |
|--------|------|
| `firecrawl search <query>` | 웹 검색 |
| `firecrawl scrape <url>` | 페이지 콘텐츠 추출 |
| `firecrawl parse <file>` | PDF/DOCX/XLSX → Markdown/JSON 변환 |
| `firecrawl crawl <url>` | 전체 사이트 크롤링 |
| `firecrawl map <url>` | 사이트 구조 매핑 |
| `firecrawl extract <url> --json-schema` | 구조화된 JSON 추출 |
| `firecrawl agent <prompt>` | AI 에이전트 구조화 리서치 |
| `firecrawl interact <prompt>` | 페이지와 상호작용 (클릭, 폼) |
| `firecrawl create agent <name>` | 리서치 에이전트 스캐폴딩 (플랜-액트 루프 + 병렬 서브 에이전트) |
| `firecrawl credit-usage` | 남은 크레딧 확인 |
| `firecrawl --status` | 버전/인증/동시성/크레딧 상태 확인 |

### 스크래핑 포맷 옵션 (v3.x)

```bash
# 일반 마크다운
firecrawl scrape https://example.com

# Question Format — 그라운드된 답변 (100x 적은 토큰)
firecrawl scrape https://example.com --format question --question "핵심 아이디어는?"

# Highlights Format — 그대로 인용 (환각 0%)
firecrawl scrape https://example.com --format highlights --query "pricing plans"

# Lockdown Mode — 캐시 전용, 아웃바운드 요청 차단
firecrawl scrape https://example.com --lockdown
```

---

## 📚 최신 연구 참조 (SOTA References)

Argus는 최신 딥 리서치 연구 결과를 통합하고 있습니다:

| 연구 | 발행 | 핵심 기여 |
|------|------|-----------|
| **LiteResearcher** (Li et al.) | Apr 2026 | RL 기반 딥 리서치 에이전트, 4B 모델이 Claude-4.5-Sonnet 매칭 |
| **FS-Researcher** (Zhu et al.) | Feb 2026 | 파일시스템 기반 이중 에이전트 아키텍처 (컨텍스트 윈도우 초과 대응) |
| **WebSeer** (He et al.) | Oct 2025 | 자기반성 RL로 더 깊은 검색 훈련 |
| **MiroEval** (Ye et al.) | Mar 2026 | 프로세스+결과 멀티모달 평가, 92% 정밀도 |
| **Detecting Reference Hallucinations** (Rao et al.) | Apr 2026 | 3-13% URL 환각률, urlhealth 오픈소스 검증 도구 |
| **SourceBench** (Jin et al.) | Feb 2026 | 8개 지표 출처 품질 평가 프레임워크 |
| **Deep Research Bench** (Du et al.) | 2025 | RACE 평가 + ECC/CA 인용 정확도 |
| **Total Recall QA** (Rafiee et al.) | 2026 | 다단계 정보 탐색 벤치마크 |
| **DR-Arena** | Jan 2026 | 인간 선호도 정렬 자동 평가 |
| **AI Scientist** (Sakana AI / Nature) | 2026 | 종단간 AI 연구 자동화 검증 |

---

## ⚠️ 핵심 금지 원칙 (Absolute Prohibitions)

Argus 요원(Agent)은 다음 사항을 **절대 금지**합니다:
- ❌ 검증되지 않은 사실을 확정적으로 주장
- ❌ 단일 출처에만 의존한 중대한 주장
- ❌ 출처(Reference)가 없는 수치 데이터 제시
- ❌ 오래된 정보를 최신 정보인 것처럼 제시
- ❌ 출처 URL을 환각(hallucination)하여 제시 — urlhealth로 검증 필수

---

## ⚠️ 오류 대응 (Error Handling)

| 에러 유형 | 1차 대응 | Fallback |
|-----------|----------|----------|
| 검색 결과 없음 | 쿼리 단순화 → 언어 전환 (EN/KR) | 정보 격차 인정 |
| 도구 실패 | 재시도 → `firecrawl --status` 진단 | 브라우저 도구로 전환 |
| Rate Limit | `firecrawl credit-usage` 확인 | 검색 범위 축소 (최소 5회) |
| 인증 실패 | `firecrawl view-config` → 재로그인 | 환경 변수 설정 |
| SDK 미설치 | Method 3 (urllib) 사용 | 브라우저 도구 Fallback |
| 문서 파싱 실패 | `firecrawl parse --format markdown` 시도 | `firecrawl scrape` Fallback |
| 추출 스키마 불일치 | JSON 스키마 정제 | 일반 scrape Fallback |

---

## 📄 License

이 스킬은 [MIT License](LICENSE)의 적용을 받습니다.

---

## 📋 변경 이력 (Changelog)

| 버전 | 날짜 | 내용 |
|------|------|------|
| 5.2 | 2026-05 | FS-Researcher, MiroEval, urlhealth 인용 검증, /agent 엔드포인트 문서 추가 |
| 5.1 | 2026-05 | LiteResearcher (RL 훈련), WebSeer (자기반성 RL) SOTA 참조 추가 |
| 5.0 | 2026-05 | SourceBench 8-Metric 프레임워크 추가 |
| 4.9 | 2026-04 | Firecrawl web-agent, Lockdown Mode, Browser Sandbox 추가 |
| 4.8 | 2026-04 | Highlights/Extract/Ask 엔드포인트, Search API, 위치 타겟팅, Total Recall QA |
| 4.7 | 2026-04 | Question Format (100x 적은 토큰), Method 3 (urllib), ECC/CA 인용 정확도 |
| 4.6 | 2026-04 | Firecrawl Python SDK 주의사항 레퍼런스 문서 추가 |
| 4.5 | 2026-04 | `firecrawl parse` 명령어, RACE 평가 프레임워크 추가 |
| 4.4 | 2026-04 | Firecrawl CLI v2 (firecrawl-cli) 전환, E-E-A-T 기준 추가, FlowSearcher 방법론 |
| 4.3 | 2026-04 | MCP → Firecrawl CLI 주 도구 전환 |
| 4.2 | 2025 | Graceful Degradation 대응 모델 추가 |
| 4.1 | 2025 | Hermes Agent 호환성 추가 |
| 4.0 | 2025 | 최초 공개 (OpenCode 전용) |

---

*버전: 5.2 (Hermes + OpenCode 호환)*
