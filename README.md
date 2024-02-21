# Keep GitHub Actions Alive

> This is a fork of [invenia/KeepActionsAlive](https://github.com/invenia/KeepActionsAlive)

Automatically prevent your scheduled GitHub Actions from becoming disabled after 60 days.

## Overview

This package is meant to be deployed in an AWS environment. It contains a Lambda function which will run every `20 days`.

The Lambda function gets GitHub repositories belonging to an organization and hits the [enable workflow](https://docs.github.com/en/rest/actions/workflows?apiVersion=2022-11-28#enable-a-workflow) REST endpoint to re-enable the GitHub Actions workflow of each repository.

## Generate GitHub Personal Access Token

You will need a [personal access token](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token) with a `workflow` scope.

You will then need to store your PAT in AWS SecretsManager.

To do so, replace `<foobar>` in the following command with the PAT you created above:

```bash
aws secretsmanager create-secret --name KeepActionsAlivePAT --secret-string '{"PAT": "<foobar>"}'
```

Copy the ARN of the created secret as you will need it when deploying the template next.

### Launch the Project

There are three parameters used when deploying the template:
- `PATSecretARN`: Enter the ARN from the secret for your PAT from the previous step
- `Organization`: Name of the GitHub organization which you want to re-enable workflows
