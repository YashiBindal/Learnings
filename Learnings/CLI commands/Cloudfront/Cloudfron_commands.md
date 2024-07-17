## Steps to follow

1. use the command get-distribution-config to get the get the current configuration and an Etag header for the distribution.
2. update the current configuration file that was returned in the response to your get-distribution-config CLI command to include your changes of the origin.
3. execute the update-distribution command to update the configuration for your distribution:
   you must use the --if-match option to provide the distribution's ETag you obtained in step 1.
   you must use the --distribution-config option to provide the updated configuration file you updated in step 2.
4. Review the response to the update-distribution command to confirm that the configuration was successfully updated.
5. Optional:
   execute a get-distribution-config command to confirm that your changes have propagated.
   when propagation is complete, the value of "Status" is "Deployed".


## Command to follow to update distribution:

- `aws cloudfront get-distribution-config --id REHDJIDE23VKL`

- ```aws cloudfront update-distribution \
   --id REHDJIDE23VKL \
   --if-match ETJER9LSMDM0H \
   --distribution-config file://cloudfront_update_distribution.json
   ```
## Command to get, create and update cache policy
- aws cloudfront get-cache-policy --id ba7a4312-8a3e-4853-875b-3127fa27a335
- aws cloudfront create-cache-policy --cache-policy-config file://ChachePolicy.json
- aws cloudfront update-cache-policy --cache-policy-config file://ChachePolicy.json --id ba7a4312-8a3e-4853-875b-3127fa27a335 --if-match E1F83G8C2ARO7P

## Command to get, create and update response header policy
- aws cloudfront get-response-headers-policy --id 3c7bbdee-464c-44c4-aeb4-b6f7d39dae23
- aws cloudfront create-response-headers-policy --response-headers-policy-config file://ResponseHeaderPolicy.json
= aws cloudfront update-response-headers-policy --response-headers-policy-config file://cloudfrontHeaderPolicy.json --id 3c7bbdee-464c-44c4-aeb4-b6f7d39dae23 --if-match E3AEGXETSR30VB

## command to get, create and update origin request policy
aws cloudfront get-origin-request-policy --id 43ca56a9-7198-450b-b169-158c467702c3

- aws cloudfront create-origin-request-policy --origin-request-policy-config file://originRequestPolicy.json
- 
- aws cloudfront update-origin-request-policy --origin-request-policy-config file://originRequestPolicy.json --id 43ca56a9-7198-450b-b169-158c467702c3 --if-match E23ZP02F085DFQ

## References:

- https://docs.aws.amazon.com/cli/latest/reference/cloudfront/index.htmlcli-aws-cloudfront
- https://docs.datadoghq.com/security_platform/default_rules/aws-cloudfront-security-policy/
