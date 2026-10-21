# Realbarrier THE REAL QUIZ — 작업 기록 / 개발 버전

> 임시 저장용 작업 기록. 컴퓨터 재시작 등으로 작업 맥락이 끊겼을 때 이 문서만 보고 이어서 작업할 수 있도록 정리.
> 최종 갱신: 2026-10-21

## 1. 현재 버전

| 항목 | 값 |
|---|---|
| 앱 버전 (빌드) | `20261021-1930` |
| 배포 커밋 | 아래 변경 이력 최신 행 |
| 배포 URL | https://autolife1.github.io/rb-quiz-proto/ |
| GitHub 저장소 | `autolife1/rb-quiz-proto` (**공개** — 행사 후 비공개 전환 예정) |
| 로컬 작업 폴더 | `~/Desktop/realbarrier-quiz/` |
| 상태 | 프로토타입. **실기기(갤럭시탭) 검증 전** |

## 2. 개요

- 광고주(리얼베리어) BTL 행사용 태블릿 퀴즈. **인터넷 없이 동작**하는 PWA (웹앱).
- 기기: **갤럭시탭 2대** (TAB-01, TAB-02). Chrome으로 설치.
- 언어: 국문(시작하기) / 영문(English 버튼).
- 진행: **초상권 동의** → 시작(언어 선택) → Q1 → Q2 → Q3 → 결과 → 처음으로(=동의 화면). 오답이어도 다음 문제로 진행(B안).
- 동의 화면: 국문·영문 동시 표시, 동의합니다 / 동의하지 않습니다. 미동의 → 안내("촬영 동의 후 참여 가능") → 8초 후 동의 화면. 문구는 광고주 제공(2026-10-21).
- 플레이 기록은 기기 내부에 저장, 관리자 화면에서 CSV로 내보내기.

## 3. 폴더 구조

```
realbarrier-quiz/
├─ README.md          ← 이 문서
├─ build.py           ← src → dist 빌드 (이미지·폰트를 index.html 안에 넣음)
├─ src/
│  ├─ index.html      ← 문항·문구·설정 수정은 여기서 (QUIZ / TEXT / CONFIG)
│  └─ assets/         ← logo, logo-white, toner, cream, mask, lotion, slogan (.png)
└─ dist/              ← 배포본 (git 저장소, GitHub Pages로 서비스)
   ├─ index.html      ← 단일 파일 (약 1.8MB, 이미지·폰트 포함)
   ├─ sw.js           ← 오프라인 캐시 (빌드 시 자동 생성)
   ├─ manifest.webmanifest, icon-192/512.png, apple-touch-icon.png
   └─ README.md       ← 이 문서 사본
```

- 폰트: Pretendard (Regular/Bold/ExtraBold/Black, OFL 무료). `~/Library/Fonts`에서 사용 글자만 잘라 포함.
- 브랜드 컬러: `#0030A8` (로고에서 추출). 배너 이미지 색은 `#083891`로 약간 다름.
- 원본 에셋: `~/Downloads/브랜드 에셋/`, 배너 원본 `~/Downloads/KakaoTalk_Photo_2026-10-20-19-50-33.png`

## 4. 수정 → 재배포 방법

```bash
cd ~/Desktop/realbarrier-quiz
# 1) src/index.html 수정 (문항은 QUIZ.ko / QUIZ.en, 결과 문구는 TEXT)
python3 build.py                 # 2) dist 재생성
cd dist && git add -A && git commit -m "수정 내용" && git push   # 3) 배포 (1~2분 후 반영)
```

- 기기 반영: 인터넷 연결 상태에서 앱을 **두 번** 실행 (첫 실행에 새 버전 받음, 두 번째부터 적용).
- **행사 기간 중에는 배포하지 말 것** (기기마다 버전이 달라질 수 있음).

## 5. 문항 (확정 — 광고주 제공)

