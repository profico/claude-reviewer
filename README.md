# Claude Code Reviewer - Reusable Workflow

A reusable GitHub Actions workflow for AI-powered code reviews using Claude, with framework-specific review rules.

## Features

- Framework-specific review rules (Next.js, NestJS, React, and more)
- Reads project-specific cursor rules
- **Duplicate detection** - Avoids leaving duplicate comments on subsequent runs
- **Chain of thought analysis** - Thorough, systematic review process that catches all issues in one pass
- **Precise line range selection** - Comments highlight only the exact lines with issues
- Label-based activation (optional)
- Inline and top-level comments
- Focuses only on bugs, errors, and critical issues
- Autofills PR descriptions from linked issues when context is missing
- Can auto-link a matching open issue if the PR has none (title/body/file aware)
- Preserves comment history across workflow runs

## Quick Start

### Step 1: Add Workflow to Your Repository

Create `.github/workflows/claude-review.yml` in your repository:

```yaml
name: Claude Review

on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  claude-review:
    permissions:
      contents: read
      pull-requests: write
      id-token: write
      issues: write
    uses: profico/claude-reviewer/.github/workflows/review.yml@main
    with:
      framework: 'next'  # Options: next, nest, react, generic
      trigger: 'auto'    # Options: label, auto (auto runs on every PR)
      pr_number: ${{ github.event.pull_request.number }}
    secrets:
      ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

### Step 2: Add API Key Secret

1. Go to your repository Settings > Secrets and variables > Actions
2. Click "New repository secret"
3. Name: `ANTHROPIC_API_KEY`
4. Value: Your Anthropic API key from console.anthropic.com

### Step 3: Add the Claude GitHub app
https://github.com/apps/claude

### Step 4: Use the Label
You can control when reviews run with the `trigger` input:

- `label`: run only when the `claude` label is present on the PR
- `auto` (default): run on every PR (no label required)

For simple setups, keep using `on: pull_request` as in the examples above; you do not need `workflow_run` to test or use this workflow.

## Workflow Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `framework` | No | `generic` | Framework type: `next`, `nest`, `react`, `generic` |
| `trigger` | No | `auto` | Trigger mode: `label` (requires `claude` label) or `auto` (runs on every PR) |
| `pr_number` | Yes | - | Pull request number to review |

## Required Secrets

| Secret | Description |
|--------|-------------|
| `ANTHROPIC_API_KEY` | Your Anthropic API key from console.anthropic.com |

## Framework Options

### Next.js (framework: 'next')

Review rules include:
- App Router vs Pages Router patterns
- Server Components vs Client Components
- Data fetching and caching strategies
- Route structure and dynamic routes
- Image optimization
- Metadata and SEO
- Server Actions validation
- Common RSC mistakes

### NestJS (framework: 'nest')

Review rules include:
- Dependency injection patterns
- Module organization and circular dependencies
- Controller and route validation
- Guards, middleware, and interceptors
- Exception handling
- Database operations (TypeORM/Prisma)
- Service layer separation
- Configuration management

### React (framework: 'react')

Review rules include:
- Hooks usage and rules
- Component performance (memo, useMemo, useCallback)
- State management patterns
- Event handlers and side effects
- Accessibility (ARIA, semantic HTML)
- TypeScript prop types
- Custom hooks composition
- Common anti-patterns

### Generic (framework: 'generic')

Default fallback rules include:
- Error handling
- Input validation
- Security vulnerabilities
- Performance issues
- Type safety
- Best practices

## Usage Examples

### Example 1: Next.js Application

```yaml
name: Claude Code Review

on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  claude-review:
    permissions:
      contents: read
      pull-requests: write
      id-token: write
      issues: write
    uses: profico/claude-reviewer/.github/workflows/review.yml@main
    with:
      framework: 'next'
      trigger: 'auto'
      pr_number: ${{ github.event.pull_request.number }}
    secrets:
      ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

### Example 2: NestJS API

```yaml
name: Claude Code Review

on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  claude-review:
    permissions:
      contents: read
      pull-requests: write
      id-token: write
      issues: write
    uses: profico/claude-reviewer/.github/workflows/review.yml@main
    with:
      framework: 'nest'
      trigger: 'auto'
      pr_number: ${{ github.event.pull_request.number }}
    secrets:
      ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

### Example 3: React Library

```yaml
name: Claude Code Review

on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  claude-review:
    permissions:
      contents: read
      pull-requests: write
      id-token: write
      issues: write
    uses: profico/claude-reviewer/.github/workflows/review.yml@main
    with:
      framework: 'react'
      trigger: 'auto'
      pr_number: ${{ github.event.pull_request.number }}
    secrets:
      ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

### Example 4: Generic/Multiple Frameworks

