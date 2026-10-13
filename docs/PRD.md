# ROCKY
## Personal AI Voice Assistant

**Document:** Product Requirements Document  
**Version:** 1.0  
**Status:** Master Specification  
**Project Type:** Cross-Platform AI Personal Assistant  
**Primary Platforms:** Android, iOS, Windows, macOS, Linux  
**Development Strategy:** Free-first / Open-source-first  
**UI Framework:** Flutter  
**Backend:** Python + FastAPI  

---

# 1. Product Overview

## 1.1 Product Name

**Rocky**

## 1.2 Product Concept

Rocky is a cross-platform personal AI voice assistant designed to provide natural voice interaction and controlled access to device capabilities, applications, information, files, productivity tools, web services, and developer tools.

The user should be able to interact with Rocky naturally using:

> "Hey Rocky..."

Rocky should understand the user's request, determine the required action, request permission or confirmation when necessary, execute the action through an appropriate tool, and provide a clear response.

---

# 2. Product Vision

The vision of Rocky is:

> **One intelligent assistant that provides a consistent AI experience across mobile and desktop platforms while respecting the security and capability limitations of each operating system.**

Rocky should not simply be a chatbot.

Rocky should combine:

- Voice assistant
- AI conversation
- OS integration
- Application control
- Productivity
- Web interaction
- File management
- Developer assistance
- Personal memory
- Security and permissions
- Multi-step AI agents

---

# 3. Core Principles

Rocky must follow these principles.

### 3.1 Cross-platform

The main application must use Flutter.

Supported targets:

- Android
- iOS
- Windows
- macOS
- Linux

### 3.2 Free-first

The project must prioritize:

- Free software
- Open-source libraries
- Local processing where practical
- Free API tiers where available
- No mandatory paid AI subscription

Paid cloud services must never be hardcoded as mandatory dependencies.

### 3.3 AI-provider independent

Rocky must not depend permanently on one AI provider.

The architecture must support:

```text
AI Provider Interface
        |
        +-- Cloud Provider
        |
        +-- Alternative Provider
        |
        +-- Local Provider
```

A future local AI provider such as Ollama can be added without redesigning Rocky.

### 3.4 Security first

Rocky must never allow an AI model to directly execute unrestricted operating-system commands.

Every action must pass through:

```text
AI
 ↓
Intent
 ↓
Tool
 ↓
Permission
 ↓
Risk Check
 ↓
Confirmation if required
 ↓
Execution
```

### 3.5 Graceful degradation

Because operating systems provide different capabilities, Rocky must not assume that every feature works everywhere.

Example:

```text
Android → Full mobile capability
iOS → Apple-permitted capability
Windows → Desktop capability
macOS → macOS-permitted capability
Linux → Linux capability
```

Unsupported features must return a clear explanation instead of crashing.

---

# 4. Target Users

## Primary User

A student/developer who wants:

- Voice-controlled device interaction
- AI assistance
- Productivity automation
- Developer assistance
- Cross-platform access
- Personal memory
- Smart device control

## Secondary Users

- Students
- Developers
- Professionals
- Power users
- Users interested in personal AI assistants

---

# 5. Product Goals

Rocky must:

1. Understand natural language.
2. Support voice interaction.
3. Respond using speech.
4. Execute supported device actions.
5. Open applications.
6. Manage productivity tasks.
7. Search the web.
8. Manage supported files.
9. Provide developer assistance.
10. Remember user-approved information.
11. Protect sensitive actions.
12. Work across multiple platforms.
13. Provide a consistent user experience.
14. Minimize unnecessary AI API usage.
15. Remain usable on an 8 GB RAM development machine.

---

# 6. Non-Goals

Rocky v1 must NOT attempt to:

- Replace the operating system.
- Bypass OS security restrictions.
- Bypass application permissions.
- Bypass device security.
- Access private information without permission.
- Execute unrestricted shell commands automatically.
- Perform financial transactions without explicit confirmation.
- Provide unrestricted background surveillance.
- Circumvent iOS security restrictions.
- Circumvent Android permission systems.

