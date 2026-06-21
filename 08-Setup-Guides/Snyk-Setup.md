# Snyk Setup

[← Back to Main Index](../README.md)

## Configuration
1. Login to Snyk.io.
2. Navigate to Account Settings and generate an API Token.
3. Save this token as `SNYK_TOKEN` in the GitHub Repository Secrets.

## Integrations
Snyk is invoked directly via the `snyk/actions/node` and `snyk/actions/python` GitHub Actions within `_ci-reusable.yml`. The project will automatically appear in the Snyk dashboard for continuous monitoring once the pipeline runs.
