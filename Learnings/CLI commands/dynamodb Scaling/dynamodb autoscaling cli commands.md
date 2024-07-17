
## 1. Command to update table provisioned capacity

```
aws dynamodb update-table --table-name table_name --billing-mode PROVISIONED --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5 --global-secondary-index-updates file://gsi-updates.json

```
- File contents
```
[
  {
    "Update": {
      "IndexName": "id-index",
      "ProvisionedThroughput": {
        "ReadCapacityUnits": 5,
        "WriteCapacityUnits": 5
      }
    }
  }
]
``` 

## 2. Command for DYnamodb autoscaling policy for RCU
- ```
  aws application-autoscaling put-scaling-policy --service-namespace dynamodb --scalable-dimension dynamodb:table:ReadCapacityUnits --resource-id table/table_name --policy-name target-tracking-scaling-policy --policy-type TargetTrackingScaling --target-tracking-scaling-policy-configuration file://config.json
  ```
- File contents
```
{
    "TargetValue": 20.0,
    "PredefinedMetricSpecification": {
      "PredefinedMetricType": "DynamoDBReadCapacityUtilization"
    },
    "ScaleOutCooldown": 10,
    "ScaleInCooldown": 15
  }
```

- ```aws application-autoscaling register-scalable-target --service-namespace dynamodb --resource-id table/table_name --scalable-dimension dynamodb:table:ReadCapacityUnits     --min-capacity 5 --max-capacity 10
  ```

## 2. Command for DYnamodb autoscaling policy for WCU
-  ```
  aws application-autoscaling put-scaling-policy --service-namespace dynamodb --scalable-dimension dynamodb:table:WriteCapacityUnits --resource-id table/table_name --policy-name target-tracking-scaling-policy --policy-type TargetTrackingScaling --target-tracking-scaling-policy-configuration file://configwrite.json
  ``` 
- File Contents
```
{
    "TargetValue": 20.0,
    "PredefinedMetricSpecification": {
      "PredefinedMetricType": "DynamoDBWriteCapacityUtilization"
    },
    "ScaleOutCooldown": 60,
    "ScaleInCooldown": 60
  }
```
-  ```
  aws application-autoscaling register-scalable-target --service-namespace dynamodb --resource-id table/table_name --scalable-dimension dynamodb:table:WriteCapacityUnits     --min-capacity 5 --max-capacity 10
  ````