---

# 7. User Interaction

## 7.1 Wake Word

Primary wake phrase:

> Hey Rocky

Flow:

```text
Microphone
    ↓
Wake Word Detection
    ↓
"Hey Rocky"
    ↓
Voice Capture
    ↓
Speech-to-Text
```

Wake-word functionality must be designed so that it does not require continuous transmission of microphone audio to a remote server.

---

# 8. Voice Pipeline

```text
User Voice
    ↓
Wake Word
    ↓
Speech-to-Text
    ↓
Command Router
    ↓
Local Command OR AI
    ↓
Intent
    ↓
Tool
    ↓
Permission Check
    ↓
Execution
    ↓
Response
    ↓
Text-to-Speech
```

---

# 9. Command Processing Strategy

Rocky must use three command levels.

## Level 1 — Local Commands

Simple deterministic commands should not require an AI API.

Examples:

- Open application
- Battery status
- Flashlight
- Volume
- Timer
- Supported settings
- Basic device information

Flow:

```text
Speech
 ↓
STT
 ↓
Command Router
 ↓
Local Tool
```

Advantages:

- Fast
- Free
- Low latency
- No unnecessary API usage

---

## Level 2 — AI Intent Commands

Use AI when natural-language interpretation is useful.

Examples:

> Find my resume.

> Send Arun a message saying I'll be late.

> Open my Pokedex project.

Flow:

```text
Speech
 ↓
STT
 ↓
AI
 ↓
Structured Intent
 ↓
Tool
```

---

## Level 3 — AI Agent Commands

Used for multi-step tasks.

Example:

> Rocky, I'm going to study Java for two hours. Turn on Do Not Disturb, open VS Code, open my Java project and start a timer.

Rocky creates a plan:

```text
1. Enable Do Not Disturb
2. Open VS Code
3. Open Java project
4. Start 2-hour timer
```

Each action is independently validated.

---

# 10. Functional Requirements

# FR-01 Voice Interaction

Rocky shall:

- Accept microphone input.
- Detect wake word where supported.
- Convert speech to text.
- Process voice commands.
- Generate spoken responses.
- Support conversational follow-up questions.
- Allow interruption of spoken responses where technically supported.

---

# FR-02 AI Conversation

Rocky shall support:

- General questions
- Explanations
- Summaries
- Brainstorming
- Context-aware conversation
- Follow-up questions
- AI-assisted planning

---

# FR-03 Application Control

Rocky shall support supported operations such as:

- List applications
- Open applications
- Launch supported application actions
- Open URLs
- Open system settings pages where supported

Example:

> "Hey Rocky, open Chrome."

---

# FR-04 Phone Features

On platforms that permit these capabilities, Rocky may support:

- Call contact
- Call number
- Open dialer
- Recent calls
- Supported call actions

Sensitive communication actions must respect platform permissions.

---

# FR-05 Messaging

Where supported, Rocky may:

- Compose messages
- Send supported messages
- Open messaging applications
- Prepare replies

Sending a message should normally require confirmation unless the user explicitly enables trusted automation.

Example:

```text
Rocky:
I prepared this message for Arun:

"I'll reach in 20 minutes."

Send it?
```

---

# FR-06 App Usage

Where the operating system provides the required APIs, Rocky shall provide:

- Total usage
- Per-application usage
- Most-used applications
- Usage history
- Application launch counts where available

Example:

> "How long did I use Instagram today?"

---

# FR-07 Device Information

Rocky shall provide supported information including:

- Battery percentage
- Charging state
- Device information
- Network status
- Storage information
- Available system information

---

# FR-08 Device Controls

Where supported:

- Flashlight
- Volume
- Wi-Fi-related settings/actions
- Bluetooth-related settings/actions
- Do Not Disturb
- Battery saver
- Supported system settings

