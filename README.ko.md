# Code Tour

디버거처럼 코드를 탐색하는 인터랙티브 도구

[English](./README.md)

## Code Tour란?

Code Tour는 코드 실행 흐름을 단계별로 탐색할 수 있는 Claude Code 플러그인입니다. 파일을 직접 찾아다니는 대신, Claude가 코드 경로를 따라 안내해줍니다.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
**[2/5] src/auth/AuthService.java#L120**

```java
   119 │   public User validateToken(String token) {
 > 120 │     TokenPayload payload = [a]jwtParser.parse(token);
   121 │     return [b]userRepo.findById(payload.getUserId());
   122 │   }
```

**JWT 토큰을 파싱하고 DB에서 사용자를 조회합니다.**

```
  [a] jwtParser.parse()    [b] userRepo.findById()
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  1 prev | 2 next | 3 drill | 4 quit
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
/tour src/auth/AuthService.java#L45

# 자연어로 요청
결제 흐름을 코드 투어로 보여줘
인증이 어떻게 동작하는지 알려줘
```

## 기능

### 단계별 탐색
간단한 명령으로 코드 흐름을 따라갑니다:
- `1` - 이전 단계
- `2` - 다음 단계
- `3` - 함수 안으로 드릴다운
- `4` - 투어 종료

### 드릴다운
함수 안으로 들어가기:
- 호출되는 함수는 `[a]`, `[b]` 등으로 표시됩니다
- 알파벳이나 함수 이름을 입력해서 들어갈 수 있습니다
- 드릴다운 상태에서 `1`을 누르면 상위 스코프로 돌아갑니다

### 자연어 질문
투어 중 언제든 질문할 수 있습니다:
- "이 함수는 뭘 반환해?"
- "이 함수를 호출하는 곳은?"
- "여기서 실패하면 어떻게 돼?"

### IDE 연동
`파일#L123` 링크를 클릭하면 연결된 IDE에서 해당 위치가 열립니다.

### 스마트 처리
- **분기점**: if/switch에서 어느 경로를 따라갈지 선택
- **외부 라이브러리**: 문서 보기 또는 건너뛰기 옵션
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
| `/tour <대상>` | 대상에서 투어 시작 (파일#L, 함수, 또는 기능) |
| `/tour` | 시작점 선택 프롬프트 표시 |

## 기여하기

기여를 환영합니다! Pull Request를 자유롭게 제출해주세요.

## 라이선스

MIT
