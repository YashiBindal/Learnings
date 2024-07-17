## 1. Create the function 
```
aws cloudfront create-function \
      --name security-headers \
      --function-config Comment="Security headers function",Runtime="cloudfront-js-1.0" \
      --function-code fileb://function.js
```
Command Response:
```
{
    "Location": "https://cloudfront.amazonaws.com/2020-05-31/function/arn:aws:cloudfront::523652376342:function/security-headers",
    "ETag": "ETVPDKIKX0DER",
    "FunctionSummary": {
        "Name": "security-headers",
        "Status": "UNPUBLISHED",
        "FunctionConfig": {
            "Comment": "Security headers function",
            "Runtime": "cloudfront-js-1.0"
        },
        "FunctionMetadata": {
            "FunctionARN": "arn:aws:cloudfront::523652376342:function/security-headers",
            "Stage": "DEVELOPMENT",
            "CreatedTime": "2023-06-22T09:58:06.780Z",
            "LastModifiedTime": "2023-06-22T09:58:06.780Z"
        }
    }
}
```

## 2. Test the created function

```
aws cloudfront test-function \
    --name security-headers \
    --if-match ETVPDKIKX0DER \
    --event-object fileb://event-object.json \
    --stage DEVELOPMENT
```

Command response:

- An error occurred (TestFunctionFailed) when calling the TestFunction operation (reached max retries: 2): The provided event object is not valid.

## 3. Publish the created function
```
aws cloudfront publish-function \
    --name security-headers \
    --if-match ETVPDKIKX0DER
```

Command response:
```
{
    "FunctionSummary": {
        "Name": "security-headers",
        "Status": "UNASSOCIATED",
        "FunctionConfig": {
            "Comment": "Security headers function",
            "Runtime": "cloudfront-js-1.0"
        },
        "FunctionMetadata": {
            "FunctionARN": "arn:aws:cloudfront::523652376342:function/security-headers",
            "Stage": "LIVE",
            "CreatedTime": "2023-06-22T10:18:12.259Z",
            "LastModifiedTime": "2023-06-22T10:18:12.259Z"
        }
    }
}
```

## 4. Attach the function to cloudfront distribution

```
aws cloudfront get-distribution-config \
    --id E2LJ4ZLKLY4335 \
    --output yaml > dist-config.yaml \
    --debug
```

Command Response: dist-config.yaml was generated 

## Apply below changes to Contents of dist-config.yaml
- replace ETag key with IfMatch
- edit FunctionAssociations with below value
```
FunctionAssociations:
  Items:
    - EventType: viewer-response
      FunctionARN: arn:aws:cloudfront::523652376342:function/security-headers
  Quantity: 1
```
- run update distribution commmand
```
aws cloudfront update-distribution \
    --id E2LJ4ZLKLY4335 \
    --cli-input-yaml file://dist-config.yaml
```


