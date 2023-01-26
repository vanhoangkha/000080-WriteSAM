---
title : "Test APIs by Postman"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 5. </b> "
---
In this step, we will test operation of the APIs using [Postman](https://www.postman.com/downloads/) tool.
#### Test the listing API
1. Click **+** to add a new tab
- Select **GET** method
- Enter URL of the listing API that recorded from the previous step
- Click **Send**

![TestListAPI](/images/5-test-api-by-postman/5-test-api-by-postman-1.png?featherlight=false&width=90pc)

2. The returned result is the entire data of the **Books** table that has been processed

![TestListAPI](/images/5-test-api-by-postman/5-test-api-by-postman-2.png?featherlight=false&width=90pc)

#### Test the writing API
1. Similarly create a new tab
- Select **POST** method
- Enter URL of the writing API that recorded from the previous step
- In **Body** pattern, select **raw**
- Copy the below text block:
```
{
    "id": "5",
    "rv_id": 0,
    "name": "Amazon Web Services in Action 2nd Edition",
    "author": "Andreas Wittig",
    "price": "59.99",
    "category": "IT",
    "description": "Amazon Web Services in Action, Second Edition is a comprehensive introduction to computing, storing, and networking in the AWS cloud. You'll find clear, relevant coverage of all the essential AWS services you to know, emphasizing best practices for security, high availability and scalability.",
    "image": "https://book-image-resize-store.s3.ap-southeast-2.amazonaws.com/aws.jpg"
}
```

![TestCreateAPI](/images/5-test-api-by-postman/5-test-api-by-postman-3.png?featherlight=false&width=90pc)

2. Switch to **Headers**
- Add KEY is **Content-Type**, VALUE is **application/json**
- Click **Send**

![TestCreateAPI](/images/5-test-api-by-postman/5-test-api-by-postman-4.png?featherlight=false&width=90pc)

3. Wait a moment, see the results returned

![TestCreateAPI](/images/5-test-api-by-postman/5-test-api-by-postman-5.png?featherlight=false&width=90pc)

4. Open **Books** table in DynamoDB console to check data
- Before call the write API

![TestCreateAPI](/images/5-test-api-by-postman/5-test-api-by-postman-6.png?featherlight=false&width=90pc)

- After call the write API

![TestCreateAPI](/images/5-test-api-by-postman/5-test-api-by-postman-7.png?featherlight=false&width=90pc)

#### Test the deleting API
Since the delete Lambda function on execution deletes images uploaded by the user, we manually upload the images to the S3 bucket so the API can run properly.

1. Open [Amazon S3 console](https://s3.console.aws.amazon.com/s3/buckets?region=ap-southeast-2&region=ap-southeast-2)

2. Click **book-image-store** bucket

![TestListAPI](/images/5-test-api-by-postman/5-test-api-by-postman-8.png?featherlight=false&width=90pc)

3. Click **Upload**

![TestListAPI](/images/5-test-api-by-postman/5-test-api-by-postman-9.png?featherlight=false&width=90pc)

4. Click **Add files**

![TestListAPI](/images/5-test-api-by-postman/5-test-api-by-postman-10.png?featherlight=false&width=90pc)

5. Tải ảnh sau về máy của bạn và chọn nó để tải lên bucket

{{%attachments title="Image" pattern=".*\.(jpg)$"/%}}
6. Click **Upload**
![TestListAPI](/images/5-test-api-by-postman/5-test-api-by-postman-11.png?featherlight=false&width=90pc)

7. After the upload is done, switch to **book-image-resize-store** bucket to check. This is execution result of reszie_image Lambda funtion

![TestListAPI](/images/5-test-api-by-postman/5-test-api-by-postman-12.png?featherlight=false&width=90pc)

8. Back to Postman, add a new tab to call the delete API
- Select **GET** method
- Enter URL of the deleting API that recorded from the previous step, replace **/{id}** with **/5**
- Click **Send**

![TestListAPI](/images/5-test-api-by-postman/5-test-api-by-postman-13.png?featherlight=false&width=90pc)

9. Check the returned result:

![TestListAPI](/images/5-test-api-by-postman/5-test-api-by-postman-14.png?featherlight=false&width=90pc)

10. Open **Books** table in DynamoDB console to check data

![TestListAPI](/images/5-test-api-by-postman/5-test-api-by-postman-6.png?featherlight=false&width=90pc)

11. Open **book-image-resize-store** bucket to check object. The **aws.jpg** is deleted

![TestListAPI](/images/5-test-api-by-postman/5-test-api-by-postman-15.png?featherlight=false&width=90pc)