---
title: "Using localstack to test SQS and SNS locally"
layout: post
date: 2017-11-23 12:27
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

---

Before going into action, make sure you have installed ```python```(I'm currently using Python 3.6.1), ```pip```, ```aws-cli```.

To set up localstack it's pretty easy, just ```pip install localstack ```.  
To start AWS Services just ```localstack start``` and you'll see SNS running on port 4575 and SQS running on port 4576.

Now it's time to create our topics and queues, make sure you have set fake aws credentials to not mess with production enviroment.  

{% highlight bash %}
  AWS_ACCESS_KEY_ID=foo  
  AWS_SECRET_ACCESS_KEY=bar
{% endhighlight %}

And that's it! All your sqs/sns will be ready to be tested.

[goaws]: http://goaws
[localstack]: https://github.com/localstack/localstack
[sqs-sns-to-yaml]: https://gist.github.com/wiliamsouza/c687ac7d8c86dc603a32e143cacc71b0
[sqs-sns-from-yaml]: https://gist.github.com/gugsrs/20d90e1f21792a67eb6d27c445aa7a1f
[wiliam]: https://github.com/wiliamsouza/
