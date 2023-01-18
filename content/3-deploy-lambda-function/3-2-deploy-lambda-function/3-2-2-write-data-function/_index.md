---
title : "Writing Lambda function"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 3.2.1 </b> "
---
Trong bước này chúng ta tạo một Lambda function mới để ghi dữ liệu vào DynamoDB trên SAM

1. Open **template.yaml** file in **fcj-book-store** folder
2. Add the following script at the end of the file to create a Lambda function that writes data to DynamoDB
```
  BookCreate:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: fcj-book-store/book_create
      Handler: book_create.lambda_handler
      Runtime: python3.9
      FunctionName: book_create
      Architectures:
        - x86_64
      Policies:
        - Statement:
            - Sid: BookCreateItem
              Effect: Allow
              Action:
                - dynamodb:PutItem
                - s3:PutObject
              Resource:
                - !Sub arn:aws:dynamodb:${AWS::Region}:${AWS::AccountId}:table/Books
                - arn:aws:s3:::book-image-store/*
```
- Add the following script at the end of the file to create an S3 bucket that stores uploaded image files
```
  BookImageStore:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: book-image-store
```
![LambdaCreateFunction](/images/3-deploy-lambda-function/3-2-deploy-lambda-function-6.png?featherlight=false&width=90pc)

3. The directory structure is as follows:
```
fcj-book-store
├── fcj-book-store
│   ├── books_list
│   │   └── books_list.py
│   ├── book_create
│       └── book_create.py
└── template.yaml

```
- Create **book_create** folder in **fcj-book-store/fcj-book-store/** folder
- Create **book_create.py** file and copy the below code block to it
```
import boto3
import json
import base64
import io
import cgi
import os

s3 = boto3.client('s3')
client = boto3.resource('dynamodb')
runtime_region = os.environ['AWS_REGION']

def get_data_from_request_body(content_type, body):
    fp = io.BytesIO(base64.b64decode(body)) # decode
    environ = {"REQUEST_METHOD": "POST"}
    headers = {
        "content-type": content_type,
        "content-length": len(body),
    }

    fs = cgi.FieldStorage(fp=fp, environ=environ, headers=headers) 
    return [fs, None]
    
def lambda_handler(event, context):
    content_type = event['headers'].get('Content-Type', '') or event['headers'].get('content-type', '')
    
    if content_type == 'application/json':
        book_item = json.loads(event["body"])
    else:
        book_data, book_data_error = get_data_from_request_body(
            content_type=content_type, body=event["body"]
        )
    
        name = book_data['image'].filename
        image = book_data['image'].value
        s3.put_object(Bucket='book-image-store', Key=name, Body=image)
        image_path = "https://{}.s3.{}.amazonaws.com/{}".format("book-image-resize-store", runtime_region, name)
        
        book_item = {
            "id": book_data['id'].value,
            "rv_id": 0,
            "name": book_data['name'].value,
            "author": book_data['author'].value,
            "price" : book_data['price'].value,
            "category": book_data['category'].value,
            "description": book_data['description'].value,
            "image": image_path
        }
    
    table = client.Table('Books')
    table.put_item(Item = book_item)
    
    response = {
        'statusCode': 200,
        'body': 'successfully created item!',
        'headers': {
            'Content-Type': 'application/json',
            "Access-Control-Allow-Headers": "Access-Control-Allow-Headers, Origin, Accept, X-Requested-With, Content-Type, Access-Control-Request-Method,X-Access-Token, XKey, Authorization",
            "Access-Control-Allow-Origin": "*",
            "Access-Control-Allow-Methods": "GET,PUT,POST,DELETE,OPTIONS"
        },
    }
  
    return response
```

4. Run the following command to deploy SAM
```
sam build
sam deploy --guided
```

![LambdaCreateFunction](/images/3-deploy-lambda-function/3-2-deploy-lambda-function-7.png?featherlight=false&width=90pc)

5. Back to AWS Lambda console
- Click **book_create** function created

![LambdaCreateFunction](/images/3-deploy-lambda-function/3-2-deploy-lambda-function-8.png?featherlight=false&width=90pc)

6. Click **Configuration** tab
- Select **Permissions** on the left menu
- Click on the role that the function is executing

![LambdaCreateFunction](/images/3-deploy-lambda-function/3-2-deploy-lambda-function-9.png?featherlight=false&width=90pc)

7. Expand the policy, see the permissions granted to the function

![LambdaCreateFunction](/images/3-deploy-lambda-function/3-2-deploy-lambda-function-10.png?featherlight=false&width=90pc)

8. Open [Amazon S3 console](https://s3.console.aws.amazon.com/s3/buckets?region=ap-southeast-1&region=ap-southeast-1), the **book-image-store** bucket has been created

![LambdaCreateFunction](/images/3-deploy-lambda-function/3-2-deploy-lambda-function-11.png?featherlight=false&width=90pc)

