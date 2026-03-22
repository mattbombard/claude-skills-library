# Claude Code Skills Library

A personal collection of reusable Claude Code skills.

## Available Skills

### organize-filenames
Reviews files in a directory and renames them using consistent naming conventions (kebab-case for folders, snake_case for files).

### example-skill
An example skill demonstrating the basic structure and format. Use this as a reference when creating new skills.

### data-analysis-template
Creates Excel or Google Sheets templates for simple data analysis tasks with formulas, formatting, and structure. Supports expense trackers, sales dashboards, project time tracking, and more.

### quant-code-review
Reviews Python code for quant finance with focus on numerical stability, performance, vectorization, and financial modeling best practices. Covers Pandas/NumPy optimization, risk management, and common pitfalls.

### quant-strategy-backtest
Creates robust backtesting frameworks for trading strategies with proper data handling, performance metrics, and bias prevention. Supports multiple strategy types (momentum, mean reversion, pairs trading) and asset classes.

## Usage

Copy the desired skill folder from `skills/` to your project's `.claude/skills/` directory, then invoke with `/skill-name`.

## Skill Structure

Each skill contains a `SKILL.md` file with:
- Frontmatter (name, description)
- Purpose and instructions
- Examples
- Guidelines and notes

## Adding New Skills

1. Create a new folder in `skills/` using kebab-case naming
2. Add a `SKILL.md` file following the format in `example-skill`
3. Update this file and `README.md` with the new skill
