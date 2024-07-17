1. Command To create API like app-uat
   aws apigateway create-rest-api --name 'app-uat' --region ap-south-1

2. Command to get REST API  : helps in getting parent id
   aws apigateway get-resources --rest-api-id ghhjhjkh --region ap-south-1

3. Command to create a resource like /sample-api

aws apigateway create-resource --rest-api-id ghhjhjkh \
--region ap-south-1 \
--parent-id mr3v7r9n62 \
--path-part sample-api-2

4. Command to create put method : will get resource-id on running above step3 or step 2 after creating sample-api API

aws apigateway put-method --rest-api-id ghhjhjkh \
--resource-id hdsdy3j \
--http-method POST \
--authorization-type "NONE" \
--region ap-south-1

5. Command to setup intergration request:

aws apigateway put-integration --rest-api-id ghhjhjkh \
--resource-id hdsdy3j --http-method POST \
--type AWS --integration-http-method POST \
--uri 'arn:aws:apigateway:ap-south-1:lambda:path/2015-03-31/functions/arn:aws:lambda:ap-south-1:574638763345:function:lambda_function_name/invocations' \
--timeout-in-millis 29000 \
--passthrough-behavior WHEN_NO_TEMPLATES \
--region ap-south-1 \
--request-templates  file://mappingTemplate \
--credentials arn:aws:iam::574638763345:role/uat-api-access-lambda-dynamodb


 ## Note:
--> above command integrates lambda function with API using '--uri', assign role to invoke lambda to API using '--credentials' and  sets Method request passthrough behavior in Integration request using '--passthrough-behavior' and '--request-templates'.

---> Below is content to be pasted inside mappingTemplate file. This file has no extension.

{"application/json": "##  See http://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-mapping-template-reference.html\n##  This template will pass through all parameters including path, querystring, header, stage variables, and context through to the integration endpoint via the body/payload\n#set($allParams = $input.params())\n{\n\"body-json\" : $input.json('$'),\n\"params\" : {\n#foreach($type in $allParams.keySet())\n    #set($params = $allParams.get($type))\n\"$type\" : {\n    #foreach($paramName in $params.keySet())\n    \"$paramName\" : \"$util.escapeJavaScript($params.get($paramName))\"\n        #if($foreach.hasNext),#end\n    #end\n}\n    #if($foreach.hasNext),#end\n#end\n},\n\"stage-variables\" : {\n#foreach($key in $stageVariables.keySet())\n\"$key\" : \"$util.escapeJavaScript($stageVariables.get($key))\"\n    #if($foreach.hasNext),#end\n#end\n},\n\"context\" : {\n    \"account-id\" : \"$context.identity.accountId\",\n    \"api-id\" : \"$context.apiId\",\n    \"api-key\" : \"$context.identity.apiKey\",\n    \"authorizer-principal-id\" : \"$context.authorizer.principalId\",\n    \"caller\" : \"$context.identity.caller\",\n    \"cognito-authentication-provider\" : \"$context.identity.cognitoAuthenticationProvider\",\n    \"cognito-authentication-type\" : \"$context.identity.cognitoAuthenticationType\",\n    \"cognito-identity-id\" : \"$context.identity.cognitoIdentityId\",\n    \"cognito-identity-pool-id\" : \"$context.identity.cognitoIdentityPoolId\",\n    \"http-method\" : \"$context.httpMethod\",\n    \"stage\" : \"$context.stage\",\n    \"source-ip\" : \"$context.identity.sourceIp\",\n    \"user\" : \"$context.identity.user\",\n    \"user-agent\" : \"$context.identity.userAgent\",\n    \"user-arn\" : \"$context.identity.userArn\",\n    \"request-id\" : \"$context.requestId\",\n    \"resource-id\" : \"$context.resourceId\",\n    \"resource-path\" : \"$context.resourcePath\"\n    }\n}\n"}



6. Command set headers in Method Response using put method response

#use when setting up integration for 1st time

command option 1

aws apigateway put-method-response --rest-api-id ghhjhjkh --resource-id ghsd6g --http-method POST --status-code 200 --region ap-south-1 --response-models '{"application/json":"Empty"}' --response-parameters '{"method.response.header.Access-Control-Allow-Headers":true,"method.response.header.Access-Control-Allow-Origin":true,"method.response.header.Access-Control-Allow-Methods":true}'

command option 2: passing response parameters directly instead of using json format

aws apigateway put-method-response --rest-api-id ghhjhjkh --resource-id hdsdy3j --http-method POST --status-code 200 --region ap-south-1 --response-models '{"application/json":"Empty"}' --response-parameters method.response.header.Access-Control-Allow-Headers=true,method.response.header.Access-Control-Allow-Origin=true,method.response.header.Access-Control-Allow-Methods=true

