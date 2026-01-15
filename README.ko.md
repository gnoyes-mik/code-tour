# Code Tour

함수를 따라 드릴다운하며 코드를 탐색하는 인터랙티브 도구

[English](./README.md)

## Code Tour란?

Code Tour는 함수 호출을 따라 드릴다운하며 코드를 탐색하는 Claude Code 플러그인입니다. 라인 단위로 이동하는 대신, 함수 안으로 들어가며 코드 흐름을 이해합니다.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
**src/service/AuthService.java:42**

```
  login() - 로그인 엔트리포인트
  └─ authService.authenticate() ← 현재 위치
     ├─[a] userRepository.findByEmail() - 이메일로 사용자 조회
     ├─[b] passwordEncoder.matches() - 비밀번호 검증
     └─[c] mfaService.verify() - MFA 검증
```
```
──────────────────────────────────────────────────────────────────────
```
```java
   40 │   public User authenticate(Credentials credentials) {
   41 │     User user = [a]userRepository.findByEmail(credentials.getEmail());
   42 │     if (![b]passwordEncoder.matches(credentials.getPassword(), user.getHash())) {
   43 │       throw new AuthException("Invalid password");
   44 │     }
   45 │     if (user.isMfaEnabled()) {
   46 │       [c]mfaService.verify(user, credentials.getMfaCode());
   47 │     }
   48 │     return user;
   49 │   }
```

**사용자 인증 로직. 이메일로 사용자를 찾고, 비밀번호 검증 후 MFA가 활성화되어 있으면 추가 검증합니다.**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[0] back | [a] userRepository.findByEmail | [b] passwordEncoder.matches | [c] mfaService.verify | [q] quit
```

## 설치

```bash
# 마켓플레이스 추가
/plugin marketplace add gnoyes-mik/code-tour

# 플러그인 설치
/plugin install code-tour@gnoyes-mik-code-tour
```

## 빠른 시작

```bash
# 기능으로 투어 시작
/tour 로그인 기능

# 특정 위치에서 시작
/tour src/auth/AuthService.java:45

# 자연어로 요청
결제 흐름을 코드 투어로 보여줘
인증이 어떻게 동작하는지 알려줘
```

## 기능

### 미니맵 네비게이션
코드 흐름에서 현재 위치를 확인:
- 엔트리포인트부터 현재까지의 경로 표시
- 현재 위치는 `←`로 표시
- 하위 함수들을 드릴 옵션으로 표시
- 최대 4개까지 표시, 나머지는 축약

### 트리 탐색
트리 구조처럼 코드를 탐색합니다:
- `[a]`, `[b]`, `[c]`... - 표시된 함수로 드릴다운
- `[0]` - 상위 스코프로 돌아가기
- `[q]` - 투어 종료
- 26개 초과 시 `[aa]`, `[ab]`, `[ac]`... (엑셀 방식)

### 드릴다운
함수 안으로 들어가기:
- 모든 호출 가능한 함수가 `[a]`, `[b]`, `[c]`...로 표시됩니다
- 알파벳을 입력하면 해당 함수의 구현을 볼 수 있습니다
- 미니맵으로 현재 경로를 항상 확인할 수 있습니다

### 자연어 질문
투어 중 언제든 질문할 수 있습니다:
- "이 함수는 뭘 반환해?"
- "이 함수를 호출하는 곳은?"
- "여기서 실패하면 어떻게 돼?"

### IDE 연동
`파일:123` 링크를 클릭하면 연결된 IDE에서 해당 위치가 열립니다.

### 스마트 처리
- **분기점**: 모든 분기가 드릴 옵션으로 표시
- **외부 라이브러리**: 문서 보기 또는 돌아가기 옵션
- **순환 참조**: 감지 및 경고
- **전체 함수**: 함수 전체를 보여줌 (잘리지 않음)

## 트리거 표현

다음 표현을 사용하면 skill이 활성화됩니다:

**한국어:**
- "X 흐름 보여줘"
- "X 흐름 알려줘"
- "X 코드 설명해줘"
- "X는 어떻게 동작해?"
- "코드 투어로 보여줘"

**영어:**
- "show me the flow of X"
- "walk me through X"
- "code tour of X"
- "explain how X works"
- "how does this work?"

## 명령어

| 명령어 | 설명 |
|--------|------|
| `/tour <대상>` | 대상에서 투어 시작 (파일:, 함수, 또는 기능) |
| `/tour` | 시작점 선택 프롬프트 표시 |

## 네비게이션

| 입력 | 동작 |
|------|------|
| `0` | 상위 함수로 돌아가기 |
| `a`, `b`, `c`... | 해당 함수로 드릴다운 |
| `q` | 투어 종료 |

## 기여하기

기여를 환영합니다! Pull Request를 자유롭게 제출해주세요.

## 라이선스

MIT
