# Convolution Accelerator on FPGA (Logical Circuit Final Project)

4×4 입력 행렬과 3×3 필터로 **2×2 Convolution(valid)** 결과를 계산하고, 결과를 7-segment에 표시하는 FPGA 프로젝트입니다.  
같은 연산을 **Single PE / 2×2 Systolic Array / 3×3 Systolic Array** 세 가지 방식으로 구현해 구조와 성능 차이를 비교했습니다.

---

## Repository structure

```text
Logical-Circuit-Final-Project/
├── lib/
│   ├── adder/
│   ├── demux/
│   ├── gate/
│   ├── mux/
│   └── register/
├── src/
│   ├── controller/
│   ├── display/
│   ├── memory/
│   ├── pe/
│   ├── systolic/
│   └── top/
├── constraint.xdc
├── final_report.pdf
├── implementation.mp4
└── mat_gen.py
```

- `lib/`: 게이트레벨 구현에 사용되는 기본 블록(standard-cell)
- `src/`: 컨트롤/연산/디스플레이 등 메인 RTL 모듈

---

## What’s inside

- **Gate-level / Structural modeling 중심 설계**
  - PE 내부 MAC(shift + add 기반 곱셈, 누산기) 구성
  - Binary → BCD 변환기, 7-seg decoder 등도 게이트레벨로 구현
    
- **세 가지 연산기 구현**
  - **Single PE**: 순차 처리(가장 느림)
  - **2×2 Systolic**: 4개 PE 병렬 활용(가장 빠름)
  - **3×3 Systolic**: 데이터 전파 방향 제약을 만족하도록 설계
    
- **FPGA 출력**
  - 연산 완료 후 7-seg에 **3×3 Systolic의 결과 4개 + 2×2 Systolic의 결과 4개**를 순서대로 표시

---

## Architecture

- **Controller**: FSM + counter로 모듈을 순차 활성화
- **Memory**: 4×4 입력(16개) + 3×3 필터(9개) 고정값 저장/출력
- **Processing Element (PE)**: MAC 연산 + mode 기반 연산 결과 전파
- **Systolic Arrays**: 2×2 / 3×3
- **Display**: Binary → BCD 변환 후 7-segment 출력
- **Top**: 전체 모듈 연결

---

## Performance (simulation)

동일한 입력에 대해 계산 완료 시간(behavioral simulation) 비교 결과입니다.

- **Single PE**: base  
- **2×2 Systolic**: ~3.34× faster  
- **3×3 Systolic(v2)**: ~1.8× faster  

> 출력 크기(2×2)에 비해 3×3 구조는 PE 활용률이 낮아 2×2가 더 유리했습니다.

---

## Report

전체 설계 배경, 구조, 시뮬레이션/FPGA 결과, 성능 비교는 `final_report.pdf`에 정리되어 있습니다.
