---
title : "Tạo POST API"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 4.2. </b> "
---
1. Mở tệp **template.yaml** trong thư mục **fcj-book-store**
2. Thêm đoạn script sau vào cuối tệp tạo method POST
```
            post:
              responses:
                "200":
                  description: 200 response
                  headers:
                    Access-Control-Allow-Origin:
                      type: string
              x-amazon-apigateway-integration:
                uri:
                  Fn::Sub: "arn:aws:apigateway:${AWS::Region}:lambda:path/2015-03-31/functions/${BookCreate.Arn}/invocations"
                responses:
                  default:
                    statusCode: 200
                    responseParameters:
                      method.response.header.Access-Control-Allow-Origin: "'*'"
                passthroughBehavior: when_no_match
                httpMethod: POST #always POST
                type: aws_proxy
```
![CreatePostAPI](/images/4-config-api-gw/4-config-api-gw-8.png?featherlight=false&width=90pc)

- Thêm đoạn script sau vào cuối của function **BookCreate** 
```
      Events:
        CreateBook:
          Type: Api
          Properties:
            Path: /books/
            Method: post
            RestApiId:
              Ref: BookApi
```

![CreatePostAPI](/images/4-config-api-gw/4-config-api-gw-9.png?featherlight=false&width=90pc)

2. Chạy dòng lệnh dưới đây triển khai SAM
```
sam build
sam deploy --guided
```

{{% notice note %}}
Nhập "y" nếu được hỏi "BookCreate may not have authorization defined, Is this okay? [y/N]:"
{{% /notice %}}

![CreatePostAPI](/images/4-config-api-gw/4-config-api-gw-10.png?featherlight=false&width=90pc)

3. Mở bảng điều khiển của function **book_create**
- Ấn vào **API Gateway**

![CreatePostAPI](/images/4-config-api-gw/4-config-api-gw-11.png?featherlight=false&width=90pc)

4. Hiện thị API Gateway đang được tương tác với function
- Ấn vào API Gateway đó

![CreatePostAPI](/images/4-config-api-gw/4-config-api-gw-12.png?featherlight=false&width=90pc)

5. Hiện thị các resource và method POST 

![CreatePostAPI](/images/4-config-api-gw/4-config-api-gw-13.png?featherlight=false&width=90pc)

6. Chọn tab **Stages** ở menu phía bên trái
- Ấn chọn **staging**
- Ấn chọn **POST**
- Ghi lại **InvokeURL** của method POST

![CreatePostAPI](/images/4-config-api-gw/4-config-api-gw-14.png?featherlight=false&width=90pc)
