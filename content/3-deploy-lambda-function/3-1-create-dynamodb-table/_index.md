---
title : "Create DynamoDB table"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---
1. Open **template.yaml** file in **fcj-book-store** folder
2. Add the following script at the end of the file to create a simple table in DynamoDB:
```
  SimpleTable:
    Type: AWS:Serverless::SimpleTable
    Properties:
      TableName: SimpleTable
      PrimaryKey:
        Name: id
        Type: String
```
- The script defines a **SimpleTable** table in DynamoDB with the Partition key is **id**

![CreateDynamoDBTable](/images/3-deploy-lambda-function/3-1-create-dynamodb-table-1.png?featherlight=false&width=90pc)

3. Run the following command to deploy SAM
```
sam build
sam deploy --guided
```

![CreateTable](/images/3-deploy-lambda-function/3-1-create-dynamodb-table-2.png?featherlight=false&width=90pc)

4. Open [DynamoDB console](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#dashboard)
5. Select **Tables** on the left menu. You can see **SimpleTable** table created

![CreateTable](/images/3-deploy-lambda-function/3-1-create-dynamodb-table-3.png?featherlight=false&width=90pc)

6. But our table needs more config. So delete the above script and replace it with the following:
```
  BooksTable:
    Type: AWS::DynamoDB::Table
    Properties:
      TableName: Books
      BillingMode: PAY_PER_REQUEST
      AttributeDefinitions:
        - AttributeName: id
          AttributeType: S
        - AttributeName: rv_id
          AttributeType: N
        - AttributeName: name
          AttributeType: S
      KeySchema:
        - AttributeName: id
          KeyType: HASH
        - AttributeName: rv_id
          KeyType: RANGE
      LocalSecondaryIndexes:
        - IndexName: name-index
          KeySchema:
            - AttributeName: id
              KeyType: HASH
            - AttributeName: name
              KeyType: RANGE
          Projection:
            ProjectionType: ALL
```
- The above script creates the **Books** table in DynamoDB with the partition key of id, the sort key of rv_id and a Local Scondary Index.

![CreateTable](/images/3-deploy-lambda-function/3-1-create-dynamodb-table-4.png?featherlight=false&width=90pc)

7. Run the following command to deploy SAM
```
sam build
sam deploy --guided
```

![CreateTable](/images/3-deploy-lambda-function/3-1-create-dynamodb-table-5.png?featherlight=false&width=90pc)

8. Back to DynamoDB console. The **Books** table have been created and **SimpleTable** table deleted

![CreateTable](/images/3-deploy-lambda-function/3-1-create-dynamodb-table-6.png?featherlight=false&width=90pc)

9. Select **Books** table. Check informations of this table

![CreateTable](/images/3-deploy-lambda-function/3-1-create-dynamodb-table-7.png?featherlight=false&width=90pc)

- Click **Indexes** tab

![CreateTable](/images/3-deploy-lambda-function/3-1-create-dynamodb-table-8.png?featherlight=false&width=90pc)


So you have created the **Books** table with the Local secondary index of **name-index**

10. To add data to the table, you can download the below file. Then, open file and replace all **AWS-REGION** with the region that create S3 bucket - **book-image-resize-store**, such as: `ap-southeast-1`

{{%attachments title="Data" pattern=".*\.(json)$"/%}}


11. Run the following command at the directory where you save the **dynamoDB.json** file
```
aws dynamodb batch-write-item --request-items file://dynamoDB.json
```

![TableData](/images/3-deploy-lambda-function/3-1-create-dynamodb-table-8-1.png?featherlight=false&width=90pc)