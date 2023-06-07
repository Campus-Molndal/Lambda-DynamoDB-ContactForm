# Lambda - DynamoDB - ContactForm

The goal of this exercise is to create a contact form on a web page and submit that information to a back-end database via an API.

The following four AWS services are used:

- The web page is hosted on _S3_
- The API is implemented by _API Gateway_
- The backend is written in _Python_ and uses _Lambda_
- The database is a _DynamoDB_ table

## Roles

* Create a Lambda service role named _ContactRole_
* Select two managed permission policies:
    - AWSLambdaBasicExecutionRole
    - AmazonDynamoDBFullAccess

## DynamoDB

* Create a DynamoDB table and name it _ContactTable_
* Set "timestamp" as partition key. Select String

## Lambda

* Create a Lambda function in python named _ContactFunction_
* Author from scratch
* Select the role created earlier

* Create a test event named "ContactTest"
* Use the following payload
```json
{
    "httpMethod": "POST",
    "body": "{\"name\": \"Darth\", \"email\": \"darth.vader@email.com\", \"msg\": \"I am your ...\"}"
}
```

* Create another test event named _ContactTestFailure_
* Use the following payload
```json
{

}
```

* Run the tests and verify the expected behaviour. The first should result in a new row in DynamoDB.

## API Gateway

* Create a new API gateway by pressing the <+ _Add trigger_> button in the Console Lambda UI
* Select REST API
* Select "Open" security

* Go to the API in the console and run a test
* Select "POST" method
* Use the following Request Body
```json
{
    "name": "Luke",
    "email": "luke.skywalker@email.com",
    "msg": "May the ..."
}
```

* Verify a Status 200 and a message saying:
> "Successfully saved contact info!"

* Verify the entry in DynamoDB

* Enable CORS for the API

* Update the _index.html_ file with you API Gateway endpoint and verify that it works by loading the file in a web browser

## S3

* Create an S3 Bucket called ```contactweb-<account>-<date>-<time>```

* Go to the properties tab in the Console UI and enable _Static webbsite hosting_

* Fill in _index.html_ as index document

* Find the newly created URL for your website

* Go to the permissions tab in the Console UI
[AWS Doc](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteAccessPermissionsReqd.html)
    - disable "Block all public access"
    - add bucket policy and replace the "Bucket ARN" in the example
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::Bucket-Name/*"
            ]
        }
    ]
}
```

* Upload the _index.html_ file to the bucket and verify that it works by browsing to the S3 website

* Fill in:
    - Yoda
    - yoda@email.com
    - Do. Or do not. There is no try

<br>
* Check the DynamoDB table