# SonarQube Setup

[← Back to Main Index](../README.md)

## Configuration
1. Login to your SonarQube instance.
2. Create a new Project named `FlowForge`.
3. Generate a new Project Token.
4. Save this token as `SONAR_TOKEN` in the GitHub Repository Secrets.

## Quality Gates
Ensure the default "Sonar way" quality gate is applied, which blocks builds if code coverage on new code drops below 80% or if new critical vulnerabilities are introduced.
