# Code Review Agent

An AI-powered code review tool that analyzes source code for bugs, security vulnerabilities, performance issues, and code quality problems using the Claude Agent SDK.

## Features

- **Comprehensive Analysis**: Detects bugs, security vulnerabilities, performance issues, and style problems
- **Structured Output**: Returns detailed findings with severity levels, categories, and actionable suggestions
- **Specialized Security Scanning**: Dedicated sub-agent for deep security vulnerability analysis
- **Flexible Tool Access**: Uses file reading, globbing, and grep for thorough code analysis
- **JSON Schema Validation**: Ensures consistent, structured results

## Installation

```bash
npm install
```

## Usage

Review a specific directory:

```bash
npx tsx review-agent.ts /path/to/code
```

Review the current directory:

```bash
npx tsx review-agent.ts .
```

## Example Output

The agent returns:
- Issues with severity levels (critical, high, medium, low)
- Categories (bug, security, performance, style)
- File locations and line numbers
- Detailed descriptions and suggestions
- Overall code quality score

## Requirements

- Node.js 18+
- Claude API access

## Dependencies

- `@anthropic-ai/claude-agent-sdk` - Framework for building AI agents
- `typescript` - Type safety and development experience
- `tsx` - TypeScript executor

## How It Works

1. Agent analyzes target directory using file system tools
2. Claude Opus model reviews code for issues
3. Specialized security-scanner sub-agent performs deep security analysis
4. Results are structured and formatted with severity indicators
5. Comprehensive report is generated with actionable insights

## Based on this article: https://gist.github.com/dabit3/93a5afe8171753d0dbfd41c80033171d