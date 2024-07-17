## Command to update distribution:

- ```aws cloudfront get-distribution-config --id DHFEYR3REHFJ3```

- `````````
    aws cloudfront update-distribution \
    --id DHFEYR3REHFJ3 \
    --if-match E22JC533BT7K1G \
    --distribution-config file://cloudfront_new_distribution.json
    `````````

## Command to get,create and update cache policy

- ```aws cloudfront get-cache-policy --id ba7a4312-8a3e-4853-875b-3127fa27a335```
- ```aws cloudfront create-cache-policy --cache-policy-config file://ChacheConfig.json```
- ```aws cloudfront update-cache-policy --cache-policy-config file://chachePolicy.json --id ba7a4312-8a3e-4853-875b-3127fa27a335 --if-match E1F83G8C2ARO7P```

## Command to create response header policy

- ```aws cloudfront get-response-headers-policy --id 3c7bbdee-464c-44c4-aeb4-b6f7d39dae23```
- ```aws cloudfront create-response-headers-policy --response-headers-policy-config file://HeaderResponsePolicy.json```
- ```aws cloudfront update-response-headers-policy --response-headers-policy-config file://HeaderResponsePolicy.json --id 3c7bbdee-464c-44c4-aeb4-b6f7d39dae23 --if-match ETVPDKIKX0DER```

## command to get,create and update origin request policy

- ```aws cloudfront get-origin-request-policy --id 43ca56a9-7198-450b-b169-158c467702c3```
- ```aws cloudfront create-origin-request-policy --origin-request-policy-config file://originRequestPolicy.json```
- ```aws cloudfront update-origin-request-policy --origin-request-policy-config file://originRequestPolicy.json --id 43ca56a9-7198-450b-b169-158c467702c3 --if-match E23ZP02F085DFQ```
