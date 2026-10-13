# GICHUKRO — MVP Specification v0.1

## 2. 주요 사용자 시나리오

**UC-01 — 프로젝트 시작:** 소유자가 프로젝트 생성 → 기본 접근 정책 설정 → 문서 업로드 → 파일 버전 저장.

**UC-02 — 작업 관리:** 태스크 생성 및 상태(TODO/IN_PROGRESS/BLOCKED/REVIEW/DONE) 변경 → 문서/결정/커밋 URL 연결.

**UC-03 — AI 검색:** MCP client가 프로젝트 범위 토큰으로 `search_knowledge` 호출 → 권한 적용 → 하이브리드 검색 → 파일 버전과 위치 포함한 snippets 반환.

**UC-04 — AI 핸드오프:** AI가 해당 태스크 작업 결과를 `submit_handoff`로 제출 → UNVERIFIED 기록 생성 → 권한 있는 사용자가 검토·승인 → 다음 AI가 `get_task_context`로 조회.

**UC-05 — 변경 반영:** 문서 새 버전 업로드 → 비동기 추출·청킹·임베딩 → 현재 버전 검색 가능 상태 전환 → 이전 버전은 감사용 보존하되 기본 검색에서 제외.

## 9. API 계약 v0.1

모든 요청은 `/api/v1` 기준, JSON 응답은 `request_id`, 표준화된 오류 `{code,message,details}` 사용. 목록 API는 cursor pagination. 수정 요청에는 가능한 한 idempotency key 또는 version precondition 지원.

| 메서드 | 경로 | 설명 |
|---|---|---|
| POST | `/auth/register` | 사용자 등록 (self-hosted 초기설정 정책에 따름) |
| POST | `/auth/login` | 로그인 및 세션 발급 |
| POST | `/auth/logout` | 세션 폐기 |
| GET/POST | `/projects` | 접근 가능한 프로젝트 목록/생성 |
| GET/PATCH | `/projects/{pid}` | 프로젝트 상세/수정 |
| GET/POST | `/projects/{pid}/members` | 멤버 조회/초대·추가 |
| GET/POST | `/projects/{pid}/files` | 파일 목록/업로드 |
| GET | `/projects/{pid}/files/{fid}/versions` | 버전 목록 |
| GET | `/projects/{pid}/files/{fid}/content?version=` | 다운로드/텍스트 |
| GET/POST | `/projects/{pid}/tasks` | 태스크 목록/생성 |
| GET/PATCH | `/projects/{pid}/tasks/{tid}` | 태스크 조회/상태수정 |
| POST | `/projects/{pid}/search` | 하이브리드 검색 |
| GET | `/projects/{pid}/index-jobs` | 색인 상태 |
| POST | `/projects/{pid}/tasks/{tid}/handoffs` | 핸드오프 제출 |
| GET | `/projects/{pid}/tasks/{tid}/context` | AI 작업 맥락 |
| POST | `/projects/{pid}/handoffs/{hid}/approve` | 핸드오프 승인 |
| POST | `/projects/{pid}/tokens` | 프로젝트 범위 토큰 생성 |
| POST | `/projects/{pid}/tokens/{token_id}/revoke` | 토큰 폐기 |
| GET | `/health/live` | 프로세스 생존 |
| GET | `/health/ready` | DB·스토리지 가용 상태 |

권한 없는 비공개 리소스 조회에 404를 반환해 존재 여부를 숨긴다. 잘못된 로그인 정보에는 통일된 응답 및 속도 제한.

## 11. UI/IA

**상위 화면**
1. `/dashboard` — 최근 프로젝트·최근 작업·색인 상태.
2. `/projects/:slug/overview` — 프로젝트 요약, README, 태스크 진행.
3. `/projects/:slug/files` — 디렉터리형 파일 브라우저/버전/미리보기.
4. `/projects/:slug/roadmap` — Kanban형 태스크, 상태/우선순위/기한.
5. `/projects/:slug/knowledge` — 검색 UI, 결과 출처, 인덱싱 현황.
6. `/projects/:slug/activity` — 작업 이벤트·핸드오프 검토.
7. `/projects/:slug/settings` — 멤버·토큰·공개범위·용량 정책.

**필수 UX:** 검색 결과를 누르면 정확한 파일 버전·문단으로 이동. 태스크에 관련 파일/결정/핸드오프가 연결됨. 인덱싱 지연과 실패를 사용자에게 노출. 확인 없는 삭제 불가.

## 14. 테스트 매트릭스

- **Unit:** RBAC 정책, path canonicalization, 파일 해시, 상태 전이, RRF, chunk offset, handoff state.
- **Integration:** DB migration, transaction rollback, blob/DB 정합, 워커 retry, token revoke, stale version exclusion.
- **Security:** IDOR, cross-project search, prompt injection as untrusted data, zip bomb/oversized upload, traversal, content-type spoof, cross-scope MCP call.
- **E2E:** 브라우저 프로젝트 생성·업로드·버전·로드맵·검색·승인; MCP context + handoff.
- **Benchmark:** 인덱스 1k/10k/100k chunks, 검색 p50/p95, retrieval Recall@5, worker throughput, 메모리 사용량.

릴리스 차단 조건: 다른 프로젝트 데이터 누출, 임의 파일 실행/경로 탈출, 손상된 파일 버전 기록, 감사 불가능한 자동 승인.

