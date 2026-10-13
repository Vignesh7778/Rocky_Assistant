# ROCKY
## Technical Requirements Specification

**Version:** 1.0  
**Status:** Master Technical Specification  
**Architecture:** Cross-Platform + AI + Native OS Integration  
**Primary Framework:** Flutter  
**Backend:** FastAPI  
**Development Constraint:** 8 GB RAM  
**Cost Strategy:** Free-first / Open-source-first

---

# 1. Technical Architecture

Rocky shall use a layered architecture.

```text
┌──────────────────────────────────────────┐
│              FLUTTER CLIENT              │
│              Dart / UI Layer             │
└────────────────────┬─────────────────────┘
                     │
              REST / WebSocket
                     │
┌────────────────────▼─────────────────────┐
│              FASTAPI SERVER              │
│              Python Backend              │
├──────────────────────────────────────────┤
│ Conversation │ Intent │ Tools │ Memory  │
│ AI Provider  │ Security │ Permissions   │
└────────────────────┬─────────────────────┘
                     │
              Tool Execution Layer
                     │
       ┌─────────────┼──────────────┐
       │             │              │
    Android         iOS          Desktop
    Kotlin          Swift        Native APIs
```

---

# 2. Technology Stack

## 2.1 Frontend

### Flutter

Purpose:

- Cross-platform UI
- Application state
- Navigation
- Voice interface
- Chat interface
- Settings
- Permission UI
- Tool status
- History

Language:

```text
Dart
```

---

# 3. Mobile Native Layer

## Android

Language:

```text
Kotlin
```

Responsibilities:

- Android permissions
- Application launching
- Phone integration
- Usage statistics
- Notifications
- Device APIs
- Native system actions
- Android-specific assistant features

Communication:

```text
Flutter
   ↓
MethodChannel / EventChannel
   ↓
Kotlin
   ↓
Android APIs
```

---

# 4. iOS Native Layer

Language:

```text
Swift
```

Responsibilities:

- iOS-specific integrations
- App Intents
- Siri integration
- Shortcuts
- Supported system actions
- Native permissions

Communication:

```text
Flutter
   ↓
MethodChannel
   ↓
Swift
   ↓
iOS APIs
```

Rocky must never attempt to bypass Apple's platform restrictions.

---

# 5. Desktop Layer

## Windows

Use:

- Flutter desktop
- Windows native APIs
- Native helper components where required

## macOS

Use:

- Flutter desktop
- Swift/Objective-C where required

## Linux

Use:

- Flutter desktop
- Linux native APIs/tools

---

# 6. Backend

## Python

Python will be used for:

- AI orchestration
- Intent processing
- Tool management
- Memory management
- Web services
- API communication
- Security policies
- Agent orchestration

---

# 7. FastAPI

FastAPI will provide:

- REST APIs
- WebSocket communication
- Authentication endpoints if needed
- AI endpoints
- Tool endpoints
- Memory endpoints
- Health checks

---

# 8. AI Provider Architecture

Rocky must use an abstraction.

```python
class AIProvider:
    def generate_response():
        pass

    def generate_tool_call():
        pass
```

Implementations:

```text
AIProvider
│
├── CloudAIProvider
├── AlternativeAIProvider
└── LocalAIProvider
```

The rest of Rocky must communicate with the interface rather than directly with a vendor SDK.

---

# 9. Cloud AI Strategy

The first implementation should use a cloud provider with a free/low-cost development option where available.

The provider must be configurable through environment variables.

Example:

```text
AI_PROVIDER=
AI_MODEL=
AI_API_KEY=
```

Never hardcode API keys.

---

# 10. Local AI Strategy

Local AI is optional.

The architecture may later support:

```text
Ollama
llama.cpp
other local inference engines
```

Local AI must NOT be a mandatory dependency for Rocky v1 because the primary development machine has 8 GB RAM.

---

# 11. Speech-to-Text

Rocky must use a provider abstraction for speech recognition.

```text
SpeechProvider
│
├── PlatformSpeechProvider
├── CloudSpeechProvider
└── LocalSpeechProvider
```

Possible local implementation:

```text
Whisper / whisper.cpp
```

