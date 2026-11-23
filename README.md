> 📌 **원본 저장소**: [Minimin0/ALLOG-Backend](https://github.com/Minimin0/ALLOG-Backend) · 멋쟁이사자처럼 해커톤 팀 프로젝트를 포트폴리오용으로 가져온 저장소입니다.
>
> 🙋 **내 역할 (장하빈)**: 백엔드 · AI 인증 파이프라인 · 인프라
> - 인증 사진의 EXIF·GPS 메타데이터를 AI 제공자로 보내기 전에 제거하는 사진 정제 기능 (방향 정보만 유지)
> - AI 판정 처리기를 provider 스위치 뒤에 연결하고, AI가 한 번 거절하면 다시 찍도록 안내하는 재시도 흐름 구현
> - AI가 보류한 인증을 운영자가 검토·확정하는 리뷰 큐와 처리자 기록 기능
> - 승인된 인증에 대한 포인트 지급 연동
> - 그룹 가입 거절 사유 코드 테스트와 그룹 API 계약 수정
> - 가비아 클라우드 배포, 도메인·DNS(`api.allog-app.store`) 설정, 배포 자료 문서화

---

# ALLOG Backend

ALLOG API 및 백엔드 서버 애플리케이션 전용 저장소입니다.

Java 21, Spring Boot, Gradle 기반의 백엔드 애플리케이션입니다.

## Repository Links

- 공통 문서: https://github.com/Minimin0/ALLOG
- 프론트엔드: https://github.com/Minimin0/ALLOG-Frontend

## Scope

- 회원 및 인증
- 사용자 프로필
- 하트
- 그룹 챌린지
- 루틴 수행 및 인증
- AI 판정 연동
- 개인 랭킹
- 그룹 성과
- 리워드 포인트
- 알림
- 운영자 기능

## Future Documents

- ERD
- API 명세
- 시스템 아키텍처
- 인증 및 권한 정책
- 배포 문서

## Directory Structure

```text
.
├── .github/
│   ├── ISSUE_TEMPLATE/
│   └── pull_request_template.md
├── docker/
├── docs/
│   ├── api/
│   ├── architecture/
│   └── database/
├── src/
├── .env.example
├── .gitignore
├── CONTRIBUTING.md
└── README.md
```

## Local Development

Java 21이 필요합니다.

```bash
./gradlew test
./gradlew bootRun
```

## Environment Variables

- 실제 환경 변수는 `.env`에 작성하고 커밋하지 않습니다.
- 공유 가능한 예시 키만 `.env.example`에 유지합니다.
- `.env.example`의 값은 로컬 개발 예시이며 운영 환경에서 그대로 사용하면 안 됩니다.

## Branch Strategy

```text
main
└── develop
    ├── feature/*
    ├── fix/*
    ├── refactor/*
    ├── test/*
    └── docs/*
```

브랜치 예시:

- `feature/auth-api`
- `feature/ranking-api`
- `fix/token-expiration`
- `refactor/challenge-service`
- `docs/api-spec`

## Commit Convention

- `feat`: 새로운 기능
- `fix`: 버그 수정
- `docs`: 문서 수정
- `style`: 코드 포맷 변경
- `refactor`: 리팩터링
- `test`: 테스트 추가 또는 수정
- `chore`: 설정 및 기타 작업

## Pull Request

1. 기능별 브랜치에서 작업합니다.
2. `develop` 브랜치로 Pull Request를 생성합니다.
3. 자기 자신이 작성한 PR도 변경 내용을 직접 검토합니다.
4. 가능한 경우 최소 1명의 리뷰를 받은 후 병합합니다.
5. API 변경 사항은 PR 본문에 반드시 작성합니다.
6. 테스트하지 않은 기능을 테스트 완료로 표시하지 않습니다.
7. `main`에는 직접 푸시하지 않습니다.
8. 배포 가능한 버전만 `develop`에서 `main`으로 병합합니다.

## Production integration contract

Production API: https://api.allog-app.store. Android receives this endpoint only through EXPO_PUBLIC_API_BASE_URL. Authentication signing keys, database passwords, AI API keys, and local signing secrets are backend runtime secrets; none belong in Android bundles or source control.

Verification media follows Android -> nginx -> Spring -> Gabia private local filesystem. VerificationMediaStorage remains the abstraction and local filesystem is the production adapter. Group lifecycle, deadline, Heart balance, reward, and verification decisions remain backend authority; neither Android nor AI becomes a business-truth source.
