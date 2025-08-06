# Implementation Plan

- [x] 1. Create the main CloudFormation CI workflow file
  - Create `.github/workflows/ci.yaml` with the complete workflow structure
  - Define workflow inputs, secrets, and permissions matching the requirements
  - Set up the workflow trigger configuration for workflow_call events
  - _Requirements: 1.1, 1.2, 1.3, 1.4_

- [x] 2. Implement discovery and validation jobs
  - [x] 2.1 Create check-environments job
    - Implement environment validation using the check-environments-action
    - Configure job to run on specified environment with proper permissions
    - _Requirements: 2.1, 2.2, 2.3_

  - [x] 2.2 Create check-branch-issue job
    - Implement branch issue verification for non-main branches
    - Add conditional logic to skip on main branch
    - Include output printing for debugging
    - _Requirements: 2.4_

  - [x] 2.3 Create detect-changes job
    - Implement file change detection using list-updated-files-action
    - Configure outputs for files-changed, json-output, and has-changes
    - Add conditional logic for non-main branches
    - _Requirements: 3.1, 3.2, 3.4_

  - [x] 2.4 Create detect-services job
    - Implement AWS service detection using scan-aws-services-action
    - Generate and display AWS services detection table in workflow summary
    - Handle JSON parsing and error cases for service detection output
    - _Requirements: 4.1, 4.2, 4.3, 4.4_

- [x] 3. Implement execution path determination
  - Create execution-path job that depends on all discovery jobs
  - Implement logic to determine which workflow steps should execute
  - Parse and prepare input files for execution path determination
  - Upload execution path data as workflow artifact
  - _Requirements: 3.3_

- [x] 4. Implement CloudFormation validation and linting
  - [x] 4.1 Create cloudformation-validate job
    - Configure AWS credentials using OIDC role assumption
    - Implement CloudFormation template validation using AWS CLI
    - Add conditional execution based on IaC changes detection
    - Configure soft-fail option for validation errors
    - _Requirements: 5.1, 5.2, 5.3, 5.4_

  - [x] 4.2 Create cloudformation-lint job
    - Implement cfn-lint execution for CloudFormation template linting
    - Configure linting format and caching options
    - Add conditional execution based on IaC changes
    - _Requirements: 6.1, 6.4_

- [x] 5. Implement security scanning
  - Create checkov-scan job for CloudFormation security analysis
  - Configure AWS credentials and Checkov scan for CloudFormation framework
  - Generate SARIF report and workflow summary for security findings
  - Implement soft-fail configuration for security scan results
  - _Requirements: 6.2, 6.3_

- [x] 6. Implement conditional build jobs
  - [x] 6.1 Create build-lambda job
    - Add conditional execution based on lambda changes detection
    - Implement Lambda package building and uploading logic
    - _Requirements: 7.1_

  - [x] 6.2 Create build-lambda-layer job
    - Add conditional execution based on lambda-layer changes detection
    - Implement Lambda layer package building and uploading
    - _Requirements: 7.2_

  - [x] 6.3 Create build-glue job
    - Add conditional execution based on glue changes detection
    - Implement Glue script packaging and uploading
    - _Requirements: 7.3_

  - [x] 6.4 Create build-state-machine job
    - Add conditional execution based on state-machine changes detection
    - Implement Step Functions state machine validation and uploading
    - _Requirements: 7.4_

- [x] 7. Implement resource tagging
  - Implement CloudFormation native tagging support in parameter preparation
  - Extract tags from deployment artifacts and parameter files
  - Pass tags to CloudFormation deployment action via cloudformation-tags input
  - Use cfn-create-stack-action branch with tagging support
  - _Requirements: 8.1, 8.2, 8.3, 8.4_

- [x] 8. Implement CloudFormation deployment planning
  - Create cloudformation-plan job that depends on resource tagging
  - Configure AWS credentials and CloudFormation change set creation
  - Implement change set analysis and summary display
  - Handle cases where no changes are detected
  - _Requirements: 9.1, 9.2, 9.3, 9.4_

- [x] 9. Implement cost estimation
  - Create infra-cost job that depends on CloudFormation planning
  - Configure Infracost for CloudFormation cost estimation
  - Generate cost difference reports and update shared Gist
  - Handle cost estimation failures gracefully
  - _Requirements: 10.1, 10.2, 10.3, 10.4_

- [x] 10. Implement CloudFormation deployment
  - Create cloudformation-apply job that depends on cost estimation
  - Configure CloudFormation stack deployment using change sets
  - Implement deployment success/failure reporting
  - Handle rollback scenarios and error reporting
  - _Requirements: 11.1, 11.2, 11.3, 11.4_

- [x] 11. Implement automated cleanup
  - Create cloudformation-destroy job that depends on deployment
  - Implement conditional cleanup for CI pipeline mode
  - Configure CloudFormation stack deletion with proper error handling
  - Provide manual cleanup instructions for failures
  - _Requirements: 12.1, 12.2, 12.3, 12.4_

- [x] 12. Implement pull request automation
  - Create create-pull-request job that depends on all previous jobs
  - Configure conditional execution based on all job success status
  - Implement pull request creation with workflow summary
  - Add logic to skip PR creation on main branch
  - _Requirements: 13.1, 13.2, 13.3, 13.4_