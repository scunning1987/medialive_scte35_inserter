# MediaLive SCTE35 Inserter
This CloudFormation enables you to insert SCTE35's per a repeating time based method in Amazon EventBridge (cron). This is useful for debugging or testing ad insertion workflows

## Overview
The solution creates the following:
- IAM Role for Lambda to assume
- IAM Policy to attach to Lambda Role
- EventBridge event, including permissions to invoke Lambda function

## Deployment Instructions
The solution can be deployed using [this CloudFormation Template](https://raw.githubusercontent.com/scunning1987/medialive_scte35_inserter/main/medialive_scte35_inserter.yaml)

There are 3 parameters in the stack creation:
- LambdaInvocationInterval: This is a value (in minutes) of how often to invoke the Lambda to create the SCTE35 schedule actions. Use values between 5-50 minutes.
- MediaLiveRegionsToCheck: The Lambda function is designed to scan all MediaLive channels in a region. Any channels that carry the specific Tags will be targeted by this Lambda for ad insertion.
- Scte35InsertionOffset: This is a value in seconds that will be used when creating the first SCTE35 schedule action. MediaLive doesn't support scheduled actions within 15 seconds of current time, so it's recommended to use between 20-30 seconds.

After the stack has created, navigate to the MediaLive console. For every channel that you want to apply the automated SCTE35 insertion to, you must add 2 Tags:
- scte_inject_interval: this is the value (in seconds) of how often you want to insert ads into the stream. **Note: The interval value must be divisible by the 'LambdaInvocationInterval' with no remainder - ie. if the LambdaInvocationInterval is 10 minutes (600 seconds), the scte_inject_interval could be 120, 200, 300, 600**
- scte_inject_duration: this is the value (in seconds) of the duration of the ad breaks you want to insert per ad break interval

![](medialive_tags.png?width=25pc&classes=border,shadow)