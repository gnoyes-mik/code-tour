---
name: tour
description: Use when user wants to explore code flow interactively. Triggers on phrases like "show me the flow", "code tour", "walk me through", "explain the code", "코드 투어", "흐름 보여줘", "흐름 알려줘", "코드 설명해줘"
---

# Code Tour Skill

This skill provides interactive code exploration by drilling into function calls, like navigating a tree structure.

## When to Use

Activate this skill when user says:
- "show me the flow of X"
- "walk me through X"
- "code tour of X"
- "explain how X works"
- "X 흐름 보여줘"
- "X 흐름 알려줘"
- "코드 투어로 보여줘"
- "X 코드 설명해줘"
- "X는 어떻게 동작해?"
- After implementing a feature, to show what was built
- When explaining complex code paths
- When user asks "how does this work?"

## How to Execute

Use the `/tour` command from the code-tour plugin:

```
/code-tour:tour {target}
```

Or invoke the tour behavior directly following the tour.md command specification.

## Core Concept

This is NOT a step-by-step debugger. It's **tree navigation**:
- Show a function with callable functions marked as [a], [b], [c]...
- User drills into a function to see its implementation
- User can go back to parent scope with [0]

## Display Format

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
   42 │     if (user == null) {
   43 │       throw new AuthException("User not found");
   44 │     }
   45 │     if (![b]passwordEncoder.matches(credentials.getPassword(), user.getHash())) {
   46 │       throw new AuthException("Invalid password");
   47 │     }
   48 │     if (user.isMfaEnabled()) {
   49 │       [c]mfaService.verify(user, credentials.getMfaCode());
   50 │     }
   51 │     return user;
   52 │   }
```

**사용자 인증 로직. 이메일로 사용자를 찾고, 비밀번호 검증 후 MFA가 활성화되어 있으면 추가 검증합니다.**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[0] back | [a] userRepository.findByEmail | [b] passwordEncoder.matches | [c] mfaService.verify | [q] quit
```

## Integration with Other Skills

### After Implementation
When completing a feature implementation, offer a code tour:

```
Implementation complete! Would you like a code tour of what was built?

[a] Yes, show me the code
[b] No, I'm good
```

If yes, start tour from the main entry point of the implemented feature.

### During Code Review
When reviewing code, offer to tour through changes:

```
I see changes in 5 files. Want to explore the code?
```

### During Debugging
When helping debug, offer to tour through the problematic path:

```
The error originates here. Want to explore the call stack?
```

## Key Behaviors

1. **Start from entry point** - Find the logical starting point for the feature
2. **Show minimap** - Always display tree showing current position and path
3. **Show entire function** - Don't truncate, show full function body
4. **Mark ALL callables** - Every function call should be marked [a], [b], [c]...
5. **Excel-style overflow** - If more than 26 options, use [aa], [ab], [ac]...
6. **Minimap limit** - Show max 4 children in minimap, rest as `... +N more`
7. **Full method names** - Use `authService.authenticate()` not just `authenticate()`
8. **Handle branches** - Show all branches as drill options
9. **Maintain stack** - Track drill-down path for back navigation
10. **Answer questions** - Respond to natural language queries in context
11. **Pre-read** - Pre-read drillable functions for fast navigation
12. **User's language** - Write explanations in user's language
13. **No emojis** - Keep output clean and professional
