# GitHub Reusable CI Workflow using CloudFormation

![Commit Activity](https://img.shields.io/github/commit-activity/t/subhamay-bhattacharyya-gha/cfn-ci-reusable-wf)&nbsp;![Last Commit](https://img.shields.io/github/last-commit/subhamay-bhattacharyya-gha/cfn-ci-reusable-wf)&nbsp;![Release Date](https://img.shields.io/github/release-date/subhamay-bhattacharyya-gha/cfn-ci-reusable-wf)&nbsp;![Repo Size](https://img.shields.io/github/repo-size/subhamay-bhattacharyya-gha/cfn-ci-reusable-wf)&nbsp;![File Count](https://img.shields.io/github/directory-file-count/subhamay-bhattacharyya-gha/cfn-ci-reusable-wf)&nbsp;![Open Issues](https://img.shields.io/github/issues/subhamay-bhattacharyya-gha/cfn-ci-reusable-wf)&nbsp;![Top Language](https://img.shields.io/github/languages/top/subhamay-bhattacharyya-gha/cfn-ci-reusable-wf)&nbsp;![Monthly Commit Activity](https://img.shields.io/github/commit-activity/m/subhamay-bhattacharyya-gha/cfn-ci-reusable-wf)&nbsp;![Custom Endpoint](https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/bsubhamay/137270a1d412786fd1bae6d4f1a32e9b/raw/cfn-ci-reusable-wf.json?)

## Reusable CI Workflow for CloudFormation

This GitHub Action provides a reusable composite workflow that simplifies CI processes for CloudFormation templates. It includes steps to validate, lint, and deploy CloudFormation stacks, as well as interact with the GitHub API for enhanced automation.

---

## Inputs

| Name                | Description                                                                 | Required | Default        |
|---------------------|-----------------------------------------------------------------------------|----------|----------------|
| `aws-role-arn`      | ARN of the AWS IAM role to assume for deploying the stack.                  | Yes      | —              |
| `aws-region`        | AWS region where the stack will be deployed.                                | No       | `us-east-1`    |
| `cfn-template-file` | Path to the CloudFormation template file to be validated and deployed.      | Yes      | —              |
| `cfn-params-file`   | Path to the JSON file containing CloudFormation stack parameters.           | Yes      | —              |
| `snyk-token`        | Snyk token for security scanning integration.                               | No       | —              |

---

## Example Usage

```yaml
name: CloudFormation Build
run-name: Build run by ${{ github.actor }} on ${{ github.ref_name }}

on:
  workflow_dispatch:

env:
  AWS_ROLE_ARN: ${{ vars.AWS_ROLE_ARN }}
  AWS_REGION: ${{ vars.AWS_REGION }}

jobs:
  ci-build:
    uses: subhamay-bhattacharyya-gha/cfn-ci-reusable-wf/.github/workflows/ci.yaml@main
    environment: ci
    with:
      aws-role-arn: ${{ env.AWS_ROLE_ARN }}
      aws-region: ${{ env.AWS_REGION }}
      cfn-template-file: root-stack-template.yaml
      cfn-params-file: ./cfn/params/cfn-parameters.json
      snyk-token: ${{ secrets.SNYK_TOKEN }}

```

## License

MIT
