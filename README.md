# SiliconMCP Skills

A collection of skills for AI-powered hardware design workflows.

## Skills

### eval-sv-tbench

Generate, save, and submit SystemVerilog testbenches to EaaS (Evaluation as a Service) evaluations. This skill automates the full pipeline:

- Lists testcases from an EaaS eval set
- Loads testcase data (RTL module + spec)
- Generates self-checking `tb.sv` testbenches
- Creates an evaluation session and submits results
- Polls for and retrieves evaluation results

Requires access to the `eaas-siliconmcp` MCP server.

## Installation

### Claude Code

Add the skill to your project or global configuration:

```bash
# Project-level (recommended)
git clone git@github.com:siliconmcp/skills.git /tmp/siliconmcp-skills

# User level (global)
cp -r /tmp/siliconmcp-skills/eval-sv-tbench ~/.claude/skills/

# Project level
cp -r /tmp/siliconmcp-skills/eval-sv-tbench  <project-dir>/.claude/skills/
```

Alternatively, add it manually to your `.claude/settings.json`:

```json
{
  "skills": [
    "/path/to/skills/eval-sv-tbench/SKILL.md"
  ]
}
```

### Codex

Add the skill content to your Codex instructions file:

1. Copy the skill definition into your `codex.md` (or `AGENTS.md`) file:

```bash
cat /path/to/skills/eval-sv-tbench/SKILL.md >> codex.md
```

2. Or reference it in your Codex agent configuration (`agents/openai.yaml`):

```yaml
interface:
  display_name: "Eval Tbench"
  short_description: "Generate and submit SystemVerilog testbenches to EaaS evals."
  default_prompt: "Use when running EaaS testbench evaluations given an eval set name."
```

3. Ensure the `eaas-siliconmcp` MCP server is configured in your environment for tool access.

## License

MIT