### 국문
| | 유형 | 문항 | 보기 | 정답 |
|---|---|---|---|---|
| Q1 | 빈칸 4지선다 | 독자개발 ___™ 성분으로 / 피부장벽을 근본부터 회복하는 / 리얼베리어 익스트림 ___ 크림 | 아스트로 / 슈퍼파워 / 프로세라 / 리얼진짜 | **프로세라** |
| Q2 | O/X | 시중 기능성화장품 중 피부장벽 회복 기능성 제품은 단 1%에 불과하다 | O / X | **O** |
| Q3 | O/X | NEW 익스트림 프로세라 크림은 5배 더 많은 세라마이드를 흡수해 스스로 회복하는 피부장벽을 세우는 크림이다 | O / X | **O** |

### 영문
| | 유형 | 문항 | 보기 | 정답 |
|---|---|---|---|---|
| Q1 | 빈칸 4지선다 | Our very own secret-sauce ___™ ingredient / rebuilds your skin barrier from the ground up / — Realbarrier Extreme ___ Cream | Astro / Superpower / ProCera / RealReal | **ProCera** |
| Q2 | True/False | Out of all the functional cosmetics on the market, only 1% are certified to help restore your skin barrier. | O / X | **O** |
| Q3 | True/False | The NEW Extreme ProCera Cream helps your skin absorb 5x more ceramides, so it can build a skin barrier that repairs itself! | O / X | **O** |

- Q1 화면 하단에 크림 튜브 이미지 (국문·영문 공통).

### 결과 화면
| 맞힌 개수 | 국문 | 영문 |
|---|---|---|
| 3 | 축하드립니다! / 정답을 모두 맞히셨습니다. | Congratulations! / Your skin barrier must be proud |
| 2 | 아깝습니다! / 3문제 중 2문제를 맞히셨습니다. | Nice Try! / But Don’t Worry, We’ll still keep your skin protected |
| 1 | 조금 아쉬워요! / 3문제 중 1문제를 맞히셨습니다. | (위와 동일) |
| 0 | 다음엔 꼭 맞힐 수 있어요! / 정답을 확인해 보세요. | (위와 동일) |

- 공통: 문항별 정답 칩, THIS IS REAL 배너(이미지), 제품 4종(토너·크림·마스크·로션), 처음으로 버튼.

## 6. 설정값 (`src/index.html` → `CONFIG`)

| 항목 | 값 |
|---|---|
| 관리자 진입 | **첫 화면(동의 화면) 왼쪽 위 구석 5번 연속 탭** |
| 관리자 PIN | `2580` (그대로 사용하기로 확정) |
| 동의 후 시작 화면 무입력 | 30초 → 동의 화면 (중도이탈 기록) |
| 문제 화면 무입력 | 60초 → 처음으로 (중도이탈 기록) |
| 미동의 안내 화면 | 8초 → 동의 화면 |
| 결과 화면 자동 복귀 | 20초 |
| 선택 후 다음 화면 | 0.9초 |
| 기본 기기 이름 | TAB-01 |

## 7. 로그 / CSV

- 저장: 기기 내부 localStorage (`rbq.logs.v1`). 기기 간 동기화 없음.
- 기록 1건 = 동의/미동의 버튼 1회. 미동의도 기록됨.
- CSV 열: `기기, 날짜, 동의여부(동의/미동의), 동의시각, 언어(KO/EN), 시작, 종료, 소요(초), 결과(완료/중도이탈/미동의), 점수, 성공여부, Q1~Q3 선택, Q1~Q3 정답여부`
- 동의 후 언어 선택 없이 이탈하면: 동의 + 언어 빈칸 + 중도이탈
- 성공여부: 완료 + 3문제 모두 정답 → **성공** / 완료 + 오답 있음 → **실패** / 중도이탈 → 빈칸
- CSV는 매번 **전체 누적**. 행사 중 초기화하지 않으면 마지막 날 파일 2개(TAB-01, TAB-02)만 합치면 됨.
- 엑셀: 성공 인원 `=COUNTIF(K:K,"성공")`, 동의 인원 `=COUNTIF(C:C,"동의")`, 기기별 `=COUNTIFS(A:A,"TAB-01",K:K,"성공")`
- 관리자 화면: 동의/미동의 수, 전체 요약, 언어별 수, 문항별 정답률, 날짜별·시간대별, 최근 20건, CSV 내보내기 / 텍스트 보기 / 기록 초기화.

