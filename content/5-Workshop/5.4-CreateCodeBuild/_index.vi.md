---
title: "Tạo Code Build"
date: 2025-11-30
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---

1. Đầu tiên, bạn cần thêm file `buildspec.yml` vào mã nguồn của bạn với nội dung như sau:

```
version: 0.2

env:
  variables:
    AWS_DEFAULT_REGION: ap-northeast-1          # thay đổi region của bạn
    ACCOUNT_ID: ############                   # thay đổi AWS account ID của bạn
    REPO_NAME: videoshare/repository   # thay đổi tên repository của bạn

phases:
  pre_build:
    commands:
      - echo Logging in to Amazon ECR...
      - aws --version
      - aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin $ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com
      - REPOSITORY_URI=$ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$REPO_NAME
      - IMAGE_TAG=$(echo $CODEBUILD_RESOLVED_SOURCE_VERSION | cut -c 1-7)
      - echo Repository URI $REPOSITORY_URI
      - echo Image Tag $IMAGE_TAG

  build:
    commands:
      - echo Building the Docker image for .NET API...
      - docker build -t $REPO_NAME:$IMAGE_TAG -f backend-video-sharing-platform/Dockerfile .
      - docker tag $REPO_NAME:$IMAGE_TAG $REPOSITORY_URI:latest

  post_build:
    commands:
      - echo Pushing Docker image to Amazon ECR...
      - docker push $REPOSITORY_URI:latest
      - echo Writing imagedefinitions.json for CodePipeline/App Runner...
      - printf '[{"name":"%s","imageUri":"%s"}]' "$REPO_NAME" "$REPOSITORY_URI:$IMAGE_TAG" > imagedefinitions.json
      - echo Build completed successfully!

artifacts:
  files:
    - imagedefinitions.json
```

2. Commit lên github

3. Mở [Amazon CodeBuild](https://ap-northeast-1.console.aws.amazon.com/codesuite/codebuild/start?region=ap-northeast-1)
4. Trong ngăn điều hướng, chọn **Build projects**, sau đó nhấp **Create project**:
   ![image1](/image1.png)
5. Trong giao diện Create project:

- Nhập tên project
- Thêm Source 1
- Trong trường Source provider chọn GitHub (Nếu bạn chưa có Credential thì cần kết nối nó)
- Chọn Repository
  ![image2](/image2.png)

6. Trong Buildspec chọn **Use a buildspec file**
   ![image3](/image3.png)
7. Nhấp **Create build project**
