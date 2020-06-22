## REST, GraphQL & AWS AppSync

REST, which is an architectural concept for network-based software. GraphQL instead, consists of a query language, a specification and a set of tools which operate over a single endpoint using HTTP.

GraphQL differentiates itself in how you can specify the amount of data retrieved. With REST APIs, you are not able to fetch specific information as you always return the full dataset. With GraphQL, you can tailor the request to only retrieve what you need, and nothing more.

However, with the challenges around managing your own GraphQL endpoint, AWS AppSync simplifies the setup as AppSync is a managed service.
AppSync automatically scales with your workload requirements, and integrates seamlessly with various Amazon services.

What this page will illustrate, is the challenges and methods in which i have used, during development of my own projects.

## Contents

## Identity & Authentication
In the early release days of AppSync, AppSync did not support multiple-authorization types. This meant that you had to choose a single authorization type such as:

1. API Key
2. Cognito
3. IAM

This limitation was a pain point as i was not able to utilise AppSync subscriptions efficiently, when performing a mutation from a backend service, and having a client UI update in real-time using subscriptions. 


## Pipelines

AppSync supports [pipeline resolvers ](https://docs.aws.amazon.com/appsync/latest/devguide/pipeline-resolvers.html) in order to handle the challenge with executing multiple operations for a single GraphQL field.

For my example, i needed to perform two operations, in order to push data to [Amazon Cognito](https://aws.amazon.com/cognito/) and [Amazon Aurora](https://aws.amazon.com/rds/aurora/). However, my Amazon Aurora instance was within a private VPC subnet, which meant that i had to utilise a [NAT Gateway](https://aws.amazon.com/premiumsupport/knowledge-center/internet-access-lambda-function/) in order for lambda functions to interact with both Cognito and RDS Aurora. I wanted to keep costs low, and could not create a VPC service endpoint for cognito, so i elected to create two lambdas, one to perform commands with Cognito outside of the VPC, and the second to perform actions against RDS Aurora, within the VPC.

<Insert image here on using NAT Gateway vs Pipelines>