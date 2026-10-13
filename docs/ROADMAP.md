# GICHUKRO — Roadmap v0.1

## 13. 개발 단계 및 수용 기준

### P0 — Foundation
- [ ] monorepo, lint/format/typecheck/test, Docker Compose, DB migrations.
- [ ] 사용자 인증 및 project RBAC; 허용/거부 테스트.
- [ ] 보안 토큰/에러 응답/요청 trace id.

**Gate:** 초기 환경에서 README 지시만으로 API·DB 실행; 프로젝트간 권한 누출 테스트 모두 통과.

### P1 — Files + Work
- [ ] 파일 업로드·다운로드·버전·복원·quota.
- [ ] Markdown viewer/editor, 프로젝트 overview.
- [ ] 태스크 CRUD, 상태, 연결 링크, 활동 로그.

**Gate:** 파일 수정 전후 버전 모두 조회 가능; 임의 경로 쓰기 차단; 권한별 e2e 통과.

### P2 — Knowledge
- [ ] extraction & async index job; 실패 재시도.
- [ ] fulltext + vector 검색/RRF; source attribution.
- [ ] 변경 시 재색인, 삭제·비공개 변경 시 검색 제외.

**Gate:** 30문항 benchmark 통과, ACL 누출 0, 인덱스 장애 후 복구.

### P3 — Agents
- [ ] MCP read tools, handoff write tool.
- [ ] AI 결과 UNVERIFIED 저장 + 승인 workflow.
- [ ] 태스크 context package (토큰 상한/출처/현재 상태).

**Gate:** Codex → handoff → Claude Code 이어받기 시나리오 수동 테스트와 자동 계약 테스트 통과.

### P4 — Release hardening
- [ ] 백업/복원 검증, 파일 검사 정책, rate-limit, 감사 이벤트, 문서화.
- [ ] UI 통합테스트/실패 상태 UX, 버전 업그레이드 migration 시험.

**Gate:** 빈 환경 복원, 샘플 프로젝트 생성→파일→검색→handoff→승인→재조회가 완료.

## 18. 권장 첫 개발 작업

1. `docs/ARCHITECTURE.md`, `docs/ADR/001-007`를 현재 문서로 분할.
2. 프로젝트 skeleton/API migrations + compose 준비.
3. identity/projects/files schema와 permission tests 우선.
4. 태스크 및 감사 로그 이후 지식 인덱싱을 추가.
5. 마지막으로 MCP bridge 및 핸드오프 전체 흐름을 연결.

