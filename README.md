# CloudFormation CI Reusable Workflow

![Built with Kiro](https://img.shields.io/badge/Built%20with-Kiro-blue?style=flat&logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTEyIDJMMTMuMDkgOC4yNkwyMCA5TDEzLjA5IDE1Ljc0TDEyIDIyTDEwLjkxIDE1Ljc0TDQgOUwxMC45MSA4LjI2TDEyIDJaIiBmaWxsPSJ3aGl0ZSIvPgo8L3N2Zz4K)&nbsp;![GitHub Action](https://img.shields.io/badge/GitHub-Action-blue?logo=github)&nbsp;![Release](https://github.com/subhamay-bhattacharyya-gha/cfn-ci-reusable-wf/actions/workflows/release.yaml/badge.svg)&nbsp;![Commit Activity](https://img.shields.io/github/commit-activity/t/subhamay-bhattacharyya-gha/cfn-ci-reusable-wf)&nbsp;![Bash](https://img.shields.io/badge/Language-Bash-green?logo=gnubash)&nbsp;![CloudFormation](https://img.shields.io/badge/AWS-CloudFormation-orange?logo=amazonaws)&nbsp;![Last Commit](https://img.shields.io/github/last-commit/subhamay-bhattacharyya-gha/cfn-ci-reusable-wf)&nbsp;![Release Date](https://img.shields.io/github/release-date/subhamay-bhattacharyya-gha/cfn-ci-reusable-wf)&nbsp;![Repo Size](https://img.shields.io/github/repo-size/subhamay-bhattacharyya-gha/cfn-ci-reusable-wf)&nbsp;![File Count](https://img.shields.io/github/directory-file-count/subhamay-bhattacharyya-gha/cfn-ci-reusable-wf)&nbsp;![Issues](https://img.shields.io/github/issues/subhamay-bhattacharyya-gha/cfn-ci-reusable-wf)&nbsp;![Top Language](https://img.shields.io/github/languages/top/subhamay-bhattacharyya-gha/cfn-ci-reusable-wf)&nbsp;![Custom Endpoint](https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/bsubhamay/1808f9dd556677dc9ee3bc4b3cc2fbfe/raw/cfn-ci-reusable-wf.json?)

## Description

A comprehensive CloudFormation CI/CD workflow that mirrors the functionality of an existing Terraform CI pipeline. This reusable GitHub Actions workflow provides linting, validation, security scanning, and deployment capabilities for CloudFormation templates. It includes environment checking, change detection, service detection, and cost analysis to ensure robust infrastructure deployment practices.

---

## Features

- ✅ **Reusable Workflow**: Deploy AWS infrastructure consistently across different projects and environments
- ✅ **Environment Validation**: Ensure deployments only happen to valid, configured environments
- ✅ **Change Detection**: Optimize workflow execution by only running relevant steps when files have changed
- ✅ **AWS Service Detection**: Identify which AWS services are being used in CloudFormation templates
- ✅ **CloudFormation Validation**: Catch template syntax and logical errors before deployment
- ✅ **Linting & Security Scanning**: Ensure templates follow best practices and security standards
- ✅ **Conditional Build Steps**: Build Lambda, Glue, and other AWS service artifacts only when relevant changes are detected
- ❌ **Resource Tagging**: Resource tagging with Yor has been removed from this implementation
- ✅ **Deployment Planning**: Review infrastructure changes before they are applied
- ✅ **Cost Estimation**: Understand the financial impact of infrastructure changes before deployment
- ✅ **CloudFormation Deployment**: Apply approved infrastructure changes to AWS environments
- ✅ **Automated Cleanup**: Prevent CI environments from accumulating unnecessary resources and costs
- ✅ **Pull Request Automation**: Automatically promote successful CI runs for code review

---

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `environment` | Environment to deploy to (e.g., ci, devl, test, prod) | Yes | - |
| `cfn-directory` | Directory containing CloudFormation template files | Yes | `cfn` |
| `ci-build` | Indicates if this is a CI build run | No | `true` |

## Secrets

| Name | Description | Required |
|------|-------------|----------|
| `aws-role-arn` | AWS role ARN for assuming a role | Yes |
| `infracost-api-key` | API key for Infracost | Yes |
| `infracost-gist-id` | Gist ID for Infracost output | Yes |

## Permissions

- `id-token: write` - Required for OIDC authentication with AWS
- `contents: write` - Required for repository operations

---

## Example Usage

```yaml
name: CloudFormation CI/CD

on:
  push:
    branches:
      - main
      - 'feature/**'
    paths:
      - 'cfn/**'
      - '.github/workflows/**'

jobs:
  cloudformation-ci:
    name: CloudFormation CI
    uses: subhamay-bhattacharyya-gha/cfn-ci-reusable-wf/.github/workflows/ci.yaml@main
    with:
      environment: ci
      cfn-directory: cfn
      ci-build: true
    secrets:
      aws-role-arn: ${{ secrets.AWS_ROLE_ARN }}
      infracost-api-key: ${{ secrets.INFRACOST_API_KEY }}
      infracost-gist-id: ${{ secrets.INFRACOST_GIST_ID }}
```

## Workflow Steps

The workflow executes the following steps in sequence:

1. **Discovery Phase**
   - Check environments
   - Check branch issue
   - Detect changes
   - Detect services

2. **Validation Phase**
   - CloudFormation template validation
   - CloudFormation linting
   - Security scanning with Checkov

3. **Build Phase**
   - Conditional Lambda package building
   - Conditional Lambda layer building
   - Conditional Glue script packaging
   - Conditional State Machine validation

4. **Deployment Phase**
   - CloudFormation stack creation
   - CloudFormation stack deletion (for CI environments)

5. **Integration Phase**
   - Pull request creation for successful runs

## CloudFormation Template Requirements

For this workflow to function properly, your CloudFormation project should have:

1. A `cloudformation.json` file in your CloudFormation directory that specifies the main template:
   ```json
   {
     "template": "your-template.yaml"
   }
   ```

2. Parameter files in the `params` subdirectory of your CloudFormation directory

## Implementation Details

- **Parameter Handling**: Supports both CloudFormation parameter format (`[{"ParameterName":"key","ParameterValue":"value"}]`) and simple JSON format (`{"key":"value"}`)
- **Error Handling**: Comprehensive error reporting with detailed logs and GitHub Step Summaries
- **Security**: OIDC-based authentication with AWS for secure credential handling
- **Performance**: Optimized through parallel job execution and conditional logic

## License

MIT