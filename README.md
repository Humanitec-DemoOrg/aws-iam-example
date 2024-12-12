# AWS IAM Example

This example demonstrates how to implement IAM Policies on AWS resources using Pod Identity in EKS.

In order to use this example, the following must already be configured:
- The Pod Identity Agent must be configured for the EKS cluster being used
- A Cloud Account using Temporary credentials for AWS must be configured. The role that will be assumed must have broad permissions to create and delete AWS Roles, Policies and S3 buckets.

## Steps to run example

### Setup

1. Update the `driver_account` property on line 11 of `./defs/config-aws-account-def.yaml`.

2. Create app
   ```
   humctl create app aws-iam-example
   ```
3. Register resource definitions
   ```
   humctl apply -f defs/
   ```
4. Deploy workload
   ```
   humctl score deploy -f score-a.yaml --app aws-iam-example --env development --wait
   ```

### Explore

Open the `aws-iam-example` app in the UI, select the development environment and review the resource graph

Try the following:

- add another workload
  ```
  humctl score deploy -f score-b.yaml --app aws-iam-example --env development --wait
  ```
  Review the new graph. Notice how there are now two roles, but still only one policy.

- Edit one of the score files to add an additional `s3` resource.


### Teardown

1. Delete the app
   ```
   humctl delete app aws-iam-example
   ```
2. Delete the resource definitions
   ```
   humctl delete -f defs/
   ```