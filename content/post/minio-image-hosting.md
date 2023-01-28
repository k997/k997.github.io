---
title: "minio作为图床的 bucket 及 token 策略配置"
date: 2023-01-28T20:59:24+08:00
draft: false
tags: ["mino","image hosting"]
---

## bucket 策略配置

以下例子存储桶名称为`pic`，将 `pic` 设置为只读。

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": [
                    "*"
                ]
            },
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::pic/**"
            ]
        }
    ]
}
```

## access token 策略设置

access token 拥有读写权限。

```json
{
 "Version": "2012-10-17",
 "Statement": [
  {
   "Effect": "Allow",
   "Action": [
    "s3:GetBucketLocation",
    "s3:ListBucket"
   ],
   "Resource": [
    "arn:aws:s3:::pic"
   ]
  },
  {
   "Effect": "Allow",
   "Action": [
    "s3:GetObject",
    "s3:PutObject"
   ],
   "Resource": [
    "arn:aws:s3:::pic/*"
   ]
  }
 ]
}

```

