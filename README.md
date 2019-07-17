# stock-price-sms
Use AWS SNS(and optionally Lambda) to schedule receiving a text message with stock price of a desired company

### Warning:
AWS Charges applicable

### Prerequisites:

 - Python 2.7
 - AWS credentials must be setup
 - "company_code" refers to the Stock Exchange code: GOOG, AAPL, AMZN etc
 - "phone_number" in International format
 - "sender_name" as desired
 - "exchange_api_key" needs to be obtained from https://openexchangerates.org/account/app-ids (Sign Up required, 1000 calls free every month)
 - If running on AWS Lambda, the "requests" module can be imported from botocode library, and the Environment variables can be used from Lambda(paste the contents of stock-price-lambda.md file into a new and completely blank Python2.7 Lambda function), the IAM role should have "AmazonSNSFullAccess" policy attached
 
 
### Instructions:

For testing:

```
python stock-price.py
```

For scheduling in cron (8 am everyday from an EC2 instance):
```
0 8 * * * python /home/ec2-user/stock-price-sms.py
```

### Further development:

- Can be improved by adding more company codes since the Yahoo Finance URL accepts comma separated codes
- Can be used with CloudFormation template that creates the Lamdba function, the required roles and the CloudWatch cron schedules
