---
title: "Preparation"
date: 2025-11-30
weight: 2
chapter: false
pre: " <b> 5.2 </b> "
---

### IAM Permission

Add the following IAM permission policy to your user account to deploy and cleanup this workshop.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Resource": [
                "arn:aws:logs:ap-northeast-1:833390817075:log-group:/aws/codebuild/CodeBuild-VideoShare",
                "arn:aws:logs:ap-northeast-1:833390817075:log-group:/aws/codebuild/CodeBuild-VideoShare:*"
            ],
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ]
        },
        {
            "Effect": "Allow",
            "Resource": [
                "arn:aws:s3:::codepipeline-ap-northeast-1-*"
            ],
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:GetObjectVersion",
                "s3:GetBucketAcl",
                "s3:GetBucketLocation"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "codebuild:CreateReportGroup",
                "codebuild:CreateReport",
                "codebuild:UpdateReport",
                "codebuild:BatchPutTestCases",
                "codebuild:BatchPutCodeCoverages"
            ],
            "Resource": [
                "arn:aws:codebuild:ap-northeast-1:833390817075:report-group/CodeBuild-VideoShare-*"
            ]
        }
    ]
}
```
