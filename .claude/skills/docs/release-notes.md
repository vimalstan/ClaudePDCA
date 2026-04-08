# Release Notes

Generate changelog and user-facing release notes.

## Process

1. Collect commits since last release tag
2. Group by conventional commit type (feat, fix, perf, etc.)
3. Generate developer changelog (all commits, grouped)
4. Generate user-facing release notes (feat + fix only, curated from PR descriptions)
5. Create GitHub Release with both

## Output

Two formats:
- **CHANGELOG.md**: developer-facing, complete, auto-generated
- **GitHub Release**: user-facing, curated, highlights what matters to end users