## 8. 갤럭시탭 세팅 (기기별)

1. 설정 → 잠금 화면 → 화면 잠금 방식 → **PIN**
2. 설정 → 보안 및 개인정보 보호 → 기타 보안 설정 → **앱 고정** 켜기 + "고정 해제 전 잠금 화면 유형 사용" 켜기 (메뉴 위치는 One UI 버전마다 다를 수 있음 → 설정 검색창에 "앱 고정")
3. 설정 → 디스플레이 → 탐색 바 → **버튼**
4. **Chrome**으로 배포 URL 접속 → ⋮ → **앱 설치**
5. 설치된 아이콘으로 실행 → 관리자에서 "오프라인 캐시: 준비 완료" 확인 → 기기 이름 TAB-01 / TAB-02 지정
6. 비행기 모드에서 실행 확인, CSV 내보내기 테스트 → **기록 초기화**
7. 개발자 옵션(빌드번호 7번 탭) → **충전 중 화면 켜짐 유지**, 자동 회전 끄기, 충전기 연결
8. 앱 실행 → 최근 앱(|||) → 앱 아이콘 → **이 앱 고정**. 해제: 최근 앱 + 뒤로 동시 길게 → PIN
9. 행사 중 **비행기 모드** 유지 (팝업·업데이트 방지)

## 9. 행사 운영 / 종료 후

- 매일 종료 후: 기기별 CSV 내보내기 (카톡 나와의 채팅·메일). 초기화하지 않음.
- 종료 후:
  1. 최종 CSV 2개 확보 확인
  2. 저장소 비공개 전환: `gh repo edit autolife1/rb-quiz-proto --visibility private --accept-visibility-change-consequences` (Pages 자동 중단)
  3. 태블릿: 앱 삭제 + Chrome 사이트 데이터 삭제 (반납·재사용 시 공장 초기화 권장)

## 10. 변경 이력

| 날짜 | 커밋 | 내용 |
|---|---|---|
| 10-18 | 8ebb59c | 최초 프로토타입 (국문, 로그·관리자·CSV, 오프라인 PWA) 배포 |
| 10-20 | 1ea298f | 영문 버전, THIS IS REAL 배너(임시 폰트), CSV 언어 열 |
| 10-20 | c8e182a | CSV 성공여부 열 |
| 10-20 | f656678 | 배너를 공식 이미지로 교체, Pro-Cera → ProCera |
| 10-20 | 28a0317 | 국문 문항 신규 시안으로 교체 (Q1 빈칸, Q2 O/X), Q1 크림 이미지 |
| 10-21 | f2f081b | 초상권 동의 페이지(국/영) 추가, CSV 동의여부·동의시각 열, 관리자 진입 위치 → 동의 화면 |

## 11. 남은 일 / 미검증

- [ ] 갤럭시탭 실기기 검증: 설치, 오프라인 실행, 터치 반응, CSV 공유, 화면 꺼짐 방지
- [ ] 기기 이름 지정 + 테스트 기록 초기화
- [ ] (선택) 브랜드 공식 HEX 받아 앱 컬러를 배너 색과 통일
- [ ] (선택) 인터넷 연결 시 구글 시트 자동 전송 기능 — 요청 시 개발
- 데스크톱 Chrome에서 iPad·갤럭시탭 해상도로 흐름·로그·CSV·오프라인 동작 확인 완료.
