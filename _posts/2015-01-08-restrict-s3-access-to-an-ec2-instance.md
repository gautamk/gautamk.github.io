---
layout: post
title: Restrict s3 access to an ec2 instance
categories: [dev-log]
tags: [aws, security]
published: True
comments: True
---

I've been rambling since yesterday trying to find an elegant way to restrict access s3 access to an ec2 instance. I wish it was more obvious. 

So anyway the trick is to assign an elastic ip to your ec2 instance and apply the following bucket policy 

{% highlight json %}
{
  "Statement": [
    {
      "Action": [
        "s3:GetObject"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::<bucket_name>/path/*",
      "Condition": {
        "StringEquals": {
          "aws:SourceIp": "<elastic_ip>"
        }
      },
      "Principal": {
        "AWS": [
          "*"
        ]
      }
    }
  ]
}
{% endhighlight %}

so now we can do `wget 	https://s3.amazonaws.com/bucket-name/path/filename` and it will work.