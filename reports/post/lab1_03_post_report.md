# LAB1-03 4비트 가산기 — 실험 후 레포트

- 과목: 전자전기컴퓨터설계실험Ⅱ / LAB1 조합논리 (교육 번호 03, 기존 번호 13)
- 작성자: 엄상혁 (학번 ______) / 조: ______ / 실험일: 2026-09-14 / 작성일: 2026-09-__

> 미수행·미확인 항목은 **미완료**로 표시했고 후속 확인을 적었다. bit 파일 생성만으로 보드 동작 성공을 선언하지 않는다.

## 1. 구현 환경 기록

| 항목 | 기록 |
|---|---|
| Vivado | 2026.1 (win64, Build 6511674) |
| part | xc7s75fgga484-1 (Spartan-7, fgga484, speed -1) |
| 설계 top | `adder_4bit` (Design Sources) |
| 시뮬레이션 top | `tb_adder_4bit` (`sim/tb_adder_4bit.sv`) |
| 핀 제약 | `constraints/pins.xdc`, IOSTANDARD LVCMOS33 |
| 소스 커밋 | `138faf9` — https://github.com/dhawldnjs010-star/lab1_03_adder_4bit/commit/138faf9dbe0d899c1a51c5c46f26b1e4e186c677 |
| Vivado 프로젝트 | `adder_4bit.runs` (Git 제외: `.gitignore`) |

## 2. VS Code(Icarus)와 Vivado 시뮬레이션 비교

두 실행은 같은 RTL과 같은 자기검사 TB(`tb_adder_4bit`)를 사용했다.

| 비교 항목 | VS Code (Icarus) | Vivado (xsim) |
|---|---|---|
| PASS 문구 | `LAB1_PASS adder_4bit cases=256` | 로그에 없음(재실행 필요) |
| 검사 수 | 256개 | - |
| 종료 시각 | 2560 ns | - |
| 로그 위치 | `evidence/simulation.txt` | `adder_4bit.sim/sim_1/behav/xsim/simulate.log` |
| 입력·출력 | 진리표와 일치 (사전 레포트 2절) | - |

일치 여부와 차이 원인: ______

## 3. 합성·구현·비트스트림

| 단계 | 결과 |
|---|---|
| Vivado 시뮬레이션 (xsim) | `simulate.log`에 PASS 문구가 남아 있지 않다(`Time resolution is 1 ps`만 기록). **재실행 후 로그 보관 필요** |
| Run Synthesis | `synth_design completed successfully`, 0 errors / 0 critical warnings / 0 warnings: 확인 |
| Run Implementation | 배치·배선 완료(`Fully Routed`) |
| Generate Bitstream | `write_bitstream completed successfully`: 확인 |
| bit 파일 | `adder_4bit.runs/impl_1/adder_4bit.bit` (3,687,013 bytes) |
| bit SHA-256 | `3bcf7e546ce5dcf58a905599ca526e02755c3d98c7ac0252ae5eb539b915241b` |
| DRC | Checks found: 1 — CFGBVS-1(Warning) |
| Methodology | Checks found: 0 |
| 타이밍 요약 | WNS/WHS = inf, 실패 endpoint 0 — 사용자 타이밍 제약이 없는 순수 조합회로라 setup/hold를 계산할 경로가 없다(`Timing 38-313`, `Power 33-232` 경고와 일치) |

- DRC의 `CFGBVS-1`은 CONFIG_VOLTAGE·CFGBVS 속성이 지정되지 않았다는 경고이다. 실제 보드의 구성 뱅크 전압과 대조해 해석한다(오류 아님).
- 경고 코드: `Place 46-29`, `Power 33-232`, `Timing 38-313` — 클록·타이밍 제약이 없는 조합회로에서 예상되는 경고이다.

현재 `constraints/pins.xdc` 상태: **정상** — 모든 포트(13개)에 PACKAGE_PIN과 IOSTANDARD(LVCMOS33)가 지정되어 있다.

> **수정 이력:** 기존 XDC는 `s[2]=N7`, `s[1]=M2`(서로 바뀜)였고, 교안 핀 표(`s[3]=M4, s[2]=M2, s[1]=N7, s[0]=M7`)에 맞게 `s[2]=M2`, `s[1]=N7`로 고쳤다. 기존 bit는 수정 전 XDC로 만들어졌으므로 **다시 구현·bit 생성**해야 한다.

## 4. 실제 장치 기록 — 미완료

조건: Spartan-7 XC7S75 교육용 보드, Hardware Manager → Open target → Auto Connect → 장치 `xc7s75` 확인 → Program Device로 bit 기록.

- 장치 인식·기록 완료: ☐
- 조교 무작위 선정 여부와 출석부 기록: ☐ 선정 ☐ 미선정

| 번호 | 입력 | 예상 출력 | 실제 출력 | 사진·영상 |
|---|---|---|---|---|
| 1 | ______ | ______ | ______ | `evidence/board/photos/______` |
| 2 | ______ | ______ | ______ | `evidence/board/videos/______` |

## 5. 결과 해석

- 예상값·두 시뮬레이션·실측의 일치 또는 차이와 원인: ______
- 사전 수정 실험에서 배운 점(실패 원인·복구): ______
- 시험 조건(입력 범위·경계 조건)에 대한 판단: ______

## 6. 미수행 항목과 후속 확인

- [ ] Vivado 시뮬레이션 재실행 후 PASS 로그(`simulate.log`) 보관
- [ ] 보드 기록·사진·영상 및 조교 확인(4절)

## 7. 제출 점검

- [ ] Vivado 버전·part·top·핀 제약·커밋 기록(1절)
- [ ] VS Code/Vivado 비교(2절), 합성·구현·bit(3절)
- [ ] 장치 기록·사진·영상(4절), 해석(5절), 미완료 항목(6절)
- [ ] `reports/post/`, `evidence/`에 저장 후 push, GitHub 웹에서 사진·영상 확인
