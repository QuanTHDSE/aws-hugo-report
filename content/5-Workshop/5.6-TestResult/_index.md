---
title: "Test Result"
date: 2025-11-30
weight: 6
chapter: false
pre: " <b> 5.6 </b> "
---

Once the setup is complete, the automated CI/CD workflow operates as follows:

1. Code Changes Pushed
   A developer commits and pushes new code to the source repository (GitHub or CodeCommit).

2. Pipeline Triggered
   CodePipeline automatically detects the change and starts the pipeline.

3. Source Stage Executes
   CodePipeline fetches the latest version of the source code and passes it to the next stage.
   ![image1](/image1.png)
4. Build Stage (CodeBuild)
   CodeBuild pulls the source code, reads the buildspec.yml, logs in to ECR, builds the Docker image, tags it, and pushes it to the correct ECR repository.
   (you can see process in pipeline)
   ![image2](/image2.png)
5. Image Published to ECR
   The newly built Docker image is stored and versioned in Amazon ECR.
