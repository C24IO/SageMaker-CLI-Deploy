# SageMaker-CLI-Deploy
## Deploy an endpoint using boto3 CLI

### Step 1: define create_model.json
```json
{
    "ModelName": "my-sagemaker-model",
    "PrimaryContainer": {
        "Image": "<insert the ECR Image URI>",
        "ModelDataUrl": "s3://path/to/model/artifact/model.tar.gz",
        "Environment": {}
    },
    "ExecutionRoleArn": "<ARN for AmazonSageMaker-ExecutionRole>"
}
```

if required, include the  following environment variables:
```json
 "Environment": {
            "SAGEMAKER_PROGRAM": "<specify your entry-point.py script>",
            "SAGEMAKER_SUBMIT_DIRECTORY": "/opt/ml/model/code",
            "SAGEMAKER_REGION": "<insert your region>",
            "SAGEMAKER_ENABLE_CLOUDWATCH_METRICS": "FALSE",
            "SAGEMAKER_CONTAINER_LOG_LEVEL": "20"            
        }
```        

### Step 2: create sagemaker model using create_model.json
```bash 
aws sagemaker create-model --model-name <<model name>>  --primary-container  Image=<<Docker image location>>  --execution-role-arn <<Role ARN>> --region <<region>>
```

### Step 3: define create_config.json

```json
{
    "EndpointConfigName": "<provide your endpoint config name>",
    "ProductionVariants": [
        {
            "VariantName": "<provide your variant name>",
            "ModelName": "my-sagemaker-model",
            "InitialInstanceCount": 1,
            "InstanceType": "<provide your instance type here>",
            "InitialVariantWeight": 1.0
        }
    ]
}        
```

### Step 4: create sagemaker endpoint configuration using create_config.json
```bash
aws sagemaker create-endpoint-config --cli-input-json file://create_config.json
```

### Step 5: deploy the endpoint

```bash
aws sagemaker create-endpoint --endpoint-name '<provide your endpoint name>' --endpoint-config-name '<insert your endpoint config name>'
```

Thanks to - [Pratyush Bagaria](https://github.com/prats13bag) for this code snippet. 

Security config and VPC config using CLI 