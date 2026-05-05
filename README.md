# Argus - Deep Web Research Skill 👁️

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Compatibility: Hermes Agent + OpenCode](https://img.shields.io/badge/Compatibility-Hermes%20%2B%20OpenCode-blue.svg)]()
[![Version](https://img.shields.io/badge/version-4.2-brightgreen.svg)]()

**Argus**는 Perplexity 스타일의 심층 웹 리서치 스킬(Agent Skill)입니다. 단순한 단일 검색을 넘어, 다중 출처를 교차 검증하고 신뢰할 수 있는 종합적인 정보 수집과 분석을 자율적으로 수행합니다.

> **지원 환경**: Hermes Agent ✅ | OpenCode ✅

---

## 🌟 주요 특징 (Key Features)

- **체계적인 4단계 검색 프레임워크 (4-Phase Framework)**: 광범위한 탐색부터 타겟 조사, 교차 검증, 비교 분석까지 단계별 리서치 수행
- **엄격한 출처 검증 (Source Transparency)**: 모든 주요 주장에 대해 `[web:N]` 형태의 출처 표기 의무화 및 출처 투명성 보고서 제공
- **정량적 데이터 지향**: 모호한 표현("빠르다")을 배제하고 구체적인 수치("150ms latency")와 벤치마크 지향
- **Firecrawl SDK 연동**: `FirecrawlApp`(`search`, `scrape`, `map`)을 활용한 심층 데이터 추출
- **오류 복원력 (Graceful Degradation)**: API 제한이나 검색 실패 시 유연하게 대처하는 Fallback 시스템 탑재

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

- **Firecrawl SDK**: `pip install firecrawl-py`
- **API 키**: Firecrawl API 키 (`FIRECRAWL_API_KEY`) 환경 변수 등록
- **커스텀 엔드포인트** (선택): `FIRECRAWL_BASE_URL` 환경 변수 (예: `https://firecrawl.jiminbox.com`)

### Python 설정 예시
```python
from firecrawl import FirecrawlApp

app = FirecrawlApp(
    api_key="YOUR_API_KEY",
    api_url="https://firecrawl.jiminbox.com"  # 커스텀 엔드포인트
)
```

---

## 모델 호환성 (Model Compatibility)

Argus는 다음 모델에서 정상 작동이 검증되었습니다:
- **DeepSeek V4 Flash** ✅ (기본)
- **GLM-5-Turbo** ✅
- **GLM-4.7** 이상 ✅
- **Claude Sonnet/Opus 4.5** 이상 ✅
- **Kimi K2.6** ✅

---

## 트리거 (Triggers)

사용자가 다음과 같은 의도나 키워드를 사용할 때 Argus 스킬이 자동으로 개입합니다.
> "검색해줘", "찾아줘", "조사해줘", "비교해줘", "분석해줘", "알아봐줘", "웹 검색", "리서치"
> *(English triggers: "search", "find", "investigate", "compare", "analyze", "look up", "web search", "research")*

---

## 📖 작동 방식 (How it Works)

Argus는 질문의 복잡도(Complexity 1~5)에 따라 최소 1회에서 최대 14회 이상의 다중 검색을 수행하여 정보의 포화 상태(Saturation)에 도달할 때까지 조사합니다.

### 1. 다단계 리서치 프로세스
1. **Phase A: 광범위 탐색 (Broad Exploration)** - 핵심 개념 및 최신 동향 파악 (1~3회 검색)
2. **Phase B: 타겟 조사 (Targeted Investigation)** - 구현 사례, 성능 지표, 문제점 등 구체적 수치 수집 (4~6회 검색)
3. **Phase C: 교차 검증 (Verification)** - 2개 이상의 독립된 출처를 통한 팩트 체크 및 커뮤니티 반응 확인 (7~8회 검색)
4. **Phase D: 비교 분석 (Comparative)** - 대안 비교 및 향후 전망 분석 (9~10회 이상 검색)

### 2. 최적화된 응답 모드 (Response Modes)
사용자의 요청 성격에 따라 최적화된 포맷으로 답변을 구조화하여 제공합니다:
- **Mode A (구현/Implementation)**: "how to", "setup" ➔ 빠른 시작, 단계별 가이드, 트러블슈팅 중심
- **Mode B (아키텍처/Architecture)**: "how it works", "architecture" ➔ 컴포넌트 분석, 비교 표, 심층 기술 문서 중심
- **Mode C (리서치/Research)**: "analyze", "pros cons" ➔ 요약, 심층 분석, 대안, 투명성 보고서 중심

---

## ⚠️ 핵심 원칙 (Core Principles)

Argus 요원(Agent)은 다음 사항을 **절대 금지**합니다:
- ❌ 검증되지 않은 사실을 확정적으로 주장
- ❌ 단일 출처에만 의존한 중대한 의사결정/주장
- ❌ 출처(Reference)가 없는 수치 데이터 제시
- ❌ 오래된 정보를 최신 정보인 것처럼 제시

---

## 📊 출처 투명성 보고서 (Source Transparency)

모든 심층 리서치 결과(Mode B, C)에는 반드시 출처 투명성 보고서가 하단에 포함됩니다. 사용자는 어떤 수준(공식 문서, 전문가 분석, 커뮤니티)의 자료가 인용되었는지, 정보 간 충돌은 없었는지 명확히 확인할 수 있습니다.

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
```

---

## 📄 License

이 스킬은 [MIT License](LICENSE)의 적용을 받습니다.

---

## 📋 변경 이력 (Changelog)

| 버전 | 날짜 | 내용 |
|------|------|------|
| 4.2 | - | Graceful Degradation 대응 모델 추가 |
| 4.1 | - | Hermes Agent 호환성 추가 (Frontmatter, Firecrawl SDK API 정리) |
| 4.0 | - | 최초 공개 (OpenCode 전용) |

---

*버전: 4.2 (Hermes + OpenCode 호환)*