Unsupported operations must be clearly reported.

---

# FR-09 Productivity

Rocky shall support:

### Timer

> Set a timer for 20 minutes.

### Reminder

> Remind me to study Java at 7 PM.

### Notes

> Remember this note: revise binary trees.

### Tasks

> Add Java interview preparation to my tasks.

### Calendar

Where supported:

- Create event
- Read events
- Schedule reminders

---

# FR-10 Web Assistance

Rocky shall support:

- Web search
- Search result processing
- Information retrieval
- Summarization
- Website opening
- URL handling

Web functionality must use a provider abstraction.

---

# FR-11 File Management

Where platform permissions allow:

- Search files
- Open files
- Create files
- Rename files
- Move files
- Copy files
- Delete files
- Read supported documents
- Summarize supported documents

Deletion must have protection against accidental destructive operations.

---

# FR-12 Developer Assistant

Rocky shall provide developer-focused capabilities.

Examples:

> Open VS Code.

> Open my Pokedex project.

> Check Git status.

> Explain this error.

> Run the backend.

> Start the FastAPI server.

> Search this project for a function.

Developer tools must be protected by permission and confirmation policies.

---

# FR-13 Desktop Automation

Desktop versions may support:

- Application launching
- File operations
- Terminal
- Developer tools
- Screenshots
- Window operations where permitted
- System information
- Screen understanding where permitted

---

# FR-14 Screen Understanding

Where supported and explicitly authorized, Rocky may analyze visible screen content.

Examples:

> "Rocky, explain this error."

> "What is displayed on my screen?"

Screen capture must be clearly controlled by the user.

---

# FR-15 Memory

Rocky shall support user-controlled memory.

User commands:

- Remember this.
- What do you remember?
- Forget this.
- Delete my memories.
- Delete all memories.

Memory must never be silently created for sensitive information.

---

# FR-16 Personalization

Rocky may remember:

- User preferences
- Projects
- Preferred applications
- Non-sensitive habits
- Frequently used commands
- User-approved notes

---

# FR-17 Security

Rocky shall include:

- Permission manager
- Action risk classification
- Confirmation system
- Secure credential storage
- API key protection
- Local data protection
- Audit logging for sensitive actions

---

# 11. Risk Classification

## LOW

Examples:

- Open application
- Battery status
- Timer
- Weather search
- General questions

No confirmation normally required.

## MEDIUM

Examples:

- Send message
- Delete a file
- Modify settings
- Create calendar event

Confirmation normally required.

## HIGH

Examples:

- Delete many files
- Execute destructive commands
- Change security settings
- Financial actions
- Credential changes

Strong confirmation/authentication required.

---

# 12. Conversation Experience

Rocky should communicate naturally.

Example:

User:

> Hey Rocky.

Rocky:

> Yes?

User:

> Open VS Code.

Rocky:

> Opening VS Code.

The assistant should avoid unnecessary explanations for simple commands.

For complex requests, Rocky should explain what it is going to do.

---

# 13. Error Handling

If an action cannot be performed:

Bad:

> Error 403.

Good:

> I can't change that setting on iOS because the operating system doesn't allow third-party apps to control it directly.

---

# 14. Offline Mode

Rocky should continue supporting basic functions when the internet is unavailable.

Offline-capable functionality may include:

- Application launching
- Device information
- Timers
- Basic settings
- Local notes
- Local memory
- Previously available commands

AI-dependent functionality should gracefully report when internet connectivity is required.

---

# 15. Cross-Platform Strategy

## Android

Priority platform for deep device integration.

Potential features:

- Calls
- App usage
- Notifications
- Device controls
- Applications
- System information

## iOS

Use supported Apple frameworks.

Features depend on:

- App Intents
- Shortcuts
- Siri integration
- Apple platform permissions

Rocky must not attempt to bypass iOS restrictions.

## Windows

