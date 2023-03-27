# Amazon API Gateway
Que es?


Caracteristicas
- Escalable y sin administración
- Soporta Loggin a nivel de API como con CloudTrail
- Manejo de nombres de dominio personalizados y certificados SSL (MTLS inclusive)
- Diferentes tipos de mecanismos para control de acceso
- Soporta Cacheo y Throtling
- Configurable para métodos HTTP (POST, PUT, GET, ANY,. OPTIONS, DELETE, HEAD)

Integraciones con otros servicios
- CloudTrail
- Route53
- Cognito
- AWS Certificate Manager (ACM)
- Amazon DynamoDB

## [Amazon API Gateway Hands on Lab](https://catalog.workshops.aws/general-immersionday/en-US/basic-modules/20-vpc/api-gateway)
Create a CloudFormation teplate file `./packaged-template.yaml`
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'SAM template for Serverless framework service: '
Resources:
  CalculateCostPerUnit:
    Properties:
      CodeUri: s3://serversless-immersion-api-gateway/a0dd3aca487ebd387f8ee70b6bc99382
      Handler: com.serverless.CostCalculator
      MemorySize: 128
      Runtime: java8
      Timeout: 15
    Type: AWS::Serverless::Function
  MedianPriceCalculator:
    Properties:
      CodeUri: s3://serversless-immersion-api-gateway/aa251f27b8bcf5de00dca96c8de879cf
      Handler: com.serverless.MedianPriceCalculator
      MemorySize: 128
      Runtime: java8
      Timeout: 15
    Type: AWS::Serverless::Function
Transform: AWS::Serverless-2016-10-31

```
Deploy the stack
```bash
aws cloudformation deploy --template-file ./packaged-template.yaml --stack-name serverless-immersion-day-stack --capabilities CAPABILITY_IAM
```

Continue with the API Gateway section\
Create your First API https://catalog.workshops.aws/general-immersionday/en-US/basic-modules/20-vpc/api-gateway/2-apigateway

Test the Endpoing
```json
{
  "price": "400000",
  "size": "1600",
  "unit": "sqFt",
  "downPayment": "20"
} 

```

then, continue creating a model https://catalog.workshops.aws/general-immersionday/en-US/basic-modules/20-vpc/api-gateway/3-apigateway

Model schema
```json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "title": "costCalculatorRequestModel",
  "type": "object",
  "properties": {
                  "price": { "type": "number" },
                  "size": { "type": "number" },
                  "unit": { "type": "string"}, 
                  "downPayment": { "type": "number"} 
  }
}
```

Mapping the template from request
```json
#set($inputRoot = $input.path("$"))
{
  "price": "$inputRoot.price",
  "size": "$inputRoot.size",
  "unit": "$inputRoot.unit",
  "downPaymentAmount": $inputRoot.downPayment
}
```

Apply authentication and Authorization with Amazon Cognito
https://catalog.workshops.aws/general-immersionday/en-US/basic-modules/20-vpc/api-gateway/5-apigateway



Useful commands with Amazon Cognito and AWS CLI
```bash
# login
aws cognito-idp admin-initiate-auth --user-pool-id us-east-1_bcb0JPv7u --client-id 7hg4upmlt1geppt5obu6m625tv --auth-flow ADMIN_NO_SRP_AUTH --auth-parameters 'USERNAME=xxx,PASSWORD="yyy"'

# Change Password for the user, using the previous Session token
aws cognito-idp admin-respond-to-auth-challenge --user-pool-id "us-east-1_bcb0JPv7u"  --client-id "7hg4upmlt1geppt5obu6m625tv" --challenge-name NEW_PASSWORD_REQUIRED --challenge-response "USERNAME=xxx,NEW_PASSWORD=yyy" --session "zzz"

```


## Workshops
- [Amazon API Gateway Hands on Lab](https://catalog.workshops.aws/general-immersionday/en-US/basic-modules/20-vpc/api-gateway)

## Resouces
- [La Amenaza del Nivel 100: Amazon API Gateway](https://youtu.be/p1otaxU9pOI)
- [Amazon API Gateway CORS Configurator (preview)](https://cors.serverlessland.com/)
- [Amazon API Gateway - The CORS Episode | Serverless Office Hours](https://www.youtube.com/watch?v=I8QVOjNuc3E)
- [Enable CORS on a resource using the API Gateway console](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-cors-console.html)
- [Configuring CORS for an HTTP API](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-cors.html)
- [Enable CORS for API Gateway Endpoint | AWS API Gateway Tutorial - 3](https://www.youtube.com/watch?v=Dm25vCPs8yA)

