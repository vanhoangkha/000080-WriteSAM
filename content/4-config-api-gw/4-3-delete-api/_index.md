---
title : "Create DELETE API"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 4.3. </b> "
---
1. Open **template.yaml** file in **fcj-book-store** folder
2. Add the following script at the end of the file that create the DELETE method
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

- Add the following script at the end of the **BookDelete** function
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

2. Run the following command to deploy SAM
```
sam build
sam deploy --guided
```

{{% notice note %}}
Enter "y" if asked "BookDelete may not have authorization defined, Is this okay? [y/N]: "
{{% /notice %}}

![CreateDeleteAPI](/images/4-config-api-gw/4-config-api-gw-17.png?featherlight=false&width=90pc)

3. Open **book_delete** function console
- Click **API Gateway**

![CreateDeleteAPI](/images/4-config-api-gw/4-config-api-gw-18.png?featherlight=false&width=90pc)

4. Show API Gateway being interacted with this function
- Click this API Gateway

![CreateDeleteAPI](/images/4-config-api-gw/4-config-api-gw-19.png?featherlight=false&width=90pc)

5. Display the resources and DELETE method

![CreateDeleteAPI](/images/4-config-api-gw/4-config-api-gw-20.png?featherlight=false&width=90pc)

6. Click **Stages** on the left menu
- Click **staging**
- Click **DELTE**
- Record the **InvokeURL** of DELETE method

![CreateDeleteAPI](/images/4-config-api-gw/4-config-api-gw-21.png?featherlight=false&width=90pc)

7. Add the following script at the end of the **template.yaml** file to API can support Binary Media Types files
```
      BinaryMediaTypes: 
        - multipart~1form-data
```
8. Run the following command to deploy SAM
```
sam build
sam deploy --guided
```

![CreateDeleteAPI](/images/4-config-api-gw/4-config-api-gw-22.png?featherlight=false&width=90pc)

9. Back to API console
- Select **Settings** on the left menu
- Scroll down, check if **multipart/form-data** has been added under **Binary Meida Types**

![CreateDeleteAPI](/images/4-config-api-gw/4-config-api-gw-23.png?featherlight=false&width=90pc)