Priority desktop platform.

Potential features:

- Application control
- File system
- Terminal
- Developer tools
- Screen understanding
- System controls

## macOS

Use Flutter plus native macOS APIs and permissions.

## Linux

Use Flutter plus Linux-compatible native integrations.

---

# 16. User Interface

Rocky UI must be:

- Modern
- Minimal
- Voice-first
- Responsive
- Dark-mode friendly
- Accessible
- Fast

Main screens:

```text
Home
Chat
Voice
History
Memory
Tools
Usage
Settings
Permissions
About
```

---

# 17. Main Home Screen

The home screen should contain:

```text
             ROCKY

        [ Voice Orb ]

        "Hey Rocky"

     Ask Rocky anything...

Recent commands
-----------------------
Open VS Code
Battery status
Study timer
```

---

# 18. Permission UI

Users must be able to see:

```text
Permissions

Microphone       ✓
Notifications    ✓
Contacts         ✓
Phone            ✓
Location         -
Files            ✓
Screen Access    -
```

The user can revoke access.

---

# 19. Settings

Settings should include:

### General

- Assistant name
- Wake word
- Language
- Voice
- Response style

### AI

- Provider
- API configuration
- Model
- AI usage
- Fallback provider

### Privacy

- Memory
- Conversation history
- Voice processing
- Screen analysis
- Data deletion

### Permissions

- Microphone
- Notifications
- Contacts
- Phone
- Files
- Location
- Screen access

### Developer

- Developer mode
- Tool logging
- Debug mode
- API logs

---

# 20. Privacy Principles

Rocky must follow:

1. Collect only required information.
2. Process locally whenever practical.
3. Explain why permissions are required.
4. Never silently access microphone or screen.
5. Allow users to delete their data.
6. Do not store API keys in source code.
7. Do not expose sensitive data to AI unnecessarily.

---

# 21. Performance Requirements

Rocky must be designed for development on:

```text
RAM: 8 GB
```

Therefore:

- No mandatory local LLM.
- No mandatory Docker.
- No mandatory Ollama.
- No unnecessary background processes.
- Lightweight local command processing.
- Cloud AI through provider abstraction.
- SQLite instead of a heavy database for MVP.

Target:

- UI should remain responsive.
- Basic commands should feel near-instant.
- AI response latency should depend primarily on network/provider response.

---

# 22. MVP Scope

The first release should contain:

### Voice

- Microphone
- Speech-to-text
- Text-to-speech
- Wake word architecture

### AI

- Cloud AI provider abstraction
- Conversation
- Intent detection
- Tool calling

### Android

- Open apps
- Battery
- Flashlight
- Volume
- Timer
- Basic device information
- App usage where permitted

### Productivity

- Notes
- Tasks
- Reminders
- Timer

### Security

- Permission manager
- Confirmation system
- Action risk classification

### UI

- Home
- Chat
- Voice
- History
- Settings
- Permissions

---

# 23. Future Features

Future releases may include:

- Multi-agent system
- Advanced memory
- Screen understanding
- Desktop automation
- Developer agent
- Git integration
- IDE integration
- Advanced notification management
- Smart routines
- Cross-device synchronization
- Offline AI
- Local LLM support
- Custom Rocky voice
- Advanced personalization

---

# 24. Success Criteria

Rocky v1 is successful when:

1. User can activate Rocky.
2. Rocky understands voice input.
3. Rocky can answer using AI.
4. Rocky can execute supported tools.
5. Rocky can perform basic Android actions.
6. Rocky protects sensitive actions.
7. Rocky works on the developer's 8 GB laptop.
8. Architecture supports iOS and desktop.
9. AI provider can be changed without major code changes.
10. No core functionality requires a paid service.

---

# 25. Product Definition

Rocky is:

> **A secure, cross-platform, voice-first personal AI assistant that combines natural language intelligence with controlled operating-system tools and user-approved automation.**

End of PRD.