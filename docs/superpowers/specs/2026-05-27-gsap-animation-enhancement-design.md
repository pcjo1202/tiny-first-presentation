# GSAP 애니메이션 강화 설계

## 개요

- **목표**: 대면 발표의 시각적 품질 향상
- **방법**: GSAP CDN 추가, 기존 CSS 애니메이션을 GSAP 타임라인으로 교체
- **원칙**: 콘텐츠를 보강하는 애니메이션만 사용, 과한 효과 배제

## 변경 범위

단일 파일(`index.html`)만 수정. HTML 마크업 구조 변경 없음.

### 추가

- GSAP Core CDN (`gsap.min.js`)

### 제거

- CSS `@keyframes childIn` 및 관련 스태거 룰
- 기존 `go()` 함수의 opacity 기반 전환

### 교체

- 슬라이드 전환 → GSAP 기반 방향성 전환
- `.a` 요소 등장 → GSAP 타임라인 기반 요소별 등장

## 슬라이드 전환

| 유형 | 감지 방법 | 효과 |
|---|---|---|
| 섹션 디바이더 | `.num` 요소 존재 | scale(0.92→1) + opacity |
| 일반 콘텐츠 | 기본 | translateX(60px→0) + opacity |
| 뒤로 가기 | 방향 감지 | translateX(-60px→0) + opacity |

- 전환 시간: 0.4s, ease: `power2.out`
- 퇴장: opacity만 0.15s로 빠르게

## 요소별 등장 애니메이션

| 요소 | 효과 | 비고 |
|---|---|---|
| `.cards .card` | stagger(0.1s) + scale(0.95→1) + opacity + y(20→0) | 콘텐츠 카드 |
| `.vs-container .vs-box:first` | x(-30→0) + opacity | 좌측 진입 |
| `.vs-container .vs-divider` | scale(0.5→1) + opacity | 중앙 팝 |
| `.vs-container .vs-box:last` | x(30→0) + opacity | 우측 진입 |
| `.table-wrap tr` | stagger(0.08s) + opacity + y(12→0) | 행 순차 |
| `.flow .step, .flow .arrow` | stagger(0.12s) + opacity + x(-20→0) | 순차 체인 |
| `.formula` | opacity + scale(0.97→1) | 미세 강조 |
| `.code-block` | opacity + y(16→0) | 블록 단위 |
| `.callout` | opacity + y(16→0) | 알림 박스 |
| `.cmp .cmp-col:first` | x(-30→0) + opacity | Before 좌측 |
| `.cmp .cmp-col:last` | x(30→0) + opacity | After 우측 |
| `.emoji-huge, .emoji-big` | scale(0.85→1) + opacity | 부드러운 등장 |
| `.toc .toc-item` | stagger(0.1s) + opacity + x(-20→0) | 목차 항목 |
| `.list-items li` | stagger(0.1s) + opacity + x(-16→0) | 리스트 |
| 기타 `.a` 요소 | opacity + y(16→0), stagger(0.06s) | 폴백 |

## 구현 구조

```javascript
// 1. 슬라이드 진입 시 타임라인 생성
function animateSlideIn(slide, direction) {
  var tl = gsap.timeline();
  // 슬라이드 자체 진입
  // 내부 요소별 애니메이션 추가
  return tl;
}

// 2. go() 함수에서 호출
function go(to) {
  // 현재 슬라이드 퇴장 (GSAP)
  // 새 슬라이드 진입 (animateSlideIn)
}
```
