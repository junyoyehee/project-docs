# 프로젝트 관리 가이드

## 프로젝트 구조

```
D:\aiworks\githubs\
├── project-docs/      # 프로젝트 관리 문서 (이 저장소)
├── pole/              # 메인 프로젝트 (개발팀 인트라넷 플랫폼)
├── pole-template/     # 스타터 템플릿 (pole 기반 구조 재사용)
└── jack/              # 신규 프로젝트 (pole-template 기반)
```

## 프로젝트 관계도

```
pole (원본)
 └─► pole-template (구조 추출)
      └─► jack (템플릿으로 생성)
      └─► [향후 신규 프로젝트...]
```

## 각 프로젝트 개요

### pole (메인)
- **용도**: 개발팀 인트라넷 플랫폼
- **주요 기능**: Figma Hub, Design Review, Figma Explorer, Handoff
- **외부 연동**: Figma REST API, Anthropic Claude API, Supabase(PostgreSQL)
- **GitHub**: github.com/junyoyehee/pole

### pole-template (템플릿)
- **용도**: pole의 기반 구조를 재사용하기 위한 스타터 템플릿
- **포함 항목**: Sidebar, Header, RichEditor, 디자인 토큰, Prisma 싱글턴, API 헬퍼
- **GitHub**: github.com/junyoyehee/pole-template

### jack (신규)
- **용도**: pole-template 기반으로 생성된 새 프로젝트
- **상태**: 초기 구성 완료
- **GitHub**: github.com/junyoyehee/jack

### project-docs (관리)
- **용도**: 프로젝트 관리 문서 저장소
- **포함 항목**: project-manager.md (이 파일)
- **GitHub**: github.com/junyoyehee/project-docs

## 공통 기술 스택

| 항목 | 기술 |
|------|------|
| 프레임워크 | Next.js 16 (App Router) + React 19 |
| 언어 | TypeScript |
| UI 라이브러리 | Mantine v8 (다크 테마, 바이올렛 액센트) |
| 스타일링 | Tailwind CSS v4 + CSS Modules |
| ORM / DB | Prisma 7 + PostgreSQL |
| 아이콘 | @tabler/icons-react |
| 상태 관리 | React hooks (외부 라이브러리 없음) |

## 공통 컨벤션

- **API 응답 형식**: `{ success, data/error }` (src/lib/api.ts)
- **DB 컬럼명**: snake_case (`@map` 사용)
- **컴포넌트 스타일**: CSS Modules (`*.module.css`)
- **CSS 변수**: `globals.css`에 정의
- **환경 변수**: `.env.local`에 관리 (DATABASE_URL 등)

## 신규 프로젝트 생성 절차

1. 템플릿 복사: `cp -r pole-template <새 프로젝트명>`
2. `package.json`의 `name` 수정
3. Sidebar 메뉴, 대시보드 카드 커스터마이징
4. 의존성 설치: `npm install`
5. DB 설정: `prisma generate`
6. 개발 서버 실행: `npm run dev`
7. GitHub 저장소 생성 및 remote 연결

## Git 관리 규칙

- **브랜치**: `master` (기본)
- **커밋 메시지**: 한국어, 접두사 사용 (`docs:`, `feat:`, `fix:` 등)
- **CLAUDE.md**: 각 프로젝트 루트에 유지 (Claude Code 프로젝트 설정)

## Claude Code Status Line

모든 프로젝트에 공통 적용되는 상태 표시줄 설정.

### 설정 파일
- **설정**: `~/.claude/settings.json` (글로벌 적용)
- **스크립트**: `~/.claude/statusline.sh`

### 표시 내용
```
[프로젝트명] 모델명 | ######---- 60%
```

- **프로젝트명**: 현재 작업 디렉토리명 (시안 색상)
- **모델명**: 사용 중인 Claude 모델 (마젠타 색상)
- **컨텍스트 바**: 컨텍스트 윈도우 사용률 (색상 변화)
  - 50% 미만: 초록 (여유)
  - 50~80%: 노랑 (주의)
  - 80% 이상: 빨강 (위험)

### 참고 사항
- jq 미설치 환경 대응: grep/sed로 JSON 파싱
- `settings.json`의 `statusLine` 항목이 글로벌 설정이므로 별도 프로젝트별 설정 불필요

## 템플릿 업데이트 반영

pole에서 공통 구조가 변경된 경우:
1. pole-template에 해당 변경 반영
2. 기존 프로젝트(jack 등)에는 필요 시 수동 반영
