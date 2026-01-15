# Code Tour

Interactive code explorer - navigate your codebase by drilling into functions.

[한국어](./README.ko.md)

## What is Code Tour?

Code Tour is a Claude Code plugin that lets you explore code by drilling into function calls, like navigating a tree structure. Instead of stepping through lines, you dive into functions to understand the code flow.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
**src/service/AuthService.java:42**

```
  login() - Login entry point
  └─ authService.authenticate() ← current
     ├─[a] userRepository.findByEmail() - Find user by email
     ├─[b] passwordEncoder.matches() - Validate password
     └─[c] mfaService.verify() - MFA verification
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

**Authenticates user by email, validates password, and performs MFA if enabled.**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[0] back | [a] userRepository.findByEmail | [b] passwordEncoder.matches | [c] mfaService.verify | [q] quit
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
/tour src/auth/AuthService.java:45

# Natural language
Show me the payment flow as a code tour
Explain how authentication works
```

## Features

### Minimap Navigation
See where you are in the code flow:
- Shows parent path from entry point
- Current position marked with `←`
- Children functions as drill options
- Max 4 children shown, rest collapsed

### Tree Navigation
Explore code like navigating a tree:
- `[a]`, `[b]`, `[c]`... - Drill into marked functions
- `[0]` - Go back to parent scope
- `[q]` - Quit tour
- More than 26 options? Uses `[aa]`, `[ab]`, `[ac]`... (Excel-style)

### Drill Down
Dive into function calls:
- ALL callable functions are marked with `[a]`, `[b]`, `[c]`...
- Type the letter to see that function's implementation
- Minimap always shows your current path

### Natural Language Queries
Ask questions during the tour:
- "What does this function return?"
- "Who calls this function?"
- "What happens if this fails?"

### IDE Integration
Click on `file:123` links to open in your connected IDE.

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
| `/tour <target>` | Start tour from target (file:, function, or feature) |
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
