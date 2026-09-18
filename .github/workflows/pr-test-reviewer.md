---
description: Review pull requests for missing tests
model: gpt-4.1
max-turns: 24
max-ai-credits: 100
on:
  pull_request:
    types: [opened, synchronize]
permissions:
  contents: read
  pull-requests: read
  copilot-requests: write
tools:
  github:
    min-integrity: approved
    toolsets: [pull_requests, repos]
safe-outputs:
  add-comment:
    max: 1
    hide-older-comments: true
---

Review the pull request that triggered this workflow for test coverage. Keep the review bounded and finish within the configured turn budget.

1. Use the GitHub pull-request read tool once to inspect PR #1's title, description, changed files, and diff. Do not use shell commands, `git fetch`, or repeated repository-wide searches to gather this information.
2. Use at most one focused repository search for test files or test configuration, then inspect only the most relevant result. If no test files exist, record that fact and continue.
3. Identify production code, public behavior, configuration, or bug-fix changes that reasonably require tests.
4. Do not request tests for documentation-only changes, generated files, dependency lockfiles, or changes that are clearly configuration-only unless the configuration changes executable behavior.
5. If one or more concrete test gaps remain, use `add-comment` once to post one concise review comment on the pull request. Summarize the missing tests, identify the changed files or behaviors they should cover, and suggest the most appropriate existing test location or pattern.
6. If appropriate tests exist or no meaningful test gap is found, call `noop` once and finish.

Do not make code changes or run tests. Do not repeat a search or tool call. Stop after the review decision and its single output.