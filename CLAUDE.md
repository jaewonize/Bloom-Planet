# CLAUDE.md — Slow Terrarium

## 상위 컨텍스트

### 서비스 개요
저속노화(Slow Aging) 지표를 관리하는 헬스케어 앱.
4개 카테고리(잘 움직이기/잘 쉬기/잘 가꾸기/잘 다스리기) 점수를
시각적으로 보여주는 그래픽 대시보드.

### 데이터 수집 방식
- 엔드 프로덕트: 시스템이 자동 수집/분석 (웨어러블, 셀피 분석)
- 유저가 직접 입력하지 않음
- PoC: 수동 입력으로 점수 변화에 따른 그래픽 반응을 시연

### PoC A 목적
점수 변화에 따라 그래픽이 반응하는 걸 보여주기 위한 시뮬레이터.
엔드유저용 앱 화면이 아닌, 디자이너/기획자가 실시간으로 화면 느낌을 확인하는 용도.

### PoC A 레이아웃 원칙
- **좌측 (모바일 화면)**: 엔드유저가 실제로 보게 될 화면. 컨트롤 UI 없음.
- **우측 여백 (컨트롤 패널)**: 시연자/디자이너용. 모바일 화면 밖 브라우저 여백에 배치.
  - 4개 카테고리 슬라이더
  - 트리거 버튼들 (점수 급락, 할로윈 특별 보상, 100회 방문 달성 등)
- 슬라이더/버튼 조작 → 좌측 모바일 화면 그래픽이 동적으로 반응

```
[브라우저 창]
┌─────────────────┬──────────────────┐
│                 │  🎛 Control Panel │
│  📱 모바일 화면  │  ── 잘 움직이기  │
│  (유저가 보는   │  ── 잘 쉬기      │
│   실제 화면)    │  ── 잘 가꾸기    │
│                 │  ── 잘 다스리기  │
│                 │                  │
│                 │  [트리거 버튼들]  │
└─────────────────┴──────────────────┘
```

### 전체 테마 목록 (PoC A)
- **Slow Terrarium** (지구 꾸미기) ← 이 폴더
- **Slow Museum** (갤러리)
- **Wellness Marble** (보드게임판)

---

## 프로젝트 개요
저속노화 지표 4개 카테고리 점수를 3D 지구 꾸미기로 시각화하는 PoC A.
점수가 오를수록 지구 위에 보상 아이템(동식물/사물)이 배치되고, 합산 점수가 지구 자전 속도에 반영됨.

## 기술 스택
- Three.js r128 (CDN)
- 순수 HTML/CSS/JS (프레임워크 없음)
- 단일 HTML 파일

## 파일 구조
```
slow-terrarium/
├── CLAUDE.md
├── PocA_SlowTerrarium_v2.html   # 메인 파일
└── assets/
    ├── move/                    # 잘 움직이기 이미지
    ├── rest_tree/               # 잘 쉬기 나무 이미지
    └── rest_shelter/            # 잘 쉬기 휴식처 이미지
```

## 점수 체계

### 4개 카테고리
| 카테고리 | 데이터 소스 |
|---------|-----------|
| 잘 움직이기 (move) | 웨어러블 (걸음수, 운동량) |
| 잘 쉬기 (rest) | 웨어러블 (수면 데이터) |
| 잘 가꾸기 (groom) | 셀피 분석 |
| 잘 다스리기 (manage) | 웨어러블 (HRV, 스트레스) |

### LED 상태
- delta ≥ +2 → green (상승)
- delta ≤ -2 → red (하락)
- 그 외 → off

### 보상 지급 로직
- 누적 상승폭 기준
- 하락해도 누적 유지, 소급 없음
- 2점 간격으로 보상 지급

### 저속노화 속도 (자전 속도 7단계)
- 합산 점수 기준
- 점수 높을수록 → 느린 자전 (저속노화)
- 점수 낮을수록 → 빠른 자전

## 그래픽 스펙
- Three.js 3D 지구 구체
- 아이소메트릭 대륙 레이어
- 오렌지 3종 랜덤 컬러 실린더 디스크 보상
  - `0xe85820` 레드 오렌지
  - `0xe87830` 순수 오렌지
  - `0xf0a030` 옐로 오렌지
- 슬롯 배치: 남위 45° ~ 북위 45°, Great Circle Distance 이격
- 최대 슬롯: 20개

## 보상 테이블

