# GitHub Copilot CLI - Agent Instructions

## Project Overview

This is the **distribution repository** for GitHub Copilot CLI - a terminal-native AI coding assistant. This repo contains:
- Public-facing documentation ([README.md](README.md))
- Cross-platform installation script ([install.sh](install.sh))
- Release changelog ([changelog.md](changelog.md))
- Issue triage workflows ([.github/workflows](.github/workflows))

**Important**: This is NOT the main source code repository. It serves as the public interface for installation, documentation, and issue tracking.

## Repository Architecture

### Distribution-Only Structure
- No application source code - this repo orchestrates public releases
- Installation script [install.sh](install.sh) downloads pre-built binaries from GitHub releases
- Supports multiple installation methods: WinGet (Windows), Homebrew (macOS/Linux), npm, or direct script

### Key Components

**[install.sh](install.sh)** - Cross-platform installation orchestrator
- Auto-detects platform (Darwin/Linux) and architecture (x64/arm64)
- Downloads from `github.com/github/copilot-cli/releases`
- Validates SHA256 checksums when available
- Supports `VERSION` and `PREFIX` environment variables for custom installs
- Default install paths: `/usr/local/bin` (root) or `$HOME/.local/bin` (non-root)

**[changelog.md](changelog.md)** - Release history with version-dated entries
- Format: `## 0.0.XXX - YYYY-MM-DD` followed by bullet points
- Documents new features, fixes, and model availability changes
- Recent versions (~0.0.360-0.0.372) show rapid iteration

**Installation Methods** (documented in [README.md](README.md)):
```bash
# WinGet (Windows)
winget install GitHub.Copilot[.Prerelease]

# Homebrew (macOS/Linux)
brew install copilot-cli[@prerelease]

# npm (all platforms)
npm install -g @github/copilot[@prerelease]

# Install script (macOS/Linux)
curl -fsSL https://gh.io/copilot-install | bash
```

## Development Workflows

### Updating Documentation
When updating [README.md](README.md):
- Maintain structure: Introduction → Features → Installation → Usage → Feedback
- Keep installation instructions for ALL methods (WinGet, Homebrew, npm, script)
- Update prerequisites section if Node.js/PowerShell versions change
- Link to official docs: `https://docs.github.com/copilot/concepts/agents/about-copilot-cli`

### Release Management
When adding changelog entries to [changelog.md](changelog.md):
- Add new versions at the TOP of the file
- Use format: `## VERSION - YYYY-MM-DD` (e.g., `## 0.0.373 - 2025-12-30`)
- Group changes by category: features, fixes, model updates, MCP improvements
- Keep entries concise but specific (reference commands, flags, or features)

### Installation Script Updates
When modifying [install.sh](install.sh):
- Test on multiple platforms (Darwin, Linux) and architectures (x64, arm64)
- Preserve error handling for missing tools (curl/wget, sha256sum/shasum)
- Maintain Windows detection with winget fallback
- Keep `PREFIX` and `VERSION` environment variable support
- Validate tarball integrity before extraction

## Issue Triage Conventions

### GitHub Actions Workflows
Located in [.github/workflows](.github/workflows):
- `triage-issues.yml` - Auto-labels new issues with "triage"
- `close-single-word-issues.yml` - Auto-closes low-effort issues
- `stale-issues.yml` - Manages stale issue lifecycle
- `no-response.yml` - Closes issues awaiting user response
- `on-issue-close.yml` - Post-close automation
- `winget.yml` - WinGet package publishing workflow

### Issue Templates
[.github/ISSUE_TEMPLATE](.github/ISSUE_TEMPLATE):
- `bug_report.yml` - Requires version (`copilot --version`), reproduction steps, environment details
- `feature_request.yml` - Asks for example prompts/workflows demonstrating value

All new issues receive "triage" label - review and re-label appropriately.

## Project-Specific Patterns

### Version Numbering
- Format: `0.0.XXX` (pre-1.0 public preview)
- High velocity: multiple releases per week
- Versions referenced in install script, changelog, and npm package

### Platform Support Matrix
- **Platforms**: Windows, macOS, Linux
- **Architectures**: x64, arm64
- **Prerequisites**: Node.js v22+, npm v10+, PowerShell v6+ (Windows)
- **Authentication**: GitHub account with active Copilot subscription

### External Dependencies
- Hosted at `github.com/github/copilot-cli` (releases)
- npm package: `@github/copilot[@prerelease]`
- Homebrew formula: `copilot-cli[@prerelease]`
- WinGet package: `GitHub.Copilot[.Prerelease]`
- Install script URL: `https://gh.io/copilot-install`

## Quick Reference

### Getting Version Information
```bash
copilot --version  # Check installed CLI version
```

### Testing Installation Script Locally
```bash
# Test specific version install
VERSION="v0.0.369" PREFIX="$HOME/test" bash install.sh

# Test checksum validation
curl -fsSL https://gh.io/copilot-install | bash
```

### Common User Issues
- **PATH not configured**: Install script checks and warns if `$INSTALL_DIR` not in PATH
- **Permissions**: Script auto-detects root vs non-root for install location
- **Authentication**: First-time users need `/login` command or `GH_TOKEN`/`GITHUB_TOKEN` env var
- **Organization restrictions**: Enterprise admins can disable CLI access

### Links to Official Resources
- Main docs: `https://docs.github.com/copilot/concepts/agents/about-copilot-cli`
- Subscription plans: `https://github.com/features/copilot/plans`
- npm package: `https://www.npmjs.com/package/@github/copilot`
- Homebrew formula: `https://formulae.brew.sh/cask/copilot-cli`
