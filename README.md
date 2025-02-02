# Connecting AWS resources with Slack using AWS Chatbot

This repository creates SNS topics to send alerts to and uses Chatbot to connect these with Slack channels. It does so separately for prod and sandbox. 

## Deploy
In the project root directory execute:

`aws cloudformation deploy --template-file chatbotForSlack.cf.yml --stack-name slack-chatbot-connection`

## Resources
* CloudFormation: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-chatbot-slackchannelconfiguration.html#cfn-chatbot-slackchannelconfiguration-iamrolearn
* Chatbot and Slack: https://docs.aws.amazon.com/chatbot/latest/adminguide/slack-setup.html

# OLD WAY (to be removed later): Send CloudWatch Alarms to Slack with AWS Lambda

Read the full [blog post on cloudonaut.io](https://cloudonaut.io/send-cloudwatch-alarms-to-slack-with-aws-lambda/).

Or use [marbot, a free chatbot ensuring you never miss an alert from Amazon Web Services including CloudWatch](https://marbot.io/).

## Slack setup

1. Start by setting up an incoming webhook integration in your Slack workspace: https://my.slack.com/services/new/incoming-webhook/
2. Select a channel or create a new one
3. Click on *Add Incoming WebHooks integration*
4. You are redirected to a new page where you can see your *Webhook URL*. Copy the value; you will need it soon.

## AWS setup

1. Clone or [download](https://github.com/widdix/cloudwatch-alarm-to-slack/zipball/master/) this respository
2. create a S3 bucket for SAM (replace `$UniqueSuffix` with e.g. your username): `aws --region us-east-1 s3 mb s3://cw-to-slack-$UniqueSuffix`
3. Install Node.js dependencies: `npm install`
4. package the Lambda function code (replace `$UniqueSuffix` with e.g. your username): `aws --region us-east-1 cloudformation package --s3-bucket cw-to-slack-$UniqueSuffix  --template-file template.yml --output-template-file template.sam.yml`
5. Deploy the CloudFormation stack (replace `$WebhookURL` with your URL from Slack): `aws --region us-east-1 cloudformation deploy --parameter-overrides "WebhookURL=$WebhookURL" --template-file template.sam.yml --stack-name cw-to-slack --capabilities CAPABILITY_IAM`
