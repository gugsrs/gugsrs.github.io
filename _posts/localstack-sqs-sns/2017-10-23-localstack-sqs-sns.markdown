---
title: "Using localstack to test SQS and SNS locally"
layout: post
date: 2017-10-22 17:27
image: /assets/images/markdown.jpg
headerImage: false
tag: 
- localstack
- aws
- sqs
- sns
- python
category: blog
author: gugsrs
description: Using localstack to test SQS and SNS locally
---

## Summary:

Sometimes it's hard to test locally a microservice architecture with messages flying around.
I've found some alternatives to that, [GoAWS][goaws] and [Localstack][localstack], and in this post I'll cover how I manage to run localstack to mock my infrastructure.

## Setup:
Before going into action, make sure you have installed python (I'm currently using Python 3.6.1), pip and aws-cli.

To set up localstack it's pretty easy:
```bash
pip install localstack
```

Before we start make sure you have set fake aws credentials to not mess with production enviroment.

~/.aws/credentials
```bash
AWS_ACCESS_KEY_ID=foo
AWS_SECRET_ACCESS_KEY=bar
```

## Starting Localstack:

Since localstack mocks all kind of services from aws, it's possible to choose which one we would like to run.

```bash
$ export SERVICES=sqs,sns
```

To start localstack just :

```bash
$ localstack start
```

It's possible to use with docker too:

```bash
$ localstack start --docker
```

## Using aws-cli:

### Create Topic:

```bash
$ aws --endpoint-url=http://localhost:4575 sns create-topic --name my_topic
```

### List Topic:

```bash
$ aws --endpoint-url=http://localhost:4575 sns list-topics
```

### Create Queue:

```bash
$ aws --endpoint-url=http://localhost:4576 sqs create-queue --queue-name my_queue
```

### List Queue:

```bash
$ aws --endpoint-url=http://localhost:4576 sqs list-queues
```

### Subscribe Queue to Topic:

```bash
$ aws --endpoint-url=http://localhost:4575 sns subscribe --topic-arn arn:aws:sns:us-east-1:123456789012:my_topic --protocol sqs --notification-endpoint arn:aws:sns:us-east-1:123456789012:my_queue
```

### List subscriptions:

```bash
$ aws --endpoint-url=http://localhost:4575 sns list-subscriptions
```

After creating our infra we need to start sending messages so that our service can receive and use them.
For that we are going to publish the message to the topic as followed:

```bash
$ aws --endpoint-url=http://localhost:4575 sns publish --topic-arn arn:aws:sns:us-east-1:123456789012:my_topic --message "Hi"
```

If you want it's possible to send the content of a file too:
```bash
$ aws --endpoint-url=http://localhost:4575 sns publish --topic-arn arn:aws:sns:us-east-1:123456789012:my_topic --message file://file.json
```

## Creating Topics and Queues automatically:

Since it's a lot of hardwork to create all of those queues and topics by hand, and Localstack does not have an option to start with a config file(GoAws let you use a .yaml to set things up), I've managed to create a [Python script][sqs-sns-from-yaml] to create them from a .yaml file. You can see the example from the config file [here][yaml].


And then just:

```bash
$ ./create_sqs_sns.py my-queues-and-topics.yaml
```

[goaws]: https://github.com/p4tin/goaws
[localstack]: https://github.com/localstack/localstack
[sqs-sns-to-yaml]: https://gist.github.com/wiliamsouza/c687ac7d8c86dc603a32e143cacc71b0
[sqs-sns-from-yaml]: https://gist.github.com/gugsrs/20d90e1f21792a67eb6d27c445aa7a1f
[yaml]: https://github.com/p4tin/goaws/blob/master/app/conf/goaws.yaml
