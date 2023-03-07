---
title : "Lambda function chỉnh ảnh"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 3.2.4 </b> "
---
Trong bước này chúng ta tạo một Lambda function mới chỉnh kích thước ảnh sau khi người dùng tải lên.

1. Mở tệp **template.yaml** trong thư mục **fcj-book-store**
2. Thêm đoạn script sau vào cuối tệp để tạo một function chỉnh ảnh
```
ImageResizer:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: fcj-book-store/resize_image/function.zip
      PackageType: Zip
      Handler: index.handler
      Runtime: nodejs16.x
      FunctionName: resize_image
      Architectures:
        - x86_64
      Policies:
        - Statement:
            - Sid: ResizeUploadImage
              Effect: Allow
              Action:
                - s3:GetObject
                - s3:PutObject
                - s3:DeleteObject
              Resource:
                - arn:aws:s3:::book-image-store/*
                - arn:aws:s3:::book-image-resize-store/*
      Events:
        ResizeImage:
          Type: S3
          Properties:
            Bucket: !Ref BookImageStore
            Events: s3:ObjectCreated:*
      Environment:
        Variables:
          WIDTH: 200
          HEIGHT: 280
          DES_BUCKET: book-image-resize-store
```

{{% notice warning %}}
Nếu bạn tạo tên bộ chứa S3 khác với tên trong phòng thí nghiệm, vui lòng kiểm tra **Chính sách | Tài nguyên** hoặc **Môi trường** của tài nguyên và cập nhật.
{{% /notice %}}

- Thêm đoạn script sau vào cuối tệp để cấp quyền cho S3 bucket **books-image-store** sử dụng function này
```
LambdaInvokePermission:
    Type: "AWS::Lambda::Permission"
    Properties:
      FunctionName: !GetAtt ImageResizer.Arn
      Action: "lambda:InvokeFunction"
      Principal: "s3.amazonaws.com"
      SourceAccount: !Sub ${AWS::AccountId}
      SourceArn: !GetAtt BookImageStore.Arn
```

![LambdaResizeFunction](/images/3-deploy-lambda-function/3-2-deploy-lambda-function-21.png?featherlight=false&width=90pc)

3. Cấu trúc thư mục như sau:
```
fcj-book-store
├── fcj-book-store
│   ├── books_list
│   │   └── books_list.py
│   ├── book_create
│   │   └── book_create.py
│   ├── book_delete
│   │   └── book_delete.py
│   ├── resize_image
│       └── function.zip
└── template.yaml

```
- Tạo thư mục tên **resize_image** trong thư mục **fcj-book-store/fcj-book-store/**
- Tải tệp dưới đây và sao chép vào thư mục trên

{{%attachments title="Source code" pattern=".*\.(zip)$"/%}}

4. Chạy dòng lệnh dưới đây triển khai SAM
```
sam build
sam deploy
```

![LambdaResizeFunction](/images/3-deploy-lambda-function/3-2-deploy-lambda-function-22.png?featherlight=false&width=90pc)

5. Trở lại với bảng điều khiển của Lambda
- Ấn vào function **resize_image** đã được tạo

![LambdaResizeFunction](/images/3-deploy-lambda-function/3-2-deploy-lambda-function-23.png?featherlight=false&width=90pc)

6. Ấn sang tab **Configuration**
- Chọn mục **Trigger** ở bên menu bên trái, xuất hiện S3 bucket **book-image-store**

![LambdaResizeFunction](/images/3-deploy-lambda-function/3-2-deploy-lambda-function-26.png?featherlight=false&width=90pc)

7. Chọn mục **Permissions** ở menu bên trái
- Ấn vào role mà function đang thực hiện.

![LambdaResizeFunction](/images/3-deploy-lambda-function/3-2-deploy-lambda-function-24.png?featherlight=false&width=90pc)

8. Mở rộng policy, thấy các quyền đã được cấp cho function

![LambdaResizeFunction](/images/3-deploy-lambda-function/3-2-deploy-lambda-function-25.png?featherlight=false&width=90pc)



