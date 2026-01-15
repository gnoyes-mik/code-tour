---
name: code-tour
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
**src/controllers/AuthController.java#L25**

```java
   23 │ @PostMapping("/login")
   24 │ public Response login(@RequestBody Creds creds) {
   25 │   User user = [a]authService.authenticate(creds);
   26 │   return [b]tokenService.generate(user);
   27 │ }
```

**Entry point for login. Authenticates user and generates JWT token.**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[0] back | [a] authService.authenticate | [b] tokenService.generate | [q] quit
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
2. **Show entire function** - Don't truncate, show full function body
3. **Mark ALL callables** - Every function call should be marked [a], [b], [c]...
4. **Handle branches** - Show all branches as drill options
5. **Maintain stack** - Track drill-down path for back navigation
6. **Answer questions** - Respond to natural language queries in context
7. **Pre-read** - Pre-read drillable functions for fast navigation
8. **User's language** - Write explanations in user's language
9. **No emojis** - Keep output clean and professional
