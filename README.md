# Code Tour

Interactive code explorer - navigate your codebase like a debugger.

[한국어](./README.ko.md)

## What is Code Tour?

Code Tour is a Claude Code plugin that lets you step through code execution flow interactively. Instead of jumping between files manually, let Claude guide you through the code path step-by-step.

```
┌─────────────────────────────────────────────────┐
│ [2/5] src/auth/AuthService.java:120             │
│ ───────────────────────────────────────────────│
│  119 │   public User validateToken(String tok) {│
│  120 │ →   TokenPayload payload = [a]jwtParser. │
│  121 │         parse(token);                    │
│  122 │     return [b]userRepo.findById(         │
│  123 │         payload.getUserId());            │
│                                                 │
│ Parses JWT token and retrieves user from DB.   │
├─────────────────────────────────────────────────┤
│ [1] ← prev  [2] next →  [3] drill  [q] quit    │
└─────────────────────────────────────────────────┘
```

## Installation

```bash
# Add marketplace
/plugin marketplace add gnoyes/code-tour

# Install plugin
/plugin install code-tour@gnoyes-code-tour
```

## Quick Start

```bash
# Start a tour by feature
/tour login feature

# Start from specific location
/tour src/auth/AuthService.java:45

# Natural language
Show me the payment flow as a code tour
```

## Features

### Step-by-Step Navigation
Move through code flow with simple commands:
- `1` or `prev` - Previous step
- `2` or `next` - Next step
- `q` - Quit tour

### Drill Down
Dive into function calls:
- Functions are marked with `[a]`, `[b]`, etc.
- Type the letter or function name to drill in
- "prev" returns to parent scope

### Natural Language Queries
Ask questions during the tour:
- "What does this function return?"
- "Who calls this function?"
- "What happens if this fails?"

### IDE Integration
Click on `file:line` links to open in your connected IDE.

### Smart Handling
- **Branch points**: Choose which path to follow at if/switch
- **External libraries**: Option to view docs or skip
- **Circular references**: Detection and warning

## Examples

### Explore a Feature
```
> /tour user authentication

Starting code tour of user authentication...
[Shows AuthController.login() as entry point]
```

### After Implementation
```
> (after implementing a feature)
> Show me what I just built as a code tour

Starting tour from UserService.createUser()...
```

### Debug Investigation
```
> /tour the function that throws NullPointerException

Found potential location: OrderService.java:234
Starting tour...
```

## Commands

| Command | Description |
|---------|-------------|
| `/tour <target>` | Start tour from target (file:line, function, or feature) |
| `/tour` | Interactive prompt to choose starting point |

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

MIT
