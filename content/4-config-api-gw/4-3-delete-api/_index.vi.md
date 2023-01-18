---
title : "Tạo DELETE API"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 4.3. </b> "
---
1. Mở tệp **template.yaml** trong thư mục **fcj-book-store**
2. Thêm đoạn script sau vào cuối tệp tạo một method DELETE
```
          /books/{id}:
            delete:
              responses:
                "200":
                  description: 200 response
                  headers:
                    Access-Control-Allow-Origin:
                      type: string
              x-amazon-apigateway-integration:
                uri:
                  Fn::Sub: "arn:aws:apigateway:${AWS::Region}:lambda:path/2015-03-31/functions/${BookDelete.Arn}/invocations"
                responses:
                  default:
                    statusCode: 200
                    responseParameters:
                      method.response.header.Access-Control-Allow-Origin: "'*'"
                passthroughBehavior: when_no_match
                httpMethod: POST #always POST
                type: aws_proxy
```
![CreateDeleteAPI](/images/4-config-api-gw/4-config-api-gw-15.png?featherlight=false&width=90pc)

- Thêm đoạn script sau vào cuối của function **BookDelete** 
```
      Events:
        DeleteBook:
          Type: Api
          Properties:
            Path: /books/{id}
            Method: delete
            RestApiId:
              Ref: BookApi
```

![CreateDeleteAPI](/images/4-config-api-gw/4-config-api-gw-16.png?featherlight=false&width=90pc)

2. Chạy dòng lệnh dưới đây triển khai SAM
```
sam build
sam deploy --guided
```

{{% notice note %}}
Nhập "y" nếu được hỏi "BookDelete may not have authorization defined, Is this okay? [y/N]: "
{{% /notice %}}

![CreateDeleteAPI](/images/4-config-api-gw/4-config-api-gw-17.png?featherlight=false&width=90pc)

3. Mở bảng điều khiển của function **book_delete**
- Ấn vào **API Gateway**

![CreateDeleteAPI](/images/4-config-api-gw/4-config-api-gw-18.png?featherlight=false&width=90pc)

4. Hiện thị API Gateway đang được tương tác với function
- Ấn vào API Gateway đó

![CreateDeleteAPI](/images/4-config-api-gw/4-config-api-gw-19.png?featherlight=false&width=90pc)

5. Hiện thị các resource và method DELETE

![CreateDeleteAPI](/images/4-config-api-gw/4-config-api-gw-20.png?featherlight=false&width=90pc)

6. Chọn tab **Stages** ở menu phía bên trái
- Ấn chọn **staging**
- Ấn chọn **DELTE**
- Ghi lại **InvokeURL** của method DELETE

![CreateDeleteAPI](/images/4-config-api-gw/4-config-api-gw-21.png?featherlight=false&width=90pc)

7. Thêm đoạn script sau vào cuối tệp **template.yaml** để API hỗ trợ các tệp Binary Media Types
```
      BinaryMediaTypes: 
        - multipart~1form-data
```
8. Chạy dòng lệnh dưới đây triển khai SAM
```
sam build
sam deploy --guided
```

![CreateDeleteAPI](/images/4-config-api-gw/4-config-api-gw-22.png?featherlight=false&width=90pc)

9. Trở lại với bảng điều khiển của API
- Chọn **Settings** ở menu phía bên trái
- Kéo xuống dưới, kiểm tra xem **multipart/form-data** đã được thêm hay chưa trong phần **Binary Meida Types**

![CreateDeleteAPI](/images/4-config-api-gw/4-config-api-gw-23.png?featherlight=false&width=90pc)