The application must be able to change speech providers without redesigning the voice system.

---

# 12. Text-to-Speech

Use:

```text
TTSProvider
```

Implementations may include:

- Platform TTS
- Local TTS
- Cloud TTS

Initial implementation should prefer platform TTS where practical.

Future local option:

```text
Piper
```

---

# 13. Wake Word

Rocky shall have a dedicated wake-word interface.

```text
WakeWordDetector
```

Target phrase:

```text
Hey Rocky
```

Possible implementation:

```text
openWakeWord
```

Wake-word detection should preferably run locally.

---

# 14. Voice State Machine

Rocky voice system:

```text
IDLE
 ↓
LISTENING_FOR_WAKE_WORD
 ↓
WAKE_WORD_DETECTED
 ↓
CAPTURING_COMMAND
 ↓
TRANSCRIBING
 ↓
PROCESSING
 ↓
EXECUTING
 ↓
RESPONDING
 ↓
IDLE
```

Possible error state:

```text
ERROR
 ↓
RECOVERY
 ↓
IDLE
```

---

# 15. Command Router

The Command Router is responsible for determining whether a request is:

```text
LOCAL
AI_INTENT
AI_AGENT
```

Example:

```text
"Turn on flashlight"
        ↓
LOCAL

"Find my resume"
        ↓
AI_INTENT

"Prepare my laptop for Java study"
        ↓
AI_AGENT
```

---

# 16. Intent Schema

AI must return structured data rather than arbitrary executable code.

Example:

```json
{
  "intent": "OPEN_APP",
  "parameters": {
    "app_name": "Chrome"
  },
  "risk": "LOW"
}
```

For multiple actions:

```json
{
  "intent": "MULTI_ACTION",
  "actions": [
    {
      "intent": "ENABLE_DND"
    },
    {
      "intent": "OPEN_APP",
      "parameters": {
        "app_name": "VS Code"
      }
    }
  ]
}
```

---

# 17. Tool Architecture

Every executable capability must be represented as a tool.

Example:

```text
Tool
├── name
├── description
├── parameters
├── permission
├── risk_level
├── supported_platforms
└── execute()
```

Example:

```text
open_app
call_contact
send_message
get_battery
get_app_usage
set_timer
create_note
search_file
web_search
run_command
```

---

# 18. Tool Registry

Tools shall be registered centrally.

```text
ToolRegistry
│
├── PhoneTools
├── AppTools
├── DeviceTools
├── FileTools
├── WebTools
├── ProductivityTools
└── DeveloperTools
```

The AI can only request tools registered in the Tool Registry.

---

# 19. Platform Capability Matrix

Each tool must declare supported platforms.

Example:

```text
Tool: call_contact

Android: YES
iOS: LIMITED
Windows: NO
macOS: NO
Linux: NO
```

Rocky must check platform capability before execution.

---

# 20. Permission Manager

All sensitive tools must pass through:

```text
PermissionManager
```

Responsibilities:

- Check OS permission
- Check Rocky permission
- Check user preferences
- Check risk level
- Request confirmation
- Deny unauthorized actions

---

# 21. Risk Engine

Every tool must have a risk classification.

```text
LOW
MEDIUM
HIGH
CRITICAL
```

Example:

```text
get_battery → LOW
open_app → LOW
send_message → MEDIUM
delete_file → MEDIUM
delete_folder → HIGH
execute_shell_command → HIGH
financial_transaction → CRITICAL
```

---

# 22. Confirmation Engine

Example:

```text
Tool Request
     ↓
Risk Engine
     ↓
LOW
 └── Execute

MEDIUM
 └── Ask Confirmation

HIGH
 └── Strong Confirmation

CRITICAL
 └── Authentication + Confirmation
```

---

# 23. Developer Tool Security

Developer tools are powerful.

Rocky must NEVER allow the AI to execute arbitrary terminal commands without policy checks.

Example:

```text
User:
Delete the project.

Rocky:
This command will permanently delete project files.
Do you want to continue?
```

Dangerous commands should be blocked or require explicit confirmation.

---

# 24. File Security

File operations must support:

- Allowed directories
- Blocked directories
- Confirmation for deletion
- Recursive deletion protection
- File type validation
- Path traversal protection

