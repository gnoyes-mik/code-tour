# Code Tour

Interactive code explorer - navigate your codebase by drilling into functions.

[한국어](./README.ko.md)

## What is Code Tour?

Code Tour is a Claude Code plugin that lets you explore code by drilling into function calls, like navigating a tree structure. Instead of stepping through lines, you dive into functions to understand the code flow.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
**src/auth/AuthService.java#L120**

```java
   119 │   public User validateToken(String token) {
   120 │     TokenPayload payload = [a]jwtParser.parse(token);
   121 │     User user = [b]userRepo.findById(payload.getUserId());
   122 │     [c]auditLog.record(user, "validated");
   123 │     return user;
   124 │   }
```

**Parses JWT token, retrieves user, and records audit log.**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[0] back | [a] jwtParser.parse | [b] userRepo.findById | [c] auditLog.record | [q] quit
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

### Tree Navigation
Explore code like navigating a tree:
- `[a]`, `[b]`, `[c]`... - Drill into marked functions
- `[0]` - Go back to parent scope
- `[q]` - Quit tour

### Drill Down
Dive into function calls:
- ALL callable functions are marked with `[a]`, `[b]`, `[c]`...
- Type the letter to see that function's implementation
- Breadcrumb shows your current path

### Natural Language Queries
Ask questions during the tour:
- "What does this function return?"
- "Who calls this function?"
- "What happens if this fails?"

### IDE Integration
Click on `file#L123` links to open in your connected IDE.

### Smart Handling
- **Branch points**: All branches shown as drill options
- **External libraries**: Option to view docs or go back
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

## Navigation

| Input | Action |
|-------|--------|
| `0` | Go back to parent function |
| `a`, `b`, `c`... | Drill into that function |
| `q` | Quit tour |

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

MIT
