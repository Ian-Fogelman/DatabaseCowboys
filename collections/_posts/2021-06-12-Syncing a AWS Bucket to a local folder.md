---
layout: post
title:  Sync an S3 bucket to Local
date:   2021-06-13
description: This tutorial teaches you how to use the AWS CLI to sync an entire S3 bucket to your local work station.
categories: [AWS,S3]
datatable: true
author: Ian Fogelman # Add name author (optional)
thumbnail: "/assets/images/S3.png"
---

Recently I knew for a fact that I would be loosing one of my AWS accounts and I had a specific bucket which held some files that were near and dear to my heart. I offloaded the files into S3 knowing that they would be safe and sound and would not cluster up any local storage on my PC. All said these particular files were large video files which came to a total of about 150 GB. 

### Listing All Available Buckets in the AWS CLI

As a refresher, you can view all the S3 buckets you have by running the following command :

{% highlight cmd%}

aws s3api list-buckets

{% endhighlight %}

### Viewing the Total Size and Number of Objects in a Bucket

To view the total size and number of objects in a bucket, navigate to the ``Metrics`` tab of your specific 
bucket and you will be able to view this data:

![S3 Metrics](\assets\images\S3_Metrics.PNG)

So I wanted to figure out how can I get all of these files which have several different folders inside a single S3 bucket to my local machine in the fastest way possible? 

The answer is with the [s3 sync](https://docs.aws.amazon.com/cli/latest/reference/s3/sync.html) command. This command allows you to take the remote files in a s3 bucket and write them to your local file system.

### Example

For this example I created a folder on my ``C/:`` called ``Test`` which will be the location I pull my files into.

Open your command line and change the working directory and 
execute the ``aws s3 sync`` command with the target s3 bucket you want to pull files from. The ``.`` in this context 
specifies to pull all remote files to the current directory:


{% highlight cmd%}
cd C:\Test
aws s3 sync s3://MyTargetBucket .
{% endhighlight %}


When you run this command you may be flooded with messages into your command line interface.
These messages give you an indication of the process of the sync and exactly which files in the batch are being moved over.

<div class="alert alert-block alert-info">
<b>Tip:</b> Use blue boxes (alert-info) for tips and notes. 
If it’s a note, you don’t have to include the word “Note”.
</div>

![S3 Results](\assets\images\AWS_S3_Sync_EX.PNG)

### Conclusion

And that's it, when the command is done, you will have all the files in that S3 bucket on your local machine!

In this blog post we learned how to: 

- List the buckets available with the ``aws s3api list-buckets`` command.
- View the size of objects stored in a bucket with the AWS console.
- Sync an entire S3 bucket to your local file system with the ``aws s3 sync`` command.