Rocky must prevent:

```text
../../
```

style path traversal attacks.

---

# 25. Memory Architecture

Initial storage:

```text
SQLite
```

Tables:

```text
memories
memory_tags
memory_metadata
```

Memory lifecycle:

```text
User says "Remember..."
        ↓
Memory Request
        ↓
Permission
        ↓
Store
```

Deletion:

```text
Forget X
 ↓
Find Memory
 ↓
Delete
```

---

# 26. Database

Use:

```text
SQLite
```

Initial tables:

```text
users
conversations
messages
memories
tasks
reminders
settings
permissions
tool_logs
usage_logs
```

SQLite is preferred for MVP because it is lightweight and requires no database server.

---

# 27. API Architecture

Base:

```text
/api/v1
```

Endpoints:

```text
GET    /health

POST   /chat
POST   /voice/process

GET    /conversation
POST   /conversation

GET    /memory
POST   /memory
DELETE /memory/{id}

GET    /tools
POST   /tools/execute

GET    /settings
PUT    /settings

GET    /permissions
PUT    /permissions
```

The exact API surface may evolve during implementation but must remain versioned.

---

# 28. WebSocket

WebSocket should be used for:

- Streaming AI responses
- Voice processing status
- Tool execution status
- Real-time assistant state

Example:

```text
CONNECT
 ↓
LISTENING
 ↓
PROCESSING
 ↓
THINKING
 ↓
EXECUTING
 ↓
RESPONDING
```

---

# 29. Authentication

MVP can operate as a local personal assistant without mandatory account registration.

If cloud synchronization is introduced later:

```text
Authentication
 ↓
Session
 ↓
User
 ↓
Encrypted cloud data
```

---

# 30. Secret Management

Never store:

```text
API_KEY
PASSWORD
TOKEN
```

inside source code.

Use:

```text
.env
```

for local development.

Example:

```text
AI_API_KEY=
AI_MODEL=
DATABASE_URL=
```

`.env` must be included in `.gitignore`.

---

# 31. Project Structure

Recommended structure:

```text
rocky/
│
├── frontend/
│   └── rocky_flutter/
│       ├── lib/
│       │   ├── core/
│       │   ├── models/
│       │   ├── services/
│       │   ├── providers/
│       │   ├── features/
│       │   ├── widgets/
│       │   └── main.dart
│       │
│       ├── android/
│       ├── ios/
│       ├── windows/
│       ├── macos/
│       └── linux/
│
├── backend/
│   └── rocky_api/
│       ├── app/
│       │   ├── api/
│       │   ├── core/
│       │   ├── models/
│       │   ├── schemas/
│       │   ├── services/
│       │   ├── tools/
│       │   ├── agents/
│       │   ├── ai/
│       │   ├── memory/
│       │   ├── security/
│       │   └── main.py
│       │
│       ├── tests/
│       ├── requirements.txt
│       └── .env
│
├── docs/
│   ├── PRD.md
│   ├── Technical_Requirements.md
│   └── Prompt.md
│
├── scripts/
├── tests/
├── .gitignore
└── README.md
```

---

# 32. Flutter Architecture

Use feature-based organization.

```text
lib/
├── core/
│   ├── constants/
│   ├── theme/
│   ├── routing/
│   ├── platform/
│   └── utils/
│
├── features/
│   ├── home/
│   ├── voice/
│   ├── chat/
│   ├── history/
│   ├── memory/
│   ├── tools/
│   ├── settings/
│   └── permissions/
│
├── services/
│   ├── api_service.dart
│   ├── voice_service.dart
│   ├── tts_service.dart
│   └── platform_service.dart
│
└── main.dart
```

---

# 33. Backend Architecture

```text
app/
├── api/
│   └── routes/
│
├── ai/
│   ├── base.py
│   ├── cloud_provider.py
│   └── local_provider.py
│
├── agents/
│   ├── planner.py
│   └── executor.py
│
├── tools/
│   ├── registry.py
│   ├── phone.py
│   ├── apps.py
│   ├── device.py
│   ├── files.py
│   ├── web.py
│   └── developer.py
│
├── memory/
│   └── manager.py
│
├── security/
│   ├── permissions.py
│   ├── risk.py
│   └── confirmation.py
│
├── models/
├── schemas/
├── services/
└── main.py
```

