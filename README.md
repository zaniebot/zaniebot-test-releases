# zaniebot-test-releases

Minimal playground for one GitHub behavior:

- a tag ruleset requires a successful deployment to the `release` environment
- a workflow runs on `main`
- the workflow tries to create a release for an older target SHA
- this fails unless the workflow first creates a manual successful deployment for that target SHA

## Setup

This repository is configured with:

- a `release` environment that requires approval by `zaniebot`
- a tag ruleset that requires a successful `release` deployment

## Workflow

Use the `Release Lab` workflow.

Inputs:

- `tag`: tag / release name
- `sha`: optional target SHA; if omitted, uses `github.sha`
- `manual-deploy`: when true, creates a successful manual deployment for the target SHA before creating the release

## Repro

1. Run the workflow from `main` with:
   - `sha` set to the previous commit on `main`
   - `manual-deploy = false`
   - expected result: failure with `Missing successful active release deployment`
2. Run it again with the same `sha` but:
   - `manual-deploy = true`
   - expected result: success
