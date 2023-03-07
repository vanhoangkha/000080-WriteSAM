---
title : "Chuẩn bị"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
Để bắt đầu xây dựng các ứng dụng dựa trên SAM, đầu tiên chúng ta phải cài đặt SAM CLI, cài đặt AWS credentials và khởi tạo một SAM application đơn giản.
1. Cài đặt SAM CLI cho hệ điều hành
- MacOS
```
brew tap aws/tap
brew install aws-sam-cli
sam --version
```
- Linux
```
pip install aws-sam-cli
sam --version
```
- Windows
	- Tải tệp cài đặt AWS SAM CLI [64-bit](https://github.com/aws/aws-sam-cli/releases/latest/download/AWS_SAM_CLI_64_PY3.msi)
	- Cài đặt tệp và kiểm tra version của SAM
```
sam --version
```

{{% notice note %}}
Bạn có thể tham khảo tại: https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html
{{% /notice %}}

2. Nếu bạn đã cài đặt AWS credentials từ những bài trước thì có thể bỏ qua bước này.
- Mở bảng điều khiển của IAM

![IAMConsole](/images/1-preparation/1-preparation-1.png?featherlight=false&width=90pc)

- Ấn vào **User** ở menu phía bên trái
- Ấn nút **Create user**

![IAMConsole](/images/1-preparation/1-preparation-2.png?featherlight=false&width=90pc)

- Nhập tên người dùng, ví dụ: `AdminSAMUser`
- Chọn **Password - AWS Mannagemant Console access** tại mục **Select AWS credentail type**
- Chọn **Custom password**, sau đó nhập mật khẩu của bạn
- Bỏ chọn mục **Require password reset**
- Ấn nút **Next: Permissions**

![IAMConsole](/images/1-preparation/1-preparation-3.png?featherlight=false&width=90pc)

- Chọn **Attach existing policies directly**
- Chọn policy **AdministratorAccess** để người dùng có toàn bộ quyền truy cập vào các service
- Ấn **Next: Tags**

![IAMConsole](/images/1-preparation/1-preparation-4.png?featherlight=false&width=90pc)

- Ấn **Next: Review**
- Xem lại các thiết lập, và ấn **Create user**

![IAMConsole](/images/1-preparation/1-preparation-5.png?featherlight=false&width=90pc)

- Ấn vào người dùng vừa được tạo.

![IAMConsole](/images/1-preparation/1-preparation-6.png?featherlight=false&width=90pc)

- Ấn sang tab **Security credentials**
- Ấn nút **Create access key**

![IAMConsole](/images/1-preparation/1-preparation-7.png?featherlight=false&width=90pc)

- Ấn nút **Download .csv file**

![IAMConsole](/images/1-preparation/1-preparation-8.png?featherlight=false&width=90pc)

- Chạy câu lệnh bằng terminal trên máy của bạn
```
aws configure
```
- Nhập thông tin tương ứng với các cột trong tệp credential mà bạn tải về
    - AWS Access key ID: Nhập access key ID
    - AWS Secret access key: Nhập secret access key
    - Default region name: Nhập region gần bạn nhất
    - Default output format: Có thể bỏ qua

3. Sau đó, khởi tạo một project SAM mẫu
- Chạy các dòng lệnh dưới đây tại thư mục bạn muốn triển khai ứng dụng
```
#Step 1 - Download a sample application
sam init
```
- Sau đó chọn các tuỳ chọn:
```
Which template source would you like to use?
	1 - AWS Quick Start Templates
	2 - Custom Template Location
Choice: 1

Choose an AWS Quick Start application template
	1 - Hello World Example
	2 - Multi-step workflow
	3 - Serverless API
	4 - Scheduled task
	5 - Standalone function
	6 - Data processing
	7 - Infrastructure event management
	8 - Machine Learning
Template: 1

Use the most popular runtime and package type? (Python and zip) [y/N]: y

Would you like to enable X-Ray tracing on the function(s) in your application?  [y/N]: n

Project name [sam-app]:  fcj-book-store
```
![IAMConsole](/images/1-preparation/1-preparation-9.png?featherlight=false&width=90pc)

Bạn đã tạo khởi tạo một project SAM mẫu. Bước tiếp theo chúng ta sẽ chỉnh sửa project đó theo kiến trúc ứng dụng của mình.
