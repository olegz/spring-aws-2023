# Spring Cloud Function on AWS example

This example provides an introduction to deploying and executing standard java functions as AWS Lambda Functions.
For more information on Spring Cloud Function please refer to our [reference manual](https://docs.spring.io/spring-cloud-function/docs/current/reference/html/), and for 
more information on integration with AWS Lambda function please refer to [this section](https://docs.spring.io/spring-cloud-function/docs/current/reference/html/aws.html) of the reference manual.
The example is based on Spring Boot 3 and requires Java 17+.

The example is also using [AWS SAM - Serverless Application Model](https://aws.amazon.com/serverless/sam/) to simplify build/deployment. The Pre-requisites section provides relevant links to download, install and setup SAM.

The example provides `template.yml` file in the root folder containing the application definition required by SAM.

Things of note in `template.yaml` file:

*  The definition of  _function name_  and the corresponding AWS  _handler_  (`FunctionInvoker`) provided by Spring Cloud Function. You will always use this invoker regardless of the function(s).

```
FunctionName: uppercase
      Handler: org.springframework.cloud.function.adapter.aws.FunctionInvoker::handleRequest
```

* The definition of the main Spring Boot  _configuration_  class

```
Environment:
        Variables:
          MAIN_CLASS: io.spring.sample.FunctionConfiguration
```

* The definition of the AWS API Gateway  _trigger_  for this AWS Lambda Function. 

```
Events:
        HttpApiEvent:
          Type: HttpApi
```

Please refer to relevant documentation from [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/java-samples.html), [AWS API Gateway](https://aws.amazon.com/api-gateway/) and [Spring Cloud Function](https://docs.spring.io/spring-cloud-function/docs/current/reference/html/) for more details on these concepts. 

## Pre-requisites
* [AWS CLI](https://aws.amazon.com/cli/)
* [SAM CLI](https://github.com/awslabs/aws-sam-cli)

## Deployment
In a shell, navigate to the sample's folder and use the SAM CLI to build a deployable package
```
$ sam build
```

This command compiles the application and prepares a deployment package in the `.aws-sam` sub-directory.

To deploy the application in your AWS account, you can use the SAM CLI's guided deployment process and follow the instructions on the screen

```
$ sam deploy --guided
```

Once the deployment is completed, the SAM CLI will print out the stack's outputs, including the new application URL. 
You can use `curl` or a web browser to make a call to the URL

```
...
---------------------------------------------------------------------------------------------------------
OutputKey-Description                        OutputValue
---------------------------------------------------------------------------------------------------------
PetStoreApi - URL for application            https://xxxxxxxxxx.execute-api.us-west-2.amazonaws.com/pets
---------------------------------------------------------------------------------------------------------

$ curl -H "Content-Type: application/json" -X POST -d '"foobar"' https://xxxxxxxxxx.execute-api.us-west-2.amazonaws.com/uppercase
```

. . . or you can also use Lambda Dashboard, navigate to a Test tab of your lambda and just click "test" button. 