-------------Below commands for updating headers in existing APIs--------------------------------------------------

option 1: add headers individually

aws apigateway update-method-response --rest-api-id ghhjhjkh --resource-id hdsdy3j --http-method POST --status-code 200 --patch-operations op="add",path="/responseParameters/method.response.header.Access-Control-Allow-Headers",value="true"

aws apigateway update-method-response --rest-api-id ghhjhjkh --resource-id hdsdy3j --http-method POST --status-code 200 --patch-operations op="add",path="/responseParameters/method.response.header.Access-Control-Allow-Origin",value="true"

aws apigateway update-method-response --rest-api-id ghhjhjkh --resource-id hdsdy3j --http-method POST --status-code 200 --patch-operations op="add",path="/responseParameters/method.response.header.Access-Control-Allow-Methods",value="true"

option 2: Add all headers using json if an API is deployed previously i. e. for adding header to existing API

aws apigateway update-method-response --rest-api-id ghhjhjkh --resource-id hdsdy3j --http-method POST --status-code 200 --patch-operations "[{\"op\":\"add\",\"path\":\"/responseParameters/method.response.header.Access-Control-Allow-Headers\",\"value\":\"true\"},{\"op\":\"add\",\"path\":\"/responseParameters/method.response.header.Access-Control-Allow-Origin\",\"value\":\"true\"},{\"op\":\"add\",\"path\":\"/responseParameters/method.response.header.Access-Control-Allow-Methods\",\"value\":\"true\"}]"


7. Command to set header mapping in Integration Response

aws apigateway put-integration-response --rest-api-id ghhjhjkh --resource-id ghsd6g --http-method POST --status-code 200 --response-parameters '{"method.response.header.Access-Control-Allow-Headers":"'"'"'Content-Type,X-Amz-Date,Authorization,X-Api-Key,X-Amz-Security-Token,jtoken,jToken'"'"'","method.response.header.Access-Control-Allow-Origin":"'"'"'*'"'"'","method.response.header.Access-Control-Allow-Methods":"'"'"'POST'"'"'"}' --region ap-south-1

8. APi deployment command: To deploy an API the first time, or when you redeploy the API to a new stage.

aws apigateway create-deployment --region ap-south-1 --rest-api-id eu0jnmmvr6 --stage-name alpha


9. If deploying through yml template 

Validating and deploying the CloudFormation stack

aws cloudformation validate-template --template-body file://test_api.yml

aws cloudformation deploy --stack-name api-test-new --template-file test_api.yml --capabilities CAPABILITY_IAM

aws cloudformation create-stack --stack-name api-test-2 --tags Key=Project,Value=InstaGo 'Key=Owner,Value=Monish Parekh' Key=Environment,Value=Production Key=Application,Value=InstaGo  Key=Name,Value=api-test-2 --template-body file://apiMethod.yml --capabilities CAPABILITY_NAMED_IAM

10. deleting stack

aws cloudformation delete-stack --stack-name api-test-1


--------------------------------------------------------------------------------------------------------------------
## Commands to create Options Method with Mock endpoint

aws apigateway put-method --rest-api-id ghhjhjkh \
--resource-id ghsd6g \
--http-method OPTIONS \
--authorization-type "NONE" \
--region ap-south-1

aws apigateway put-integration --rest-api-id ghhjhjkh \
--resource-id ghsd6g --http-method OPTIONS \
--type MOCK --integration-http-method OPTIONS \
--timeout-in-millis 29000 \
--region ap-south-1 \
--request-templates '{ "application/json": "{\"statusCode\": 200}" }'

aws apigateway put-method-response --rest-api-id ghhjhjkh --resource-id ghsd6g --http-method OPTIONS --status-code 200 --region ap-south-1 --response-models '{"application/json":"Empty"}' --response-parameters '{"method.response.header.Access-Control-Allow-Headers":true,"method.response.header.Access-Control-Allow-Origin":true,"method.response.header.Access-Control-Allow-Methods":true}'

aws apigateway put-integration-response --rest-api-id ghhjhjkh --resource-id ghsd6g --http-method OPTIONS --status-code 200 --response-parameters '{"method.response.header.Access-Control-Allow-Headers":"'"'"'Content-Type,X-Amz-Date,Authorization,X-Api-Key,X-Amz-Security-Token,jtoken,jToken'"'"'","method.response.header.Access-Control-Allow-Origin":"'"'"'*'"'"'","method.response.header.Access-Control-Allow-Methods":"'"'"'POST'"'"'"}' --region ap-south-1


aws apigateway create-deployment --region ap-south-1 --rest-api-id ghhjhjkh --stage-name alpha