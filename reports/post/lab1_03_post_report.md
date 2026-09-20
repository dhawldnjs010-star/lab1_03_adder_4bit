# 실험 후 레포트: LAB1-03 4비트 가산기

작성자: 엄상혁 (학번 2025440084) / 조: g조 / 실험일: 2026-09-14 / 소스 커밋: `138faf9` (https://github.com/dhawldnjs010-star/lab1_03_adder_4bit/commit/138faf9dbe0d899c1a51c5c46f26b1e4e186c677) / 구현 도구·버전: Vivado 2026.1 (Build 6511674) / part: xc7s75fgga484-1 / top: `adder_4bit` (시뮬레이션 top `tb_adder_4bit`) / XDC: `constraints/pins.xdc`

경로: Vivado 경로로 수행했다.

## Vivado 시뮬레이션 — Vivado 경로

프로젝트 생성·등록: RTL(`src/adder_4bit.v`)은 Design Sources, TB(`sim/tb_adder_4bit.sv`)는 Simulation Sources, XDC(`constraints/pins.xdc`)는 Constraints에 추가했다(Copy sources 끔). 설계 top은 `adder_4bit`, 시뮬레이션 top은 `tb_adder_4bit`이다.

| 실행 | PASS 문구 | 검사 수 | 종료 시각 | 로그 |
|---|---|---|---|---|
| VS Code (Icarus) | `LAB1_PASS adder_4bit cases=256` | 256개 | 2560 ns | `evidence/simulation.txt` |
| Vivado xsim | `LAB1_PASS adder_4bit cases=256` | 256개 | 2560 ns | `evidence/vivado/xsim_simulate.log`, `evidence/vivado/console_lab1_03_sanghyeok_0920.txt` |

- Icarus와 Vivado xsim의 PASS 문구, 검사 수, 종료 시각이 같다.

- TB 종료 시각(2560 ns)이 Vivado 기본 실행 시간(1000 ns)보다 길어 `run all`(또는 `xsim.simulate.runtime` 3000ns)로 실행했다.

- 파형: `evidence/wave.vcd`(Icarus VCD).

## 오픈소스 실행 환경 — CLI 경로

이 랩은 Vivado 경로로 수행했다. CLI 경로는 사용하지 않았다.

## 합성·구현·비트스트림

| 항목 | 결과 |
|---|---|
| Run Synthesis | 완료. `Synthesis finished with 0 errors, 0 critical warnings and 0 warnings.` |
| Run Implementation | 배치·배선 완료 |
| Generate Bitstream | `write_bitstream completed successfully` |
| 이용률 | Slice LUT 4 / Bonded IOB 13 (xc7s75: LUT 48,000 / IOB 338) |
| DRC | Checks found: 1 — CFGBVS-1(Warning) |
| Methodology | Checks found: 0 |
| 타이밍 | WNS/WHS = inf, 실패 endpoint 0. 사용자 타이밍 제약이 없는 조합회로라 통과 수치가 아니다(`Timing 38-313`). |
| 경고 | `Place 46-29`, `Power 33-232`, `Timing 38-313` |

- bit 경로: `adder_4bit.runs/impl_1/adder_4bit.bit` (Git 제외) / 크기: 3,687,013 bytes / SHA-256: `9828b1c6edfd39706357512491c32792272727a0191c2ca49b9ca6cd99235d6f`

- 보고서 원본: `evidence/vivado/`의 `synth_runme.log`, `impl_runme.log`, `drc_routed.rpt`, `methodology_drc_routed.rpt`, `timing_summary_routed.rpt`, `utilization_placed.rpt`.

- DRC의 `CFGBVS-1`은 CONFIG_VOLTAGE·CFGBVS 속성이 지정되지 않았다는 경고이다. 실제 보드의 구성 뱅크 전압과 대조해 해석하며 오류는 아니다.

## 실제 보드 기록·실측

연결된 장치: Spartan-7 XC7S75 교육용 보드(part `xc7s75fgga484-1`), 기록 도구: Vivado Hardware Manager (Open target → Program Device). 콘솔 로그: `evidence/board/console_lab1_03_sanghyeok.txt`.

- Hardware Manager 콘솔에서 `program_hw_devices`가 6회 실행되었다.

- 배선·입력·출력이 보이는 영상: [Google Drive 폴더](https://drive.google.com/drive/folders/1jEJf1LYmlttKZgpoQ-KwmKHDibAAv11X)의 `20260914_162934.mp4` (2026-09-14 16:29:34 촬영).


| 조건 | 예상 출력 | 실측 출력 | 사진/영상 시각 | 일치 여부·원인 |
|---|---|---|---|---|
| a=0000 b=0000 | cout=0 s=0000 (a+b=0) | 예상 출력과 같음 | `20260914_162934.mp4` (2026-09-14 16:29:34) | 일치 |
| a=0111 b=1000 | cout=0 s=1111 (a+b=15) | 예상 출력과 같음 | `20260914_162934.mp4` (2026-09-14 16:29:34) | 일치 |
| a=1000 b=1000 | cout=1 s=0000 (a+b=16) | 예상 출력과 같음 | `20260914_162934.mp4` (2026-09-14 16:29:34) | 일치 |
| a=1001 b=0111 | cout=1 s=0000 (a+b=16) | 예상 출력과 같음 | `20260914_162934.mp4` (2026-09-14 16:29:34) | 일치 |
| a=1111 b=0001 | cout=1 s=0000 (a+b=16) | 예상 출력과 같음 | `20260914_162934.mp4` (2026-09-14 16:29:34) | 일치 |
| a=0101 b=1010 | cout=0 s=1111 (a+b=15) | 예상 출력과 같음 | `20260914_162934.mp4` (2026-09-14 16:29:34) | 일치 |
| a=1111 b=1111 | cout=1 s=1110 (a+b=30) | 예상 출력과 같음 | `20260914_162934.mp4` (2026-09-14 16:29:34) | 일치 |


실측은 작성자가 보드에서 직접 확인한 결과이다.

## 비교·결론

- 예상값(진리표) → VS Code(Icarus) `LAB1_PASS adder_4bit cases=256` → Vivado xsim `LAB1_PASS adder_4bit cases=256`: 검사 256개 모두 일치하고 종료 시각 2560 ns로 같다.

- 실측: 위 표의 모든 조건에서 예상 출력과 같았다. 불일치는 없었다.

- 구현 성공(bit 생성)만으로 동작을 확인한 것으로 보지 않고, 위 실측 표를 별도로 확인했다.

## 제출 링크

소스 커밋: https://github.com/dhawldnjs010-star/lab1_03_adder_4bit/commit/138faf9dbe0d899c1a51c5c46f26b1e4e186c677 / 실험 전 레포트: `reports/pre/lab1_03_pre_report.md` / 로그·VCD: `evidence/simulation.txt`, `evidence/wave.vcd`, `evidence/vivado/` / bit·해시: 위 3절 (SHA-256 `9828b1c6edfd39706357512491c32792272727a0191c2ca49b9ca6cd99235d6f`) / 영상: https://drive.google.com/drive/folders/1jEJf1LYmlttKZgpoQ-KwmKHDibAAv11X (`20260914_162934.mp4`) / GitHub에서 링크 확인한 날짜: 2026-09-20
