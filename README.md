# Code Tour

Interactive code explorer - navigate your codebase like a debugger.

[한국어](./README.ko.md)

## What is Code Tour?

Code Tour is a Claude Code plugin that lets you step through code execution flow interactively. Instead of jumping between files manually, let Claude guide you through the code path step-by-step.

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

**Parses JWT token and retrieves user from DB.**

```
  [a] jwtParser.parse()    [b] userRepo.findById()
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  1 prev | 2 next | 3 drill | 4 quit
```

## Installation

```bash
# Add marketplace
/plugin marketplace add gnoyes-mik/code-tour

# Install plugin
/plugin install code-tour@gnoyes-mik-code-tour
```

## Quick Start

```bash
# Start a tour by feature
/tour login feature

# Start from specific location
/tour src/auth/AuthService.java#L45

# Natural language
Show me the payment flow as a code tour
Explain how authentication works
```

## Features

### Step-by-Step Navigation
Move through code flow with simple commands:
- `1` - Previous step
- `2` - Next step
- `3` - Drill down into a function
- `4` - Quit tour

### Drill Down
Dive into function calls:
- Functions are marked with `[a]`, `[b]`, etc.
- Type the letter or function name to drill in
- Press `1` at drill depth to return to parent scope

### Natural Language Queries
Ask questions during the tour:
- "What does this function return?"
- "Who calls this function?"
- "What happens if this fails?"

### IDE Integration
Click on `file#L123` links to open in your connected IDE.

### Smart Handling
- **Branch points**: Choose which path to follow at if/switch
- **External libraries**: Option to view docs or skip
- **Circular references**: Detection and warning
- **Entire function**: Shows full function body, not truncated

## Trigger Phrases

The skill activates on these phrases:

**English:**
- "show me the flow of X"
- "walk me through X"
- "code tour of X"
- "explain how X works"
- "how does this work?"

**Korean:**
- "X 흐름 보여줘"
- "X 흐름 알려줘"
- "X 코드 설명해줘"
- "X는 어떻게 동작해?"
- "코드 투어로 보여줘"

## Commands

| Command | Description |
|---------|-------------|
| `/tour <target>` | Start tour from target (file#L, function, or feature) |
| `/tour` | Interactive prompt to choose starting point |

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

MIT
