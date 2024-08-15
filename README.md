# China CloudFront SSL Plugin

The China CloudFront SSL Plugin solution from Amazon Web Services in the China region helps you generate, update, and download free SSL/TLS certificates. It also supports integration with Amazon CloudFront and automates the process of updating associated SSL certificates. SSL utilizes data encryption, authentication, and message integrity verification mechanisms to ensure the security of data transmission over networks. This can help protect sensitive information on websites, such as personal identification and credit card details, guarding against theft by hackers.

## Features

- Almost Free*: Built using serverless architecture and open-source tools, it incurs charges based on the invocation of serverless services, with a default of every 80 days.
  - _*This solution adopts a serverless architecture, nearly zero cost with each certificate issuance, such as serverless resource execution costs, a small amount of Amazon S3 storage fees, and Amazon CloudWatch log storage fees._

- Out-of-the-Box: Deployment and certificate issuance for the solution can be completed in just 3 minutes. It supports certificate download, integration with Amazon CloudFront, and automatic updates.

- Fully Open Source: All code within this solution is provided in an open-source manner, allowing for customization based on your specific needs.

## Architecture Diagram

![Architecture Diagram.png](Architecture%20Diagram.png)

This solution automates the deployment of a series of serverless resources using an [Amazon CloudFormation](https://www.amazonaws.cn/cloudformation/) templates. These resources include Amazon Lambda, Amazon SNS topics, Amazon EventBridge rules, and Amazon API Gateway, etc., The goal is to facilitate the automatic and periodic generation of free SSL certificates through Let's Encrypt and the open-source tool Certbot. These certificates are then automatically uploaded to both the Amazon IAM SSL certificate storage and Amazon S3. Furthermore, the solution supports the automated renewal of IAM SSL certificates in Amazon CloudFront. Additionally, the solution provides an API interface and management interface based on the IAM SSL certificate storage.

- [Let’s Encrypt](https://letsencrypt.org/) is a free, open, and automated certificate authority (CA).
- [Certbot](https://certbot.eff.org/) is a free open-source software tool that automates the process of obtaining, deploying, and renewing SSL certificates issued by Let's Encrypt.
- [Amazon Lambda](https://www.amazonaws.cn/en/lambda/?nc1=h_ls) is used to run the Certbot certificate issuance and renewal process, manage the API interface, and handle the IAM SSL certificate management API.
- [Amazon SNS](https://www.amazonaws.cn/en/sns/) is used to send email notifications about certificate issuance status.
- [Amazon EventBridge](https://www.amazonaws.cn/en/eventbridge/) is used for event-driven architecture. It automatically runs the Certbot certificate issuance process upon successful deployment or update of the solution stack, enabling certificate issuance. Additionally, it generates free SSL certificates at regular intervals (default every 80 days) for certificate renewal.
- [Amazon API Gateway](https://www.amazonaws.cn/en/api-gateway) is used to integrate and manage SSL certificate operations, providing a callable interface.
- [Amazon S3](https://www.amazonaws.cn/en/s3/) buckets are used to store backup SSL certificates, which can be downloaded to local systems via the Amazon S3 console. A temporary bucket is also used to host the HTTP ACME challenge token, that should be manually exposed through Cloudfront.
- [IAM SSL certificate storage](https://docs.amazonaws.cn/en_us/IAM/latest/UserGuide/id_credentials_server-certs.html) is used to store SSL certificates associated with [Amazon CloudFront](https://www.amazonaws.cn/en/cloudfront/). In the Amazon Web Service China region, if you intend to use Amazon CloudFront to provide content over HTTPS, you are required to utilize the IAM SSL certificate storage. For specific details, please refer to the [Amazon CloudFront feature availability and implementation differences](https://docs.amazonaws.cn/en_us/aws/latest/userguide/cloudfront.html#feature-diff). This solution automatically adds the issued SSL certificates to the IAM SSL certificate storage. To achieve automatic SSL certificate updates in Amazon CloudFront, you will need to manually select the SSL certificate you wish to associate within the Amazon CloudFront distribution settings. Once associated, the SSL certificate will be automatically updated within Amazon CloudFront.

## Table of Contents
| Directory                       | Description                                                                             |
|---------------------------------|-----------------------------------------------------------------------------------------|
| [cdk](./cdk/)                   | Code used to generate CloudFormation                                                    |
| [lambda](./lambda/)             | Lambda code for Let's Encrypt/Certbot certificate issuance & IAM Certificate Management |

## Build Guidance

1. Depends on the requirements, please modify and build [Lambda code](./lambda/) to container and push to Amazon ECR at first.
2. Modify and export CloudFormation template based on [CDK code](./cdk/).

## Documentations

Solution Deployment Doc: ([English](https://www.amazonaws.cn/en/getting-started/tutorials/create-ssl-with-cloudfront/?nc1=h_ls) | [简体中文](https://www.amazonaws.cn/getting-started/tutorials/create-ssl-with-cloudfront/?nc1=h_ls))

Blog (in Chinese): https://aws.amazon.com/cn/blogs/china/divert-website-access-traffic-from-ec2-to-amazon-cloudfront/

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under MIT-0 License. See the [LICENSE](LICENSE) file.