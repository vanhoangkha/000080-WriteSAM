# Serverless - Deploying applications with SAM

> Writing serverless applications with AWS SAM.

A hands-on workshop from the [AWS First Cloud Journey](https://github.com/vanhoangkha/aws-first-cloud-journey) series, available in English and Vietnamese.

**Read the workshop online:** https://vanhoangkha.github.io/000080-WriteSAM/

## Overview

In the first two articles of this series, we know how to build a simple serverless web application using the AWS console. To build the serverless application faster, AWS provides users with the AWS Serverless Application Model (SAM) service, which is an open-source framework for building a serverless applications. It provides shorthand syntax to express functions, APIs, databases, and event source mappings. You define the application you want with just a few lines per resource and model it using YAML. During deployment, SAM transforms and expands the SAM syntax into AWS CloudFormation syntax. Then, CloudFormation provides the resources to the user.

In this workshop 3, we will rebuild the web application in workshop 2 with AWS SAM.

## Workshop contents

1. Preparation
2. Deploy front-end
3. Deploy Lambda function
   - Create DynamoDB table
   - Deploy Lambda function
4. Config API Gateway
   - Create GET API
   - Create POST API
   - Create DELETE API
5. Test APIs by Postman
6. Test APIs with front-end
7. Clean up

## Prerequisites

- An AWS account with permissions to create the resources used in the labs
- Basic familiarity with the AWS Management Console
- [Hugo](https://gohugo.io/installation/) (extended edition) if you want to run the site locally

## Run locally

```bash
git clone https://github.com/vanhoangkha/000080-WriteSAM.git
cd 000080-WriteSAM
hugo server
```

Then open http://localhost:1313 in your browser.

## Repository structure

| Path | Purpose |
| --- | --- |
| `content/` | Workshop pages (`*.md` English, `*.vi.md` Vietnamese) |
| `static/` | Images and other static assets |
| `layouts/` | Hugo layout overrides |
| `themes/` | Hugo theme |
| `config.toml` | Site configuration |

## Cost and cleanup

The labs create AWS resources that may incur charges. Complete the **Clean up** chapter when you finish to remove them.

## Contributing

Contributions are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).

To report a security issue, see [SECURITY.md](SECURITY.md).

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE).
