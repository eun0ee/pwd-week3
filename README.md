# TypeScript 웹 계산기 (PWD Week 3)

HTML, CSS, TypeScript를 사용하여 구현한 웹 기반 계산기 프로젝트입니다. 계산 로직, 상태 관리, 화면/이벤트 처리를 파일 단위로 분리하여 구현하였습니다.

---

## 1. 프로젝트 배포 및 저장소 링크

- **GitHub Pages (배포 주소):** `https://본인아이디.github.io/pwd-week3/`
- **GitHub Repository:** `https://github.com/본인아이디/pwd-week3`

> *(본인 GitHub 계정명으로 변경하여 사용하세요.)*

---

## 2. 주요 기능

- **기본 사칙연산**: 덧셈(`+`), 뺄셈(`−`), 곱셈(`×`), 나눗셈(`÷`) 및 연속 계산 지원
- **추가 기능**:
  - 소수점(`.`) 및 부호 전환(`+/-`)
  - 백분율(`%`) 계산
  - 마지막 글자 지우기(`⌫`) 및 전체 초기화(`AC`)
- **예외 처리**: 0으로 나누기 방지 및 계산 허용 범위 초과 시 오류 메시지 표시
- **UI/UX**: 입력 중인 수식(`expression`) 미리보기 및 천 단위 쉼표 포맷팅 표시

---

## 3. 파일 구조 및 역할 분담

```text
pwd-week3/
├── index.html          # 계산기 마크업 구조 정의 (시맨틱 태그 및 data-key 활용)
├── styles.css          # CSS Grid 기반 레이아웃 및 반응형 스타일링
├── operations.ts       # 순수 사칙연산 함수 및 Strategy 패턴 정의
├── calculator.ts       # 계산기 상태(State) 관리 및 키 입력 분기 처리
├── app.ts              # DOM 이벤트 바인딩 및 화면 렌더링(render)
├── tsconfig.json       # TypeScript 컴파일러 설정
└── package.json        # 의존성 모듈 및 실행 스크립트 설정
```

### 각 TypeScript 파일의 역할

- **`operations.ts`**
  - DOM 조작이나 상태 변경 없이 `(left, right)` 두 숫자를 받아 계산 결과만 반환하는 순수 함수 모음입니다.
  - 사칙연산 함수 객체(`operations`)와 Strategy 패턴 형태의 `calculate()` 함수를 제공합니다.
- **`calculator.ts`**
  - 계산기의 상태 객체(`state`: `input`, `stored`, `operator`, `waiting` 등)를 관리합니다.
  - 키 입력에 따른 상태 변경 로직(`inputDigit`, `selectOperator`, `equals`, `handleKey` 등)을 처리합니다.
- **`app.ts`**
  - 버튼의 클릭 이벤트를 감지하여 `handleKey()`에 전달합니다.
  - 변경된 `state`를 실제 화면(DOM)에 반영하는 `render()` 함수를 실행합니다.

---

## 4. 로컬 실행 및 빌드 방법

### 요구 환경
- Node.js & npm

### 실행 단계

1. **의존성 패키지 설치**
   ```bash
   npm install
   ```

2. **타입 검사 (컴파일 없이 오류 검사)**
   ```bash
   npm run check
   ```

3. **JavaScript 빌드 (TypeScript 컴파일)**
   ```bash
   npm run build
   ```
   *(코드 수정 시 실시간 자동 컴파일을 원할 경우: `npm run watch`)*

4. **브라우저 실행**
   - 생성된 `index.html` 파일을 더블클릭하거나 브라우저로 엽니다.

---

## 5. 실습 동작 확인 및 코드 매핑 (Step 7 실습 관찰 결과)

실습 가이드(Step 7)를 기반으로 직접 동작을 확인하고 담당 코드 영역을 매핑한 결과입니다.

| 기능 분류 | 입력 예시 | 확인된 결과 | 담당 파일 및 주요 함수 |
|---|---|---|---|
| **기본 사칙연산** | `12 + 3 =` | `15` | `operations.ts` (`add`, `calculate`) |
| **0으로 나누기 예외 처리** | `12 ÷ 0 =` | `Error` 및 "0으로 나눌 수 없습니다." 표시 | `operations.ts` (`divide`), `calculator.ts` (`handleKey` 내 `try...catch`) |
| **소수점 계산** | `0.1 + 0.2 =` | `0.3` (유효숫자 포맷팅) | `calculator.ts` (`formatNumber`) |
| **백분율 / 부호 전환** | `50 → %`, `5 → +/-` | `0.5`, `-5` | `calculator.ts` (`handleKey`의 `percent`, `sign` 로직) |
| **한 글자 지우기** | `123 → ⌫` | `12` | `calculator.ts` (`handleKey`의 `delete` 로직) |
| **연산자 덮어쓰기** | `2 + × 3 =` | 마지막 선택된 연산자(`×`)가 적용되어 `6` 출력 | `calculator.ts` (`selectOperator`) |
| **쉼표 포맷팅** | `6110000` 입력 | 화면에 `6,110,000`으로 출력 | `app.ts` (`formatDisplay`) |
| **전체 초기화** | `AC` 클릭 | 표시창 `0` 및 계산식 초기화 | `calculator.ts` (`clear`), `app.ts` (`render`) |