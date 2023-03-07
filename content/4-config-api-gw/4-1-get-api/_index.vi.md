---
title : "Tạo GET API"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 4.1. </b> "
---
1. Mở tệp **template.yaml** trong thư mục **fcj-book-store**
2. Thêm đoạn script sau vào cuối tệp tạo một REST API và GET method
```
  BookApi:
    Type: AWS::Serverless::Api
    Name: fcj-serverless-api
    Properties:
      StageName: staging
      Cors: "'*'"      # enable CORS for API
      DefinitionBody:
        openapi: 3.0.1
        info:
          description: "This is the APIs for book store web app"
          version: "1.0.0"
          title: "API Gateway REST API to Lambda"
        paths:
          /books:
            get:
              responses:
                "200":
                  description: 200 response
                  headers:
                    Access-Control-Allow-Origin:
                      type: string
              x-amazon-apigateway-integration:
                uri:
                  Fn::Sub: "arn:aws:apigateway:${AWS::Region}:lambda:path/2015-03-31/functions/${BooksList.Arn}/invocations"
                responses:
                  default:
                    statusCode: 200
                    responseParameters:
                      method.response.header.Access-Control-Allow-Origin: "'*'"
                passthroughBehavior: when_no_match
                httpMethod: POST #always POST
                type: aws_proxy
```
![CreateGetAPI](/images/4-config-api-gw/4-config-api-gw-1.png?featherlight=false&width=90pc)

- Thêm đoạn script sau vào cuối của function **BooksList** 
```
      Events:
        ListBook:
          Type: Api
          Properties:
            Path: /books/
            Method: get
            RestApiId:
              Ref: BookApi
```

![CreateGetAPI](/images/4-config-api-gw/4-config-api-gw-2.png?featherlight=false&width=90pc)

2. Chạy dòng lệnh dưới đây triển khai SAM
```
sam build
sam deploy
```
{{% notice note %}}
Nhập "y" nếu được hỏi "BooksList may not have authorization defined, Is this okay? [y/N]: "
{{% /notice %}}

![CreateGetAPI](/images/4-config-api-gw/4-config-api-gw-3.png?featherlight=false&width=90pc)

3. Mở bảng điều khiển của function **books_list**
- Ấn vào **API Gateway**

![CreateGetAPI](/images/4-config-api-gw/4-config-api-gw-4.png?featherlight=false&width=90pc)

4. Hiện thị API Gateway đang được tương tác với function
- Ấn vào API Gateway đó

![CreateGetAPI](/images/4-config-api-gw/4-config-api-gw-5.png?featherlight=false&width=90pc)

5. Hiện thị các resource và GET method

![CreateGetAPI](/images/4-config-api-gw/4-config-api-gw-6.png?featherlight=false&width=90pc)

6. Chọn tab **Stages** ở menu phía bên trái
- Ấn chọn **staging**
- Ấn chọn **GET**
- Ghi lại **InvokeURL** của method GET

![CreateGetAPI](/images/4-config-api-gw/4-config-api-gw-7.png?featherlight=false&width=90pc)






