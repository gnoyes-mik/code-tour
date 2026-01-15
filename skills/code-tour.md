---
name: code-tour
description: Use when user wants to explore code flow interactively. Triggers on phrases like "show me the flow", "code tour", "walk me through", "explain the code", "코드 투어", "흐름 보여줘", "흐름 알려줘", "코드 설명해줘"
---

# Code Tour Skill

This skill provides interactive code exploration, like stepping through a debugger.

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

## Integration with Other Skills

### After Implementation
When completing a feature implementation, offer a code tour:

```
Implementation complete! Would you like a code tour of what was built?

[a] Yes, show me the flow
[b] No, I'm good
```

If yes, start tour from the main entry point of the implemented feature.

### During Code Review
When reviewing code, offer to tour through changes:

```
I see changes in 5 files. Want to tour through the execution flow?
```

### During Debugging
When helping debug, offer to tour through the problematic path:

```
The error originates here. Want to tour through the call stack?
```

## Display Format

Follow the exact format specified in the tour.md command:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
**[1/4] src/controllers/AuthController.java#L25**

```java
   23 │ @PostMapping("/login")
   24 │ public Response login(@RequestBody Creds creds) {
 > 25 │   User user = [a]authService.authenticate(creds);
   26 │   return [b]tokenService.generate(user);
   27 │ }
```

**Entry point for login. Authenticates user and generates JWT token.**

```
  [a] authService.authenticate()    [b] tokenService.generate()
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  1 prev | 2 next | 3 drill | 4 quit
```

## Key Behaviors

1. **Start from entry point** - Find the logical starting point for the feature
2. **Follow execution flow** - Step through code as it would execute
3. **Show entire function** - Don't truncate, show full function body
4. **Mark callable functions** - Use [a], [b] etc. for drill-down options
5. **Handle branches** - Let user choose path at if/switch statements
6. **Track depth** - Manage drill-down stack properly
7. **Answer questions** - Respond to natural language queries in context
8. **Pre-read** - Pre-read next/prev steps for fast navigation
9. **User's language** - Write explanations in user's language
10. **No emojis** - Keep output clean and professional
