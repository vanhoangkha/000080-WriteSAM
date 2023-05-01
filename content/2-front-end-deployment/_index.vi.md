---
title : "Triển khai front-end"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---
Bước phần này, chúng ta sẽ tạo một S3 bucket với Static web hosting được kích hoạt và được truy cập công cộng dựa trên SAM:

1. Mở tệp **template.yaml** trong thư mục **fcj-book-store** mà chúng ta vừa tạo ở phần 1.
- Xoá phần không cần thiết:

![CreateS3Bucket](/images/2-front-end-deployment/2-front-end-deployment-1.png?featherlight=false&width=90pc)

2. Sao chép đoạn script sau vào tệp đó: 
```
FcjBookStore:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: fcj-book-store
      PublicAccessBlockConfiguration:
        BlockPublicAcls: 'false'
        BlockPublicPolicy: 'false'
      WebsiteConfiguration:
        IndexDocument: index.html

  FcjBookStorePolicy:
    Type: AWS::S3::BucketPolicy
    Properties:
      Bucket: !Ref FcjBookStore
      PolicyDocument:
        Version: 2012-10-17
        Statement:
          - Action: 
              - 's3:GetObject'
            Effect: Allow
            Principal: '*'
            Resource: !Join
              - ''
              - - 'arn:aws:s3:::'
                - !Ref FcjBookStore
                - /*
```
Đoạn script định nghĩa một S3 bucket **fcj-book-store** với policy là **FcjBookStorePolicy** - cho phép truy cập công cộng

![CreateBucket](/images/2-front-end-deployment/2-front-end-deployment-2.png?featherlight=false&width=90pc)

3. Chạy dòng lệnh dưới đây để build tại thư mục của SAM project: **fcj-book-store**
```
sam build
```

![CreateBucket](/images/2-front-end-deployment/2-front-end-deployment-3.png?featherlight=false&width=90pc)

- Chạy dòng lệnh sau để kiểm tra tính chính xác của SAM template
```
sam validate
```

![CreateBucket](/images/2-front-end-deployment/2-front-end-deployment-4.png?featherlight=false&width=90pc)

- Chạy dòng lệnh dưới để triển khai SAM
```
sam deploy --guided
```
- Nhập tên cho stack: **fcj-book-store**
- Nhập vùng mà bạn muốn triển khai, ví dụ: **ap-southeast-1**
- Sau đó nhập các thông tin khác như hình dưới đây:

![CreateBucket](/images/2-front-end-deployment/2-front-end-deployment-5.png?featherlight=false&width=90pc)

- Đợi một lúc để tạo CloudFormation stack changeset
- Nhập "y" khi được hỏi **Deploy this changeset?**

![CreateBucket](/images/2-front-end-deployment/2-front-end-deployment-6.png?featherlight=false&width=90pc)

4. Mở bảng điều khiển của [Amazon S3](https://s3.console.aws.amazon.com/s3/buckets?region=ap-southeast-1&region=ap-southeast-1)

![CheckBucket](/images/2-front-end-deployment/2-front-end-deployment-7.png?featherlight=false&width=90pc)

5. Kiểm tra xem bucket đã được tạo hay chưa.
- Ấn vào bucket **fcj-book-store**

![CheckBucket](/images/2-front-end-deployment/2-front-end-deployment-8.png?featherlight=false&width=90pc)

6. Ấn sang tab **Properties**

![CheckBucket](/images/2-front-end-deployment/2-front-end-deployment-9.png?featherlight=false&width=90pc)

7. Kéo xuống cuối trang, kiểm tra trạng thái của **Static website hosting**
- Ghi lại endpoint của website
![CheckBucket](/images/2-front-end-deployment/2-front-end-deployment-10.png?featherlight=false&width=90pc)

8. Ấn sang tab **Permissions**
- Thấy policy đã được thêm

![CheckBucket](/images/2-front-end-deployment/2-front-end-deployment-11.png?featherlight=false&width=90pc)

9. Mở bảng điều khiển của [CloudFormation](https://ap-southeast-1.console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks?filteringStatus=active&filteringText=&viewNested=true&hideStacks=false)

![CheckCloudFormation](/images/2-front-end-deployment/2-front-end-deployment-12.png?featherlight=false&width=90pc)

10. Hai stack đã được tạo
- Ấn vào stack **fcj-book-store**

![CheckCloudFormation](/images/2-front-end-deployment/2-front-end-deployment-13.png?featherlight=false&width=90pc)

10. Ấn sang tab **Resource**, thấy những resource mà CloudFormation đã khởi tạo

![CheckCloudFormation](/images/2-front-end-deployment/2-front-end-deployment-14.png?featherlight=false&width=90pc)

- Ấn sang stack còn lại:

![CheckCloudFormation](/images/2-front-end-deployment/2-front-end-deployment-15.png?featherlight=false&width=90pc)

11. Tải code **fcj-serverless-frontend** về máy của bạn
- Mở terminal trên máy bạn tại thư mục bạn muốn lưu source code
- Sao chép câu lệnh dưới đây
```
git clone https://github.com/AWS-First-Cloud-Journey/FCJ-Serverless-Workshop.git
cd FCJ-Serverless-Workshop
yarn build
```
12. Chúng ta đã build xong front-end. Tiếp theo thực hiện câu lệnh sau để tải thư mục **build** lên S3
```
aws s3 cp build s3://fcj-book-store --recursive
```
Kết quả sau khi tải xong:

![SettingBucket](/images/2-front-end-deployment/2-front-end-deployment-16.png?featherlight=false&width=90pc)



