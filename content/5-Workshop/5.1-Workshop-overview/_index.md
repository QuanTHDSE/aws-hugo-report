---
title: "Introduction"
date: 2025-11-30
weight: 1
chapter: false
pre: " <b> 5.1 </b> "
---

### Architecture

![Diagram](workshop.png)

### Amazon ECR (Elastic Container Registry)

Amazon ECR is a fully managed Docker image registry that allows you to securely store, manage, and version your container images. It integrates seamlessly with ECS, CodeBuild, and CodePipeline.

### AWS CodePipeline

AWS CodePipeline is a fully managed CI/CD service that automates the software release process. Whenever changes are pushed to the source repository, CodePipeline triggers stages such as Source → Build → Deploy without manual intervention.

### AWS CodeBuild

AWS CodeBuild is a fully managed build service that compiles source code, runs tests, builds Docker images, tags them, and pushes them to ECR. All build steps are defined in a simple buildspec.yml file.

### Workshop Overview

In this workshop, you will learn how to build an automated CI/CD pipeline for containerized applications using AWS CodePipeline, CodeBuild, and Amazon ECR. You will create a complete workflow that retrieves source code, builds a Docker image, and pushes it securely to an ECR repository. Throughout the workshop, you will configure essential AWS services, work with buildspec files, and understand how each component contributes to a smooth and automated deployment process.
