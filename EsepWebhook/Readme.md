# esep-webhooks

ESEP Assignment 8: Event-Driven Architecture.

An AWS Lambda function (C#, .NET 10) that GitHub calls when an issue changes on this repo. It reads the issue link from the webhook payload and posts `Issue Created: <link>` to the `#eventing-webhook` Slack channel.

## Flow

1. An issue is opened on this repo.
2. GitHub sends an `issues` webhook (HTTP POST, `application/json`) to an API Gateway REST API (`/default` stage, POST on `/`).
3. API Gateway invokes the `EsepWebhook` Lambda in `us-east-2` with the raw JSON body (Lambda proxy integration off).
4. The function reads `issue.html_url` and posts it to the Slack incoming webhook.

## Configuration

The Slack webhook URL is read from the `SLACK_URL` environment variable on the Lambda function. It is not stored anywhere in this repository.

## Deploy

```
cd EsepWebhook
dotnet lambda deploy-function EsepWebhook
```