# HoldemMates 프로젝트 컨텍스트

## 언어 지시 (⚠️ 최우선 — 항상 준수)
- **항상 한국어로 답변할 것.** 코드 주석/커밋 메시지가 아닌 모든 설명, 요약, 대화는 한국어로 작성
- 작업이 길어지거나(대규모 리팩터링, 여러 파일 순회, 긴 도구 호출 연속 등) 컨텍스트가 쌓여도 **중간에 영어로 전환되지 않도록** 매 응답마다 한국어 유지를 재확인할 것
- 사용자가 영어로 질문해도 답변은 한국어로 할 것 (사용자가 명시적으로 다른 언어를 요청하지 않는 한)

## 프로젝트 개요
- 친구들끼리 쓰는 포커(홀덤) 유틸리티 웹앱
- 배포 주소: holdemmates.com (GitHub Pages + Cloudflare DNS)
- GitHub 저장소: Zzang-Ki/Holdem
- 1인 개발, 저녁 시간 짧은 세션(45~60분)으로 반복 작업하는 방식

## 파일 구조
- `index.html` — 정산 계산기 (메인 페이지)
- `pushfold-chart.html` — Push/Fold 차트
- `CNAME` — Cloudflare 커스텀 도메인 설정 (holdemmates.com)
- 별도 프레임워크/빌드 과정 없이 **단일 HTML 파일** 구조 (핵심 기능들이 각각 독립된 HTML 파일)

## 기술 스택 & 배포
- GitHub Pages로 정적 호스팅, 도메인은 Cloudflare에서 구매 후 DNS 연결
- main 브랜치에 푸시하면 GitHub Pages에 자동 반영됨

## 주요 기능

### 1. 정산 계산기 (Settlement Calculator)
- 홀덤 정산 처리 핵심 기능, 단일 HTML 파일
- 바이인 스테퍼, 한국어 단위 표시, 정산(비용 분배) 로직
- html2canvas로 결과를 이미지로 공유
- 라이트 컬러 테마

### 2. 날짜별 방(Room) 구조
- 날짜를 키로 하는 방 아키텍처
- localStorage 캐싱 + 날짜 목록 랜딩 화면

### 3. Push/Fold 차트 (`pushfold-chart.html`)
- Nash 핸드 순서 전체 반영
- 한국어 UI 라벨
- 인원수 / 스택 사이즈 / 앤티 토글
- Push 모드 / Call 모드 각각 동적 힌트 텍스트

## Firebase 연동 (⚠️ 중요)
- Firestore 사용 (`index.html` 내 클라이언트 SDK, compat 버전)
- **소유자 전용 쓰기 권한** — `index.html`의 `HOST_EMAIL` 상수(iamrlgns@gmail.com)로 로그인한 계정만 쓰기 가능
- 그 외 사용자는 **읽기 전용**
- 실제 권한 보장은 클라이언트 코드가 아니라 **Firestore 보안 규칙**에서 이루어짐 — 규칙이 `request.auth.token.email == HOST_EMAIL` 등으로 실제 서버 측에서 막고 있는지 항상 확인
- `firebaseConfig`의 `apiKey` 등 클라이언트 웹 설정값은 원래 공개되어도 되는 값이라 `index.html`에 커밋되어 있음 (정상) — 절대 커밋하면 안 되는 건 **서비스 계정 JSON 키(Admin SDK)** 같은 서버 전용 시크릿이며, 이 프로젝트는 현재 그런 백엔드를 쓰지 않음. 추후 Cloud Functions/Admin SDK를 추가한다면 그 키는 반드시 `.gitignore` + 환경변수로 관리

## 디자인
- 로고: 사이트 컬러 팔레트에서 추출한 색상 기반 SVG 조합

## 검토 후 보류/폐기한 아이디어
- 근처 홀덤펍 실시간 디렉토리 기능 — 규제 리스크로 채택 안 함

## 작업 시 유의사항
- 기존 단일 HTML 파일 구조를 임의로 프레임워크화하지 말 것 (명시적 요청 없는 한)
- 새 기능 추가 시 기존 한국어 UI 라벨 스타일 유지
- 변경 후 Firebase 권한 구조(소유자 쓰기 / 읽기 전용)가 깨지지 않는지 확인
- 배포는 main 브랜치 푸시 = 즉시 반영이므로, 실험적 변경은 별도 브랜치 권장
