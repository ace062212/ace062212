# 박동균 (Donggyun Park)

**공공 대국민 AI 챗봇의 RAG 파이프라인을 만들고 운영합니다.**
검색이 아니라 *답이 맞는지*를 기준으로 시스템을 고칩니다.

`RAG` · `Elasticsearch` · `Spring Boot` · `LLM Serving` · `eGovFrame`

---

## 지금 하는 일

**AI RAG 챗봇 — 공공 대국민 상담 서비스** · 2026.04 ~ 현재

다국면(멀티 도메인) RAG 챗봇의 백엔드 파이프라인 전체를 설계·운영합니다.
SRT 예매, 한국사, 전자통관, 산재보상, 공무원연금 등 도메인별 지식베이스를 하나의 라우팅 계층 아래 묶고,
기관 확장이 인덱스 추가만으로 끝나도록 구조를 잡았습니다.

```
질문 + 대화맥락
   └→ ① 라우팅      Rule → 규칙 교차검증(margin) → 검색기반 top-1 → LLM 폴백
                     + 마진 게이트 · 세션 고착 · 비활성 도메인 차단
   └→ ② 질문 정규화  sLLM 11.5B — 오타 교정 + 생략된 문맥 복원
   └→ ③ 하이브리드 검색  Elasticsearch 8.12 (nori) — KNN + BM25, RRF k=60
   └→ ④ 리랭킹      BGE-Reranker-M3 → 상위 N건
   └→ ⑤ 프롬프트 조립  도메인별 시스템 프롬프트 + 페르소나 고정
   └→ ⑥ 생성        LLM 32B, SSE 스트리밍
```

**맡은 것**
- 라우팅 오분류 · 리랭커 컷오프 · 프롬프트 회귀를 지표로 잡고 반복 개선
- eGovFrame 3.9 (Spring 4.3) 레거시 포털에 SSE 스트리밍 챗 인터페이스 이식
- 지식베이스 관리도구 (Spring Boot + React/Vite 모노레포) 설계 및 배포
- Jenkins CI/CD — 빌드·원격 배포·운영 검증 루프

---

## 이 일에서 배운 것 중 남길 만한 것

**"환각"이라고 불린 것의 대부분은 환각이 아니었다.**

운영 평가를 돌렸더니 이런 숫자가 나왔습니다.

| 지표 | 결과 |
|---|---|
| 정답 문서 검색 성공 | **90%대** |
| 최종 답변 정합성 | **40%대** |

검색은 멀쩡한데 생성에서 무너지고 있었습니다.
그래서 오답으로 지적된 문구를 *실제 컨텍스트와 한 줄씩 대조*했더니 —
**4분의 3이 컨텍스트에 실재하는 문장**이었습니다. 실제 창작은 4분의 1뿐이었습니다.

같은 "오답"이지만 고칠 곳이 완전히 다릅니다.

| 실제 원인 | 고칠 레이어 |
|---|---|
| 무관 문서가 상위 후보에 섞여 합성됨 | 검색 · 리랭킹 |
| 절차 설명 중 UI 단계를 지어냄 | 프롬프트 |
| 지식 데이터에 구버전·신버전 금액이 **공존** | 데이터 — 프롬프트로는 절대 못 고침 |
| 리랭커가 KNN top-1 정답을 컷 (어휘 격차) | 임계값 · 인덱싱 |

한 번은 저도 잘못 셌습니다 (전부 창작으로 집계 → 실제는 4분의 1).
**컨텍스트와 대조하지 않고 센 결함 수치는 고칠 대상을 틀리게 만듭니다.**

> 📄 더 자세한 운영 기록: [rag-in-production](https://github.com/ace062212/rag-in-production)

---

## 기술 스택

**주력** Java · Spring Boot · Elasticsearch (nori, KNN, RRF) · RAG 파이프라인 설계 · Python
**LLM** 프롬프트 설계/회귀검증 · Reranker 튜닝 · SSE 스트리밍 · 임베딩 인덱싱 · RAGAS 평가
**인프라** Jenkins · Linux 배포 · Apache · CUBRID / MariaDB
**그 외** React · Vite · JSP/jQuery (레거시 연동) · R

---

## 이전 프로젝트

학부 시절 데이터 분석 · 컴퓨터 비전 작업물입니다.

| | |
|---|---|
| [**selim_chat**](https://github.com/ace062212/selim_chat) | 사내 규정 RAG 챗봇 · 하이브리드 검색 + FAISS · Next.js |
| [**DrowsinessDetection**](https://github.com/ace062212/DrowsinessDetection) | 라즈베리파이 졸음운전 감지 · OpenCV · *SW인재육성캠프 은상* |
| [**Daejeon-Transport-Demand**](https://github.com/ace062212/Daejeon-Public-Transport-Demand-Prediction) | 대전 대중교통 수요 예측 · AutoGluon |
| [**Korea2172**](https://github.com/ace062212/Korea2172) | 인구 통계 150년 예측 · R · ARIMA |
| [**Oil-Today**](https://github.com/ace062212/Oil-Today) | 위치 기반 실시간 유가 서비스 · Flask |

---

<sub>한남대학교 빅데이터응용학과 (2020–2026) · 빅데이터분석기사 · ADsP</sub>
<sub>📮 ace062212@gmail.com</sub>
