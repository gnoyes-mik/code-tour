---
name: code-tour
description: Use when user wants to explore code flow interactively, or after implementation to show what was built. Triggers on phrases like "show me the flow", "code tour", "walk me through", "코드 투어", "흐름 보여줘"
---

# Code Tour Skill

This skill provides interactive code exploration, like stepping through a debugger.

## When to Use

Activate this skill when:
- User says "show me the flow of X"
- User says "walk me through X"
- User says "code tour of X"
- User says "X 흐름 보여줘"
- User says "코드 투어로 보여줘"
- After implementing a feature, to show what was built
- When explaining complex code paths

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

[1] Yes, show me the flow
[2] No, I'm good
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
┌─────────────────────────────────────────────────┐
│ [step/total] file:line                          │
│ ───────────────────────────────────────────────│
│  (code with [a], [b] markers)                   │
│                                                 │
│  (brief explanation)                            │
├─────────────────────────────────────────────────┤
│ [1] ← prev  [2] next →  [3] drill  [q] quit    │
└─────────────────────────────────────────────────┘
```

## Key Behaviors

1. **Start from entry point** - Find the logical starting point for the feature
2. **Follow execution flow** - Step through code as it would execute
3. **Mark callable functions** - Use [a], [b] etc. for drill-down options
4. **Handle branches** - Let user choose path at if/switch statements
5. **Track depth** - Manage drill-down stack properly
6. **Answer questions** - Respond to natural language queries in context

## Example Invocation

User: "I just implemented user authentication. Show me the flow."

Response:
```
Starting code tour of user authentication flow...

┌─────────────────────────────────────────────────┐
│ [1/4] src/controllers/AuthController.java:25    │
│ ───────────────────────────────────────────────│
│  23 │ @PostMapping("/login")                    │
│  24 │ public Response login(@RequestBody Creds) │
│  25 │ →   User user = [a]authService.           │
│  26 │         authenticate(creds);              │
│  27 │   return [b]tokenService.generate(user);  │
│  28 │ }                                         │
│                                                 │
│ Entry point for login. Authenticates user and  │
│ generates JWT token.                            │
├─────────────────────────────────────────────────┤
│ [1] ← prev  [2] next →  [3] drill  [q] quit    │
└─────────────────────────────────────────────────┘
```
