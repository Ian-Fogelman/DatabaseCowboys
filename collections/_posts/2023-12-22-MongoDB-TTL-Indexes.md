---
layout: post
title:  TTL Indexes in MongoDB
categories: [MongoDB, ]
thumbnail: "/assets/images/MongoDB-Thumb.svg"
description: Explore the MongoDB TTL index which allows your server to automatically purge old data. 
---

MongoDB is the leading enterprise NoSQL database in the industry.
Lets take a look at an interesting MongoDB feature, the Time to Live (TTL) Index.
This type of index allows you to specify a time, either seconds after a time stamp field or an exact clock date to expire (delete) a record.
Reasons to do this vary widly but a couple use cases include:

| Use Case      | Description |
| ----------- | ----------- |
| Data Retention      | A timestamp could include the last login date of a user. After 6 years of no activity the TTL index removes user records.        |
| Application Logic   | A record in the database represents discount codes for a retail store. After a certain date the discount is no longer valid and is deleted by the TTL index.        |  
The example in this post is a queueing system that utilizes MongoDB TTL indexes to expire documents older than 60 seconds.
The expectation is that the application is checking the queue collection, looking for records to process and deleting them one the processing has began. If any record exists for longer than 60 seconds, it is considered out of scope for the time sensitive requirements of the application and is deleted by the TTL index. This keeps the queue open for valid requests.
Use the following Python code to import libraries to work with this example:

``` python
import pymongo
import pandas as pd
from pymongo import MongoClient
from pprint import pprint
from datetime import datetime, timedelta
client = MongoClient('INPUT_YOUR_CONNECTION_STRING')
db=client.Q
```

The following functions create a queue, insert records into the queue, and check the queue for records:

``` python
def create_queue(queuename,expireAfter):
    collection_name = queuename
    collection = db[collection_name]
    document = {"createdAt": datetime.utcnow()}
    collection.insert_one(document)
    index_name = "ttl_index"
    collection.create_index([("createdAt")], expireAfterSeconds=expireAfter, name=index_name)
    print(f"Collection '{collection_name}' created with TTL index '{index_name}'.")

def insert_queue_record(queuename,document):
    collection_name = queuename
    collection = db[collection_name]
    collection.insert_one(document)

def check_queue(queuename):
    collection_name = queuename
    collection = db[collection_name]
    cursor = collection.find({})
    df = pd.DataFrame(list(cursor))
    if(len(df) == 0):
        print('No records in the queue.')
    return df
```

This code block creates a queue `Queue` with a TTL index of `60` seconds. Any document that has a `createdAt` timestamp greater than `60` is deleted by the TTL index:

``` python
create_queue('Queue',60)
```

This code block creates a record in the `Queue` collection:

```python
document = {"createdAt": datetime.utcnow(), 'Value': "abc"}
document = {
    "metadata": "Sales data",
    "filename": "Sales.txt",
    "createdAt": datetime.utcnow()
}
insert_queue_record('Queue',document)
```

This code block checks the `Queue` collection for active records and returns the data to a Pandas Dataframe. A Python based processing layer in an application could read these records and process request from the `df` object:

``` python
queuename = 'Queue'
df = check_queue(queuename)
df
```