---

# 34. State Management

Flutter must use a structured state-management solution.

Recommended:

```text
Riverpod
```

Responsibilities:

- Application state
- Voice state
- Conversation state
- Settings
- Permissions
- Tool execution status

---

# 35. Networking

Flutter:

```text
Dio
```

Backend:

```text
FastAPI
```

Communication:

```text
REST + WebSocket
```

---

# 36. Logging

Use structured logs.

Levels:

```text
DEBUG
INFO
WARNING
ERROR
CRITICAL
```

Never log:

- Passwords
- API keys
- Authentication tokens
- Private message contents unnecessarily

---

# 37. Testing

Testing layers:

```text
Unit Tests
Integration Tests
API Tests
Tool Tests
Security Tests
Platform Tests
UI Tests
```

Minimum backend target:

- Intent tests
- Tool tests
- Permission tests
- Security tests
- API tests

---

# 38. Performance Requirements

Rocky must run development components comfortably on an 8 GB RAM machine.

Avoid mandatory:

- Local LLM
- Heavy containers
- Large local databases
- Multiple unnecessary background servers

Target architecture:

```text
Flutter
+
FastAPI
+
SQLite
+
Cloud AI
```

---

# 39. Error Handling

Backend errors must return structured responses.

Example:

```json
{
  "success": false,
  "error_code": "PERMISSION_DENIED",
  "message": "Microphone permission is required."
}
```

Flutter must convert technical errors into user-friendly messages.

---

# 40. Security Requirements

Mandatory:

- Input validation
- Output validation
- Tool allowlisting
- Permission checks
- Risk classification
- Confirmation
- API key protection
- Secure storage
- Path validation
- Command restrictions
- Rate limiting where applicable
- Audit logs
- Safe error messages

---

# 41. AI Security

The AI must never be treated as trusted code.

AI output is untrusted input.

Therefore:

```text
AI output
 ↓
Schema validation
 ↓
Tool validation
 ↓
Permission validation
 ↓
Risk validation
 ↓
Execution
```

Never:

```text
AI → os.system()
```

directly.

---

# 42. Prompt Injection Protection

Rocky must assume external content can contain malicious instructions.

Examples:

- Websites
- Emails
- Documents
- PDFs
- Web pages

External content must never automatically override Rocky's system instructions or tool policies.

---

# 43. Privacy

Rocky should follow privacy-by-design.

Requirements:

- Explain microphone usage.
- Explain screen capture.
- Allow conversation deletion.
- Allow memory deletion.
- Avoid unnecessary cloud transmission.
- Minimize stored information.
- Keep API credentials secure.

---

# 44. Dependency Policy

Dependencies must be:

- Actively maintained where practical
- Free/open-source where practical
- Compatible with target platforms
- Licensed appropriately
- Avoid unnecessary dependencies

Before adding a dependency, determine whether the feature can be implemented with existing tools.

---

# 45. Development Phases

## Phase 0

Project setup.

```text
Flutter
FastAPI
Git
SQLite
Documentation
```

## Phase 1

Voice + AI conversation.

## Phase 2

Tool architecture.

## Phase 3

Android integration.

## Phase 4

Security and permissions.

## Phase 5

Productivity.

## Phase 6

Desktop.

## Phase 7

iOS.

## Phase 8

Advanced agents and memory.

---

# 46. Definition of Done

A feature is complete only when:

- UI implemented
- Backend implemented where required
- Platform integration implemented
- Permission handling implemented
- Error handling implemented
- Tests written
- Security reviewed
- Documentation updated
- Unsupported platforms handled
- No secrets committed

---

# 47. Technical Golden Rule

Rocky must always follow:

```text
UNDERSTAND
    ↓
VALIDATE
    ↓
AUTHORIZE
    ↓
EXECUTE
    ↓
VERIFY
    ↓
RESPOND
```

Never:

```text
UNDERSTAND
    ↓
EXECUTE DIRECTLY
```

End of Technical Requirements.