```yaml
name: Claude Code Review

on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  claude-review:
    permissions:
      contents: read
      pull-requests: write
      id-token: write
      issues: write
    uses: profico/claude-reviewer/.github/workflows/review.yml@main
    with:
      framework: 'generic'  # or omit this line, as 'generic' is the default
      trigger: 'auto'
      pr_number: ${{ github.event.pull_request.number }}
    secrets:
      ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```
## Project-Specific Rules

The workflow automatically reads `.cursor/rules/*.mdc` files from your repository for project-specific context.

Example structure:
```
your-repo/
└── .cursor/
    └── rules/
        ├── api-patterns.mdc
        ├── database.mdc
        └── security.mdc
```

These rules are combined with the framework-specific rules to provide comprehensive review context.

## Version Pinning

### Use Latest Version (Main Branch)
```yaml
uses: profico/claude-reviewer/.github/workflows/review.yml@main
```

### Pin to Specific Tag
```yaml
uses: profico/claude-reviewer/.github/workflows/review.yml@v1.0.0
```

### Pin to Specific Commit
```yaml
uses: profico/claude-reviewer/.github/workflows/review.yml@abc123def456
```

## How It Works

1. External repository creates a PR
2. Workflow checks PR for the `claude` label based on the `trigger` input
3. If the PR description lacks context, it pulls linked issues and updates the PR body with the missing details
4. If no issue is linked, it searches open issues (title/body and touched files) for a clear match and links it
5. Downloads framework-specific rules from this repository
6. Reads project-specific `.cursor/rules/*.mdc` files
7. **Fetches existing Claude comments** to prevent duplicates
8. Claude performs **chain of thought analysis**, systematically reviewing all changed files
9. **Cross-references findings** against existing comments to avoid duplicates
10. Posts inline comments for specific issues with **precise line ranges**
11. Posts summary comment with overall feedback

## What Claude Reviews

Claude focuses on actual problems and will comment on:

- Actual bugs and errors
- Security vulnerabilities
- Critical performance issues
- Type errors and runtime errors
- Framework-specific anti-patterns

Claude will NOT comment on:

- Code style preferences
- Minor optimizations
- Working code that could be "better"
- Generated files and lock files
- Compliments or praise
- Issues already covered by existing comments (duplicate detection)

## Review Quality

Claude uses **chain of thought prompting** to ensure comprehensive reviews:

- Analyzes the entire PR systematically before commenting
- Identifies all files and their relationships
- Cross-references with existing comments to avoid duplicates
- Determines precise line ranges for each issue
- Catches issues in a single pass, reducing the need for re-runs

## Permissions

The workflow requires these permissions (must be explicitly granted in the calling workflow):

- `contents: read` - To read the repository code
- `pull-requests: write` - To post review comments
- `id-token: write` - For OIDC authentication
- `issues: write` - For context

**Important:** You must add the `permissions` block to your workflow job as shown in all examples above. Reusable workflows do not inherit permissions by default.

## Troubleshooting

### Reviews Not Triggering

- Check the `claude` label is on the PR
- Verify `ANTHROPIC_API_KEY` secret exists in repository settings
- Check GitHub Actions are enabled for the repository
- Verify the workflow file syntax is correct

### No Comments Posted

- Check the workflow logs in the Actions tab
- Verify the `claude` label was added before the workflow ran
- Check if there are actual issues to comment on (Claude only comments on bugs)
- Ensure the PR has file changes to review

### Wrong Framework Rules Applied

- Verify the `framework` input matches your stack
- Check if the framework rule file exists in profico/claude-reviewer/rules/
- Review the workflow logs to see which rules were loaded

### API Rate Limits

- If you hit rate limits, consider adding the label only when needed
- Remove the label after review to prevent re-runs

## Repository Structure

```
profico/claude-reviewer/
├── .github/
│   ├── workflows/
│   │   ├── review.yml        # Reusable workflow
│   │   └── claude.yml        # Self-review workflow
│   └── ISSUE_TEMPLATE/
│       └── framework-request.yml
├── rules/
│   ├── next.md               # Next.js review rules
│   ├── nest.md               # NestJS review rules
│   ├── react.md              # React review rules
│   └── generic.md            # Generic review rules
└── README.md                 # This file
```

## Contributing

### Adding New Framework Support

To add support for a new framework:

1. Fork this repository
2. Create a new rule file in `rules/your-framework.md`
3. Follow the existing format (see `rules/next.md` for reference)
4. Focus on common bugs, errors, and anti-patterns specific to the framework
5. Submit a PR with your changes

Rule files should include:

- Architecture and structure guidelines
- Common mistakes and anti-patterns
- Performance considerations
- Security concerns
- Framework-specific best practices
- Testing patterns

### Requesting Framework Support

If you'd like support for a framework but don't want to create the rules yourself, open an issue using the "Request New Framework Support" template.

## Testing

To test changes to this workflow:

1. Create a PR in this repository
2. Add the `claude` label
3. The workflow will review itself using the generic rules

## License

MIT
