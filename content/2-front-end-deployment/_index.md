---
title : "Deploy front-end"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---
In this step, we will create an S3 bucket with Static web hosting enabled and publicly accessible based on SAM:

1. Open **template.yaml** file in **fcj-book-store** folder that we created in part 1.
- Delete unnecessary part:

![CreateS3Bucket](/images/2-front-end-deployment/2-front-end-deployment-1.png?featherlight=false&width=90pc)

2. Copy the following script into that file: 
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
The above script defines an S3 bucket is  **fcj-book-store** with **FcjBookStorePolicy** policy - allow public access

![CreateBucket](/images/2-front-end-deployment/2-front-end-deployment-2.png?featherlight=false&width=90pc)

3. Run the below command to build at the directory of the SAM project: **fcj-book-store**
```
sam build
```

![CreateBucket](/images/2-front-end-deployment/2-front-end-deployment-3.png?featherlight=false&width=90pc)

- Run the following command to check the validation of the SAM template
```
sam validate
```

![CreateBucket](/images/2-front-end-deployment/2-front-end-deployment-4.png?featherlight=false&width=90pc)

- Run the following command to deploy SAM
```
sam deploy --guided
```
- Enter stack name: `fcj-book-store`
- Enter the deployemnt region, such as: `ap-southeast-1`- should be the same as the default region
- Then enter other information as shown below

![CreateBucket](/images/2-front-end-deployment/2-front-end-deployment-5.png?featherlight=false&width=90pc)

- Wait a while to create the CloudFormation stack changeset
- Enter "y" when **Deploy this changeset?** display

![CreateBucket](/images/2-front-end-deployment/2-front-end-deployment-6.png?featherlight=false&width=90pc)

4. Open [Amazon S3 console](https://s3.console.aws.amazon.com/s3/buckets?region=ap-southeast-1&region=ap-southeast-1)

![CheckBucket](/images/2-front-end-deployment/2-front-end-deployment-7.png?featherlight=false&width=90pc)

5. Check if the bucket has been created or not.
- Click **fcj-book-store** bucket

![CheckBucket](/images/2-front-end-deployment/2-front-end-deployment-8.png?featherlight=false&width=90pc)

6. Click **Properties** tab

![CheckBucket](/images/2-front-end-deployment/2-front-end-deployment-9.png?featherlight=false&width=90pc)

7. Scroll down, check state of **Static website hosting**
- Record the endpoint of the website
![CheckBucket](/images/2-front-end-deployment/2-front-end-deployment-10.png?featherlight=false&width=90pc)

8. Click **Permissions** tab
- See the policy has been added

![CheckBucket](/images/2-front-end-deployment/2-front-end-deployment-11.png?featherlight=false&width=90pc)

9. Open [CloudFormation console](https://ap-southeast-1.console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks?filteringStatus=active&filteringText=&viewNested=true&hideStacks=false)

![CheckCloudFormation](/images/2-front-end-deployment/2-front-end-deployment-12.png?featherlight=false&width=90pc)

10. Two stacks have been created
- Click **fcj-book-store** stack

![CheckCloudFormation](/images/2-front-end-deployment/2-front-end-deployment-13.png?featherlight=false&width=90pc)

10. Click **Resource** tab, see the resources that CloudFormation has initialized

![CheckCloudFormation](/images/2-front-end-deployment/2-front-end-deployment-14.png?featherlight=false&width=90pc)

- Click to other stack:

![CheckCloudFormation](/images/2-front-end-deployment/2-front-end-deployment-15.png?featherlight=false&width=90pc)

11. Download **fcj-serverless-frontend** code to your device
- Open a terminal on your computer at the directory where you want to save the source code
- Copy the below command
```
git clone https://github.com/AWS-First-Cloud-Journey/FCJ-Serverless-Workshop.git
cd FCJ-Serverless-Workshop
yarn build
```
12. We have finished building the front-end. Next, execute the following command to upload the **build** folder to S3
```
aws s3 cp build s3://fcj-book-store --recursive
```
Result after uploading:

![SettingBucket](/images/2-front-end-deployment/2-front-end-deployment-16.png?featherlight=false&width=90pc)