### 잘 움직이기 (20종)
| 순서 | 아이템 | GLB 파일명 | 이미지 파일명 |
|------|--------|-----------|-------------|
| 1 | 사슴 | deer_pbr.glb | assets/move/deer.png |
| 2 | 닭 | chicken_pbr.glb | assets/move/chicken.png |
| 3 | 양 | sheep_pbr.glb | assets/move/sheep.png |
| 4 | 앵무새 | parrot_pbr.glb | assets/move/parrot.png |
| 5 | 거북이 | turtle_pbr.glb | assets/move/turtle.png |
| 6 | 홍학 | flamingo_pbr.glb | assets/move/flamingo.png |
| 7 | 기린 | giraffe_pbr.glb | assets/move/giraffe.png |
| 8 | 공작 | peacock_pbr.glb | assets/move/peacock.png |
| 9 | 얼룩말 | zebra_pbr.glb | assets/move/zebra.png |
| 10 | 캥거루 | kangaroo_pbr.glb | assets/move/kangaroo.png |
| 11 | 곰 | bear_pbr.glb | assets/move/bear.png |
| 12 | 황소 | bull_pbr.glb | assets/move/bull.png |
| 13 | 산양 | ram_pbr.glb | assets/move/ram.png |
| 14 | 코끼리 | elephant_pbr.glb | assets/move/elephant.png |
| 15 | 치타 | cheetah_pbr.glb | assets/move/cheetah.png |
| 16 | 레드판다 | red_panda_pbr.glb | assets/move/red_panda.png |
| 17 | 흰여우 | arctic_fox_pbr.glb | assets/move/arctic_fox.png |
| 18 | 사자 | lion_pbr.glb | assets/move/lion.png |
| 19 | 팬더 | panda_pbr.glb | assets/move/panda.png |
| 20 | 블랙팬서 | panther_pbr.glb | assets/move/panther.png |

### 잘 쉬기 — 나무 (10종)
| 순서 | 아이템 | GLB 파일명 | 이미지 파일명 |
|------|--------|-----------|-------------|
| 1 | 갈대 | cattail_pbr.glb | assets/rest_tree/cattail.png |
| 2 | 대나무 | bamboo_pbr.glb | assets/rest_tree/bamboo.png |
| 3 | 바나나 나무 | banana_pbr.glb | assets/rest_tree/banana_tree.png |
| 4 | 파파야 나무 | papaya_pbr.glb | assets/rest_tree/papaya_tree.png |
| 5 | 야자수 | palm_pbr.glb | assets/rest_tree/palm_tree.png |
| 6 | 버드나무 | willow_pbr.glb | assets/rest_tree/willow.png |
| 7 | 낙우송 | bald_cypress_pbr.glb | assets/rest_tree/bald_cypress.png |
| 8 | 전나무 | tanenbaum_pbr.glb | assets/rest_tree/fir.png |
| 9 | 도토리 나무 | acorn_pbr.glb | assets/rest_tree/oak.png |
| 10 | 아카시아 | acacia_pbr.glb | assets/rest_tree/acacia.png |

### 잘 쉬기 — 휴식처 (10종)
| 순서 | 아이템 | GLB 파일명 | 이미지 파일명 |
|------|--------|-----------|-------------|
| 1 | 열기구 | balloon_pbr.glb | assets/rest_shelter/balloon.png |
| 2 | 파라솔 | parasol_pbr.glb | assets/rest_shelter/parasol.png |
| 3 | 캠프파이어 | campfire_pbr.glb | assets/rest_shelter/campfire.png |
| 4 | 무지개 | rainbow_pbr.glb | assets/rest_shelter/rainbow.png |
| 5 | 티피텐트 | teepee_pbr.glb | assets/rest_shelter/teepee.png |
| 6 | 오아시스 | oasis_pbr.glb | assets/rest_shelter/oasis.png |
| 7 | 온천 | spring_pbr.glb | assets/rest_shelter/hot_spring.png |
| 8 | 오두막 | cabin_pbr.glb | assets/rest_shelter/cabin.png |
| 9 | 폭포 | waterfall_pbr.glb | assets/rest_shelter/waterfall.png |
| 10 | 게르 | hut_pbr.glb | assets/rest_shelter/ger.png |

### 잘 가꾸기 (미정)
### 잘 다스리기 (미정)

## 작업 이력
- [x] Three.js 3D 지구 구체 구현
- [x] 오렌지 3종 랜덤 컬러 디스크 보상
- [x] 자전 속도 7단계 연동
- [x] 점수 입력 → LED 상태 업데이트
- [x] 보상 배치 로직
- [x] 하단 카테고리 아이콘 제거
- [x] GLB 파일 스케일 조정 (glb_activity_scale, glb_restoration_scale)
- [ ] 잘 가꾸기 보상 테이블 확정
- [ ] 잘 다스리기 보상 테이블 확정
- [ ] 슬롯 차감 로직 설계
- [ ] 이미지 매핑 (CORS 해결 필요 — Python http.server 또는 GitHub Pages)
