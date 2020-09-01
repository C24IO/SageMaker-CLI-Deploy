# SageMaker-CLI-Deploy
## Deploy an endpoint using boto3 CLI

### First create a model config 

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

```bash 
aws sagemaker create-model —cli-input-json file://create_model.json
```

and to add environment variables:

```json
"Environment": {
        "SAGEMAKER_SUBMIT_DIRECTORY" : "[Full S3 path for *.tar.gz file containing the training script]"
    }
```        
or 
```json
 "Environment": {
            "SAGEMAKER_PROGRAM": "inference.py",
            "SAGEMAKER_SUBMIT_DIRECTORY": "/opt/ml/model/code",
            "SAGEMAKER_CONTAINER_LOG_LEVEL": "20",
            "SAGEMAKER_ENABLE_CLOUDWATCH_METRICS": "FALSE",
            "SAGEMAKER_REGION": "<insert your region>",
            "MMS_DEFAULT_RESPONSE_TIMEOUT": "500"
        }
```        
Then create endpoint config 


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

Use this file to create endpoint config

```bash
aws sagemaker create-endpoint-config --cli-input-json file://create_config.json
```

Finally deploy the endpoint

```bash
aws sagemaker create-endpoint --endpoint-name '<provide your endpoint name>' --endpoint-config-name '<insert your endpoint config name>'
```


Thanks to - [Pratyush Bagaria](https://github.com/prats13bag)