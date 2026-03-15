# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**code-review-agent** is an automated code review tool built with the Claude Agent SDK. It analyzes source code for bugs, security vulnerabilities, performance issues, and code quality problems, returning results in a structured JSON format with severity levels and suggestions.

## Development Commands

```bash
# Install dependencies
npm install

# Run the code review agent against current directory
npx tsx review-agent.ts .

# Run the code review agent against a specific directory
npx tsx review-agent.ts /path/to/code

# Run a simple agent example
npx tsx agent.ts
```

## Architecture

### Main Components

**review-agent.ts** - The primary code review agent
- Implements a `runCodeReview(directory)` function that analyzes code in a target directory
- Uses the Claude Agent SDK's `query()` interface for agentic behavior
- Outputs structured results matching the `ReviewResult` interface with:
  - Issues array (severity: low/medium/high/critical, categories: bug/security/performance/style)
  - Summary text
  - Overall score (0-100)
- Includes a sub-agent "security-scanner" that specializes in security analysis

**agent.ts** - Simple example demonstrating basic SDK usage
- Shows how to use `query()` with a simple prompt
- Allows limiting which tools an agent can use via `allowedTools`

**example.ts** - Sample code with intentional bugs
- Used for testing the review agent
- Contains intentional issues: off-by-one error, null checks, sensitive data logging, missing type annotations

### Key SDK Concepts

The Claude Agent SDK (`@anthropic-ai/claude-agent-sdk`) enables agentic workflows:
- **query()** - Main function to create agent interactions; returns an async iterator of messages
- **Message Types**: "assistant" (tool use), "result" (completion status)
- **Tool Access**: Agents can use tools like Read, Glob, Grep depending on `allowedTools` config
- **Structured Output**: JSON schema support via `outputFormat.type: "json_schema"`
- **Sub-agents**: Define specialized agents with their own models and tool access
- **Permission Modes**: `bypassPermissions` allows automated execution without prompts

### Code Review Flow

1. `runCodeReview()` is called with a target directory
2. Agent SDK makes a query to Claude with the prompt and configuration
3. Claude uses Read/Glob/Grep tools to analyze the target directory
4. Security-scanner sub-agent runs deep security checks (uses Sonnet model)
5. Results are returned as structured JSON matching the `ReviewResult` schema
6. `printResults()` formats and displays the findings with visual indicators

## Key Configuration Options in review-agent.ts

- `model: "opus"` - Uses Claude Opus for the main agent (most capable)
- `allowedTools: ["Read", "Glob", "Grep", "Task"]` - Which tools the agent can invoke
- `maxTurns: 250` - Maximum agent loop iterations
- `permissionMode: "bypassPermissions"` - Skips user confirmation for tool use (automated mode)
- Sub-agent "security-scanner" uses `model: "sonnet"` and specialized prompt

## Testing

The project includes `example.ts` as sample code with intentional issues. Run the review agent against it:

```bash
npx tsx review-agent.ts .
```

This will review the current directory (including example.ts) and output findings.

## Dependencies

- **@anthropic-ai/claude-agent-sdk**: ^0.2.76 - Core SDK for building agents
- **typescript**: ^5.9.3 - TypeScript compiler
- **tsx**: ^4.21.0 - TypeScript executor for running .ts files directly
- **@types/node**: ^25.5.0 - Node.js type definitions

## Important Notes

- The agent uses `permissionMode: "bypassPermissions"`, which means it will execute tools without prompting. Be careful when running against sensitive directories.
- JSON schema validation is strict; the agent must return results matching the `ReviewResult` structure exactly.
- The security-scanner sub-agent is a specialized agent; extending it requires understanding how to define AgentDefinition objects in the SDK.