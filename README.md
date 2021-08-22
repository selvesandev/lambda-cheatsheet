# lambda-cheatsheet

## Provider IAM Policy

You need to provide `iamRoleStatements` right under you `provider`

```yaml

provider:
  name: aws
  region: us-east-1
  ...
  iamRoleStatements:
    - Effect: "Allow" # allow lambda any action on any resouce.
      Action: 
        - "lambda:*"
      Resource:
        - "*"

```

`iamRoleStatements` to do anything on s3.

```yaml

provider:
  name: aws
  region: us-east-1
  ...
  iamRoleStatements:
    - Effect: "Allow"
      Action: 
        - "s3:*"
      Resource: "*"

```



## Environment Variables.
Environment variable blog goes to `provider`
```yaml
provider:
  name: aws
  region: us-east-1qq
  ...
  environment:
    ENV_VARIABLE: "HELLO WORLD"
```

You can also define the environment variable in your function block this will override the global environment variable.

```yaml

functions:
  msAuthenticator:
    handler: src/handler.lambda_handler
    environment:
      ENV_VARIABLE: "HELLO WORLD"
```


Access the environment variable by 
```
import os

print(os.getenv('ENV_VARIABLE')) #python
console.log(process.env.ENV_VARIABLE) #JS
```



## Custom Variables.

You can define custom variable in your serverless.yaml file in a `custom` block and use it anywhere inside you serverless.yaml file as `${self:custom.variable_name}`

```yaml

custom:
  bucket: my-bucket-name
  pythonRequirements:
    dockerizePip: true


```



## Events Block (Method to trigger lambda)
Here example to trigger the lambda with s3 upload and api gateway.

```yaml

functions:
  my-function-name:
    handler: src/handler.index
    events:
      - http:
          method: POST
          path: /export

```

```yaml

functions:
  my-function-name:
    handler: src/handler.index
    events:
      - s3:
          bucket: ${self.custom.bucket}
          path: /export
          
```



## VPC for Lambda


