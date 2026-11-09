# 모바일 공통 기본기 (입력 확대 방지·글자 크기·터치 영역)
상태: 초안

## 목적 (한 줄)
모든 페이지에서 입력칸 탭 시 iOS 자동 확대를 없애고, 너무 작은 글씨·터치 영역을 모바일 최소 기준으로 끌어올린다.

## 사용자 시나리오
1. 아이폰 사용자가 정산 화면에서 남은 금액 칸을 탭하면 → 화면이 확대되지 않고 그대로 입력된다
2. 딜 계산기에서 이름/칩 칸을 탭해도 → 화면이 확대되지 않는다
3. 사용자가 세션 접기 버튼·마스터 로그인 링크를 탭하면 → 한 번에 정확히 눌린다 (빗나가지 않는다)
4. 사용자가 핀치 줌을 하면 → 모든 페이지에서 확대가 된다 (접근성 유지)

## 요구사항
- **입력 확대 방지**: 4개 페이지의 모든 `input`, `select`, `textarea`의 모바일 글자 크기를 16px 이상으로 한다
  - 현재: `index.html` 14~15px, `deal-calculator.html` 12~15px (2026-11-10 실측)
  - 데스크톱(769px 이상)에서는 기존 크기를 유지해도 된다
- **viewport 메타 통일**: `maximum-scale=1.0`을 제거해 핀치 줌을 막지 않는다 (`pushfold-chart.html`, `hand-ranking.html`)
  - 확대 방지는 16px 규칙으로 해결하고, 확대 금지로 해결하지 않는다
- **최소 글자 크기**: 모바일에서 정보성 텍스트(배지, 힌트, 범례, 표 라벨)는 12px 미만으로 두지 않는다
  - 현재 10~11px 사용처: `index.html`의 버튼/배지/표 라벨, `pushfold-chart.html`·`hand-ranking.html`의 `.hint`·`.legend`·`.note`, `deal-calculator.html`의 결과표 헤더
  - 핸드 격자 셀 안의 글자(AKs 등)처럼 공간이 고정된 곳은 예외
- **터치 영역**: 탭 가능한 요소는 실제 터치 영역을 최소 40×40px로 한다 (시각적 크기는 그대로 두고 padding/의사요소로 확장해도 됨)
  - 현재 미달: `.collapse-toggle` 26×26, `#host-bar .host-login-link` 높이 17px

## 완료 조건 (무인 실행 시 스스로 검증할 기준)
- [ ] 375px 뷰포트에서 4개 페이지 모든 `input/select/textarea`의 `getComputedStyle().fontSize`가 16px 이상인지 JS로 확인
- [ ] 4개 페이지 viewport 메타에 `maximum-scale`/`user-scalable=no`가 없는지 확인
- [ ] 375px 뷰포트에서 화면에 보이는 텍스트 노드 중 fontSize < 12px인 요소 목록을 뽑아, 예외(핸드 격자 셀)만 남는지 확인
- [ ] `.collapse-toggle`, `.host-login-link`의 클릭 가능 영역이 40×40px 이상인지 확인
- [ ] 데스크톱(1280px)에서 레이아웃이 기존과 눈에 띄게 달라지지 않았는지 스크린샷으로 확인
- [ ] 정산 결과 이미지 공유(html2canvas) 결과물이 깨지지 않는지 확인

## 제외 범위 (이번에 안 하는 것)
- 레이아웃 구조 변경 (각 페이지 개별 스펙에서 다룸)
- 색상/팔레트 변경
- 다크 모드

## 참고
- 관련 파일: `index.html`, `pushfold-chart.html`, `hand-ranking.html`, `deal-calculator.html`
- iOS Safari는 포커스된 입력칸 글자가 16px 미만이면 자동 확대한다
- 관련 스펙: [ledger-mobile-layout.md](ledger-mobile-layout.md), [deal-mobile-input.md](deal-mobile-input.md) — 입력칸 글자 크기는 이 스펙이 담당하고, 개별 스펙은 레이아웃만 다룬다
