# 기축로 (GICHUKRO)

**AI와 인간을 위한 통합 프로젝트 지식 플랫폼.**

파일·버전, 태스크·로드맵, 의사결정과 작업 이력을 하나의 프로젝트 공간에 보관하고, AI 에이전트가 RAG/MCP로 이를 검색하고 다음 세션으로 작업을 인계하는 것을 목표로 합니다.

> Status: design / Phase P0. 실행 가능한 서비스는 아직 구현되지 않았습니다.

## 설계 문서

- [Architecture](docs/ARCHITECTURE.md): 시스템 구조, 데이터 모델, RAG, 보안 정책
- [MVP specification](docs/MVP_SPEC.md): 주요 사용자 시나리오, API 계약, UI, 테스트
- [Roadmap](docs/ROADMAP.md): P0–P4 단계, 수용 기준, 초기 작업

## 개발 방향

- **API**: FastAPI · PostgreSQL + pgvector
- **Frontend**: React · TypeScript
- **Storage**: 로컬 파일 저장을 시작으로 S3 호환 저장소 지원
- **Integration**: MCP · REST API · Git URL/커밋 연결
- **Architecture**: 모듈형 모놀리스 + 분리된 지식 처리 워커

초기 목표는 개인 서버에서 실행 가능한 self-hosted MVP이며, BIO 연동은 선택형 후속 확장입니다.

## Security

비밀키, 토큰, `.env`, 사용자 데이터와 빌드 결과물을 커밋하지 마세요. AI가 기록한 내용과 검증된 사실은 구분하고, 프로젝트 권한 검사를 검색 전 단계부터 적용합니다.

## License

라이선스 미지정. 공개 사용·재배포 권한은 별도로 부여하지 않습니다.
