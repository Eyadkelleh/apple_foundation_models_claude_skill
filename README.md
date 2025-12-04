# Apple Foundation Models Skill – README

## Overview

This repository contains comprehensive reference documentation and guidance for Apple's Foundation Models framework on iOS 26+ and macOS 26+. It serves as a complete skill guide for developers building on-device AI capabilities using Apple's on-device language models.

The goal of this project is to provide clear, organized documentation on how to:

- Integrate Apple's on-device language models using the FoundationModels framework
- Build and configure skills that leverage SystemLanguageModel for on-device inference
- Implement advanced patterns including guided generation, tool calling, and streaming responses
- Handle safety, localization, and performance considerations for on-device AI

## The Skill Covers

- **On-device LLM integration** using the FoundationModels framework
- **SystemLanguageModel** – Apple's on-device language model API
- **LanguageModelSession** – Managing conversations and model interactions
- **Guided Generation** – Generating structured Swift types from prompts
- **Tool Calling** – Extending model capabilities with custom tools and integrations
- **Prompting** – Crafting effective prompts and system instructions
- **Safety** – Implementing guardrails and handling sensitive content
- **Localization** – Supporting multilingual AI features
- **Performance** – Optimization patterns and best practices

## Requirements

- **Xcode 17** or later with Foundation Models support
- **iOS 26, iPadOS 26, macOS 26, or visionOS 26** (or later)
- Swift and SwiftUI basic knowledge (for implementation)
- Device or simulator that supports Apple Intelligence and the Foundation Models framework

## Installation

### Install as a Claude Code Skill

This skill is available in the Claude Code marketplace. To install it:

```
/plugin marketplace add Eyadkelleh/apple_foundation_models_claude_skill
```

Then install the skill:

```
/plugin install apple-foundation-models@apple-foundation-models-marketplace
```

Once installed, Claude Code will automatically suggest this skill when you're working with Apple Foundation Models.

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/Eyadkelleh/apple_foundation_models_claude_skill.git
cd apple_foundation_models_claude_skill
```

### 2. Explore the Documentation

The repository is organized into focused reference guides:

- **[Getting Started](references/getting_started.md)** – Initial setup and basic patterns
- **[Prompting](references/prompting.md)** – How to craft effective prompts and instructions
- **[Guided Generation](references/guided_generation.md)** – Generating structured outputs
- **[Tool Calling](references/tool_calling.md)** – Extending models with custom tools
- **[Safety](references/safety.md)** – Guardrails and safety considerations
- **[Localization](references/localization.md)** – Multilingual support
- **[API Reference](references/api_reference.md)** – Complete API documentation
- **[Advanced](references/advanced.md)** – Advanced patterns and use cases

### 3. Understand the Skill Metadata

Review [SKILL.md](SKILL.md) for a structured overview of all APIs and when to use this skill.

## How It Works

### Core Pattern

The Foundation Models framework provides a simple API for on-device inference:

```swift
// 1. Access the on-device language model
let model = SystemLanguageModel(useCase: .general)

// 2. Create a session with custom instructions
let instructions = Instructions("""
You are a helpful assistant that summarizes documents concisely.
Focus on key points and maintain accuracy.
""")
let session = LanguageModelSession(
    model: model,
    instructions: instructions
)

// 3. Send prompts and handle responses
let response = try await session.respond(to: Prompt("Summarize this text..."))
```

### Key Features

- **On-Device Processing**: All models run entirely on-device with no external API calls
- **Privacy-First**: User data never leaves the device
- **Zero Configuration**: No API keys or external service setup required
- **Streaming Support**: Real-time response streaming for responsive UIs
- **Structured Generation**: Generate Swift types directly with guided generation
- **Extensible**: Define custom tools for dynamic capabilities

## Project Structure

```
apple_foundation_models_claude_skill/
├── README.md                 # This file
├── SKILL.md                  # Skill metadata and API overview
└── references/               # Detailed documentation
    ├── index.md             # Documentation index
    ├── getting_started.md   # Getting started guide
    ├── prompting.md         # Prompting strategies
    ├── guided_generation.md # Structured output generation
    ├── tool_calling.md      # Custom tool integration
    ├── safety.md            # Safety and guardrails
    ├── localization.md      # Multilingual support
    ├── api_reference.md     # Complete API reference
    └── advanced.md          # Advanced patterns
```

## Common Patterns

### Basic Text Generation

```swift
let model = SystemLanguageModel(useCase: .general)
let session = LanguageModelSession(model: model)
let response = try await session.respond(
    to: Prompt("Write a haiku about Swift programming")
)
```

### Guided Generation (Structured Output)

```swift
struct TaskItem: Generable {
    let title: String
    let priority: String
    let dueDate: String
}

let tasks = try await session.respond(
    generating: [TaskItem].self,
    prompt: Prompt("Extract tasks from this email...")
)
```

### Tool Calling

```swift
struct WeatherTool: Tool {
    func call(arguments: String) async throws -> String {
        // Fetch and return weather data
        return "Current weather: ..."
    }
}

let session = LanguageModelSession(
    model: model,
    tools: [WeatherTool()]
)
```

### Streaming Responses

```swift
let stream = try await session.streamResponse(
    to: Prompt("Tell me a story...")
)

for try await chunk in stream {
    print(chunk.text)
}
```

## Implementation Workflow

1. **Choose Your Use Case** – Start with the appropriate `SystemLanguageModel.UseCase`
2. **Define Instructions** – Set clear behavioral guidelines for your model
3. **Handle Availability** – Check for model availability on the current device
4. **Build Your Session** – Create a `LanguageModelSession` with your configuration
5. **Process Responses** – Handle streaming or final responses in your UI
6. **Add Tools** (Optional) – Extend capabilities with custom tool implementations
7. **Implement Safety** – Add guardrails appropriate for your use case

## Best Practices

- **Prompting**: Be specific and detailed in your prompts for better results
- **Instructions**: Define the model's role and behavior upfront with clear instructions
- **Safety**: Enable guardrails for content that touches on safety-sensitive topics
- **Performance**: Use `prewarm()` to initialize models before critical moments
- **Streaming**: Leverage `streamResponse()` for immediate user feedback
- **Localization**: Test with multiple languages if supporting international users
- **Error Handling**: Implement proper error handling for generation failures and unavailable models

## Documentation and References

- [Apple Developer – Foundation Models](https://developer.apple.com/documentation/FoundationModels)
- [WWDC 2024 – Meet the Foundation Models framework](https://developer.apple.com/wwdc24/)
- [Structured API Reference](references/api_reference.md) – In this repository
- [Getting Started Guide](references/getting_started.md) – In this repository

## License

[Specify your license here (e.g., MIT, Apache 2.0, or proprietary)]

---

**Note**: This is a reference and documentation repository. For sample implementation code, see the official Apple Foundation Models examples and WWDC session materials.
