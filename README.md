# Claude Skills

A collection of custom skills that extend Claude's capabilities with specialized workflows and domain expertise.

## What are Claude Skills?

Skills are reusable prompts that provide Claude with specialized knowledge and structured workflows for specific tasks. Each skill includes:
- Detailed execution workflows
- Context adaptation guidance
- Quality standards and best practices
- Structured output formats

Learn more about Claude Skills:
- **[Official Documentation](https://docs.claude.com/en/docs/claude-code/skills)** - How to use and create skills
- **[Anthropic Skills Repository](https://github.com/anthropics/skills)** - Official example skills
- **[Creating Custom Skills](https://support.claude.com/en/articles/12512198-how-to-create-custom-skills)** - Step-by-step guide
- **[Anthropic's Skills Announcement](https://www.anthropic.com/news/skills)** - Introduction to Skills

## Quick Start

### Download Skills

Skills are available as individual ZIP files from the latest release:

**[Download Latest Skills →](../../releases/latest)**

Each skill can be downloaded separately:
- [`book-report.zip`](../../releases/latest/download/book-report.zip)

### Installation

1. Download the desired skill ZIP file from the releases page
2. Extract the ZIP to your Claude skills directory (`~/.claude/skills/`)
3. The skill will be automatically available in Claude Code

## Available Skills

### 📚 [Book Report](./book-report)

**Description:** Comprehensive research and analysis of books with actionable insights, hidden gems, and personalized recommendations.

**Use when:**
- Researching a book for professional or personal purposes
- Creating detailed book reports with strategic insights
- Getting actionable takeaways adapted to your context

**Key features:**
- Two-phase workflow with confirmation step
- Multi-source web research (5+ searches)
- Context-aware recommendations (business/personal/academic/hobby)
- 8-section structured report with BLUF summary
- Hidden gems analysis for overlooked insights

**Trigger phrases:**
- "Research Book: {Book Title}"
- "Create a book report for {Book Title}"
- "Analyze {Book Title}"

[View full documentation →](./book-report/SKILL.md)

## Repository Structure

```
claude-skills/
├── README.md                   # This file
├── CLAUDE.md                   # Instructions for Claude Code
├── {skill-name}/
│   ├── SKILL.md                # Main skill definition
│   └── references/             # Optional: Additional guidance
│       └── *.md
└── .github/
    └── workflows/
        └── release-skills.yml  # Auto-release workflow
```

## Creating Your Own Skills

Each skill follows a standard structure with YAML frontmatter:

```yaml
---
name: skill-name
description: Brief description with trigger phrases
---
```

Key sections to include:
1. **Trigger Phrases** - When to activate
2. **Context Adaptation** - How to adapt to different scenarios
3. **Workflow** - Step-by-step execution
4. **Output Structure** - Format of deliverables
5. **Quality Standards** - Requirements for thoroughness

See [CLAUDE.md](./CLAUDE.md) for detailed guidance on skill architecture.

## How Skills Are Released

This repository uses a GitHub Action to automatically create ZIP files:

- **Trigger:** Changes to `*/SKILL.md` or `*/references/**` on `main` branch
- **Output:** A "latest" release with individual skill ZIPs
- **Updates:** Each push updates the existing release with fresh ZIPs

Skills can be downloaded via stable URLs:
```
https://github.com/{owner}/{repo}/releases/latest/download/{skill-name}.zip
```

## Contributing

This is an internal project that I'm making available for free. While I'm not generally accepting contributions, please don't hesitate to take these skills and build on them yourself! Feel free to fork, modify, and create your own versions.

If you find these useful, check out [Anthropic's official skills repository](https://github.com/anthropics/skills) for more examples and inspiration.

## License

Licensed under the [Apache License, Version 2.0](LICENSE) (the "License"); you may not use this project except in compliance with the License. You may obtain a copy of the License at:

https://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
