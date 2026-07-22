# Holdem Mates

친구들끼리 쓰는 홀덤(포커) 유틸리티 웹앱입니다.

- 배포 주소: [holdemmates.com](https://holdemmates.com)
- 별도 빌드 과정 없는 순수 HTML/CSS/JS 정적 사이트 (GitHub Pages)

## 구성 파일

| 파일 | 설명 |
|---|---|
| `index.html` | 정산 계산기 — 바이인 관리, 날짜별 방(Room), Firebase 실시간 공유, 결과 이미지 내보내기 |
| `pushfold-chart.html` | Nash Push/Fold 차트 — 인원수·스택·앤티별 푸시/콜 레인지 |
| `CNAME` | Cloudflare 커스텀 도메인 설정 |

## 배포

`main` 브랜치에 푸시하면 GitHub Pages에 즉시 반영됩니다. 실험적인 변경은 별도 브랜치에서 작업 후 머지하는 것을 권장합니다.

## 개발 시 참고

작업 방식과 유의사항은 [CLAUDE.md](./CLAUDE.md)를 참고하세요.
