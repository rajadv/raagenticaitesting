---
description: Review pull requests for missing tests
model: gpt-4.1
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

Review the pull request that triggered this workflow for test coverage.

1. Inspect the pull request title, description, changed files, and diff.
2. Identify production code, public behavior, configuration, or bug-fix changes that reasonably require tests.
3. Inspect the repository's existing test directories, naming conventions, and nearby tests to determine whether appropriate tests were added or already cover the changes.
4. Do not request tests for documentation-only changes, generated files, dependency lockfiles, or changes that are clearly configuration-only unless the configuration changes executable behavior.
5. If one or more concrete test gaps remain, use `add-comment` to post one concise review comment on the pull request. Summarize the missing tests, identify the changed files or behaviors they should cover, and suggest the most appropriate existing test location or pattern.
6. If appropriate tests exist or no meaningful test gap is found, do not post a comment.

Do not make code changes, run tests, or claim that tests are missing without first checking the repository's existing test conventions and the pull request diff.