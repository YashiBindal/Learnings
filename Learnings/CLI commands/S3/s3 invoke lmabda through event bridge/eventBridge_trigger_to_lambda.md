## To trigger multiple lambda from same s3 locations using event bridge

1. Enable event bridge notification in  s3. If already enabled skip this.
```
aws s3api put-bucket-notification-configuration \
    --bucket data-source \
    --notification-configuration file://notification_config.json
```

2. Create event bridge rule:
```
 aws events put-rule --name "s3_files_invoke_trigger" --event-pattern "{\"source\": [\"aws.s3\"],\"detail-type\": [\"Object Created\"],\"detail\": {\"bucket\": {\"name\": [\"data-source\"]},\"object\": {\"key\": [{\"prefix\": \"file_path_prefix\"}]}}}"
```

3. Add permission to invoke lambda to rule:

```
aws lambda add-permission \
--function-name file_process_lambda \
--statement-id file_process_lambda_invoke_rule \
--action 'lambda:InvokeFunction' \
--principal events.amazonaws.com \
--source-arn arn:aws:events:ap-south-1:251899213637:rule/s3_files_invoke_trigger
```

4. set target for the rule:

```
aws events put-targets --rule s3_files_invoke_trigger --targets "Id"="1","Arn"="arn:aws:lambda:ap-south-1:251899213637:function:file_process_lambda"
```
