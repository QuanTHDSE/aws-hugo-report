---
title: "Proposal "
date: 2025-11-30
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Video Sharing Platform

## 1.Executive Summary

This proposal outlines the development of a scalable Video Sharing Platform leveraging AWS cloud services. The platform will enable users to upload, stream, and share video content with features including user authentication, content management, and real-time video streaming.

Key objectives:

- Build a secure, scalable video sharing platform
- Implement user authentication and authorization
- Provide high-quality video streaming capabilities
- Ensure cost-effective infrastructure management
- Deliver seamless user experience across devices

The solution utilizes AWS services including Amplify for frontend hosting, Cognito for authentication, S3 for storage, CloudFront for content delivery, and Interactive Video Service for streaming capabilities.

## 2.Problem Statement

### What's the Problem?

Current video sharing solutions face several challenges:

- High infrastructure costs for video storage and streaming
- Complex setup and maintenance requirements
- Limited scalability during peak usage
- Security vulnerabilities in user authentication
- Poor video quality and buffering issues

### The Solution

Our AWS-based video sharing platform addresses these challenges by:

- Leveraging AWS's cost-effective, pay-as-you-use pricing model
- Utilizing managed services to reduce operational overhead
- Implementing auto-scaling capabilities for handling traffic spikes
- Providing enterprise-grade security through AWS Cognito
- Delivering high-quality video streaming via Amazon IVS and CloudFront

### Benefits and Return on Investment

**Cost Savings:**

- 40-60% reduction in infrastructure costs compared to traditional hosting
- No upfront hardware investments required
- Pay-per-use model optimizes operational expenses

**Performance Improvements:**

- 99.9% uptime availability
- Global content delivery with low latency
- Auto-scaling handles 10x traffic increases seamlessly

**Business Value:**

- Faster time-to-market (3-6 months vs 12+ months)
- Enhanced user experience drives higher engagement
- Scalable architecture supports business growth

## 3.Solution Architecture

![Architecture Diagram](/video-sharing-platform.png)

### AWS Services Used

**Amplify:** Frontend hosting and deployment platform for React/Vue.js applications with CI/CD integration.

**Cognito:** User authentication and authorization service providing secure sign-up, sign-in, and access control.

**App Runner:** Containerized backend API hosting with automatic scaling and load balancing.

**DynamoDB:** NoSQL database for storing user profiles, video metadata, and application data.

**S3:** Object storage for video files, thumbnails, and static assets with versioning and lifecycle policies.

**CloudFront:** Global CDN for fast content delivery and video streaming with edge caching.

**Amazon IVS (Interactive Video Service):** Real-time video streaming service for live broadcasts and on-demand content with low latency.

**AWS Elemental MediaConvert:** Used to turn original video into many formats, many resolutions, many codecs, for streaming or downloading.

**AWS Lambda:** Serverless compute service for event-driven processing, video transcoding triggers, and backend automation tasks.

**Code Pipeline:** CI/CD pipeline for automated testing, building, and deployment.

**Code Build:** Build service for compiling source code, running tests, and creating deployment packages.

**Elastic Container Registry:** Docker container registry for storing and managing application images.

### Component Design

**Frontend Layer:**

- React-based web application hosted on Amplify
- Responsive design supporting mobile and desktop
- Real-time video player with adaptive bitrate streaming

**API Layer:**

- RESTful APIs built with .NET
- Containerized and deployed on App Runner
- AWS Lambda for serverless API endpoints
- JWT-based authentication integration

**Data Layer:**

- DynamoDB tables for user data and video metadata
- S3 buckets for video storage with intelligent tiering

**Security Layer:**

- Cognito user pools for authentication
- IAM roles and policies for access control

**Video Processing Layer:**

- AWS Lambda functions triggered by S3 events
- AWS Elemental MediaConvert for transcoding videos
- Automated conversion to multiple formats and resolutions
- HLS and DASH output for adaptive streaming

**Streaming Architecture:**

- Amazon IVS for live streaming capabilities
- CloudFront for global video distribution
- Adaptive bitrate streaming for optimal quality

### Use Cases

**Live Streaming Events:**

- Real-time broadcasting of conferences, webinars, and corporate events
- Multi-bitrate streaming for optimal viewer experience

**Video On Demand (VOD):**

- Upload and share educational content, tutorials, and training materials
- Secure content access with user permissions

**Social Video Sharing:**

- User-generated content sharing
- Community features with comments and ratings

## 4.Technical Implementation

### Phase 1: Infrastructure Setup

**AWS Account Configuration:**

- Configure IAM roles and policies for least privilege access

**Core Services Deployment:**

- Deploy DynamoDB tables with proper indexing
- Configure S3 buckets with encryption and lifecycle policies
- Set up Cognito user pools and identity pools

### Phase 2: Backend Development

**API Development:**

- Build RESTful APIs using Node.js/Express framework
- Implement JWT authentication with Cognito integration
- Create video upload/processing endpoints
- Develop Lambda functions for event-driven tasks
- Develop user management and content APIs

**Database Schema:**

- Users table: userId, avatarUrl, birthDate, channelId, createdAt, email, gender, name, phoneNumber
- Videos table: videoId, channelId, commentCount, createdAt, createdFromStreamId, description, duration, key, likeCount, playbackUrl, status, thumbnailUrl, title, type, updatedAt, userId, viewCount
- VideoLikes table: userId, videoId, createdAt
- Subscriptions table: userId, channelId, createdAt
- StreamSessions table: streamId, channelId, createdAt, description, endedAt, isLive, recordingUrl, startedAt, status, thumbnailUrl, title, updatedAt, userId, viewerCount
- Notifications table: recipientUserId, createdAt, actorAvatarUrl, actorName, actorUserId
- Comments table: videoId, commentId, content, createdAt, isDeleted, isEdited, likeCount, parentCommentId, replyCount, updatedAt, userAvatarUrl, userId, userName
- CommentLikes table: commentId, userId, createdAt
- Channels table: channelId, avatarUrl, channelArn, createdAt, currentStreamId, description, ingestEndpoint, isLive, name, playbackUrl, streamKeyArn, subscriberCount, userId, videoCount

**Containerization:**

- Create Docker containers for API services
- Push images to Elastic Container Registry
- Configure App Runner for automatic deployment

### Phase 3: Frontend Development

**React Application:**

- Implement responsive UI components
- Integrate AWS Amplify SDK for authentication
- Build video upload interface with progress tracking
- Create video player with adaptive streaming

**Key Features:**

- User registration/login with email verification
- Video upload with drag-and-drop functionality
- Real-time video streaming with quality selection
- User dashboard for content management

### Phase 4: Streaming Integration

**AWS Lambda & MediaConvert Setup:**

- Create Lambda functions for S3 event handling
- Implement video processing workflow automation
- Configure MediaConvert job templates for transcoding
- Set up output presets (1080p, 720p, 480p)
- Generate HLS/DASH manifests for adaptive streaming
- Update DynamoDB with processing status

**Amazon IVS Setup:**

- Configure streaming channels and playback URLs
- Implement adaptive bitrate streaming
- Set up recording and archival workflows

**CloudFront Configuration:**

- Create distributions for video content delivery
- Configure edge locations for global reach
- Implement caching strategies for optimal performance

### Phase 5: CI/CD Pipeline

**Automated Deployment:**

- Configure CodePipeline for source-to-production workflow
- Set up CodeBuild for automated testing and building
- Implement blue-green deployment strategy

**Testing Strategy:**

- Unit tests for API endpoints
- Integration tests for AWS service interactions
- Load testing for performance validation

## 5.Timeline & Milestones

### Project Duration: 8 Weeks (2 Months)

**Week 1: Setup & Planning**

- AWS account setup and IAM configuration
- Project requirements finalization
- Team roles assignment
- Basic infrastructure deployment (S3, DynamoDB, Cognito)

**Week 2-3: Backend Development**

- RESTful APIs with .NET
- JWT authentication with Cognito
- Video upload endpoints
- Database schemas implementation
- App Runner deployment

**Week 4-5: Frontend Development**

- React application with responsive design
- User authentication flows
- Video upload interface
- Basic video player
- Amplify deployment

**Week 6: Integration & Streaming**

- Frontend-backend integration
- Lambda functions for video processing automation
- MediaConvert setup for video transcoding
- CloudFront setup for video delivery
- Basic streaming functionality
- Testing and bug fixes

**Week 7-8: Final Deployment**

- Production deployment
- User acceptance testing
- Documentation completion
- Project presentation preparation

### Key Milestones

**Milestone 1 (Week 1):** Infrastructure Ready

- AWS services configured
- Development environment accessible

**Milestone 2 (Week 3):** Backend Complete

- APIs functional
- Authentication working

**Milestone 3 (Week 5):** Frontend Complete

- UI fully developed
- Basic video upload/playback working

**Milestone 4 (Week 8):** Production Launch

- System deployed and tested
- Documentation complete

## 6.Budget Estimation

### Monthly Operating Costs (USD)

**Compute Services:**

- App Runner (1 services): $5-15/month
- AWS Lambda: $0-2/month
- Amplify Hosting: $0-5/month

**Storage & Database:**

- S3 Storage: $0-1/month
- DynamoDB: $0-2/month
- CloudFront Data Transfer: $0-2/month

**Streaming Services:**

- Amazon IVS (100 hours/month): $150-300/month
- AWS Elemental MediaConvert: $20-50/month

**Security & Monitoring:**

- Cognito: $0/month

**CI/CD**

- CodePipeline & CodeBuild: $1-3/month
- ERC: $0-1/month
- [Calculator](https://calculator.aws/#/estimate?id=89df8e5261796a621e5cafe03560859b27336a22)

  **Total Monthly Cost: $17-44/month**

## 7.Risks Assessment

### Primary Risks

**Technical Risks:**

- Integration complexity → Start simple, gradually increase
- Time management → Build buffer time, prioritize core features

**Resource Risks:**

- Exceeding AWS Free Tier → Monitor usage, set up alerts

### Mitigation Solutions

**Technical Management:**

- CloudFormation templates
- Phase-by-phase testing
- Dev/staging environments

**Contingency Plans:**

- **MVP:** Basic video upload/playback
- **Core:** User auth + streaming
- **Advanced:** Live streaming (optional)
- Use AWS Educate credits
- Mock services for demos

## 8.Expected Outcomes

### Performance Metrics

**System Performance:**

- Video upload success rate: >95%
- Streaming latency: <3 seconds
- System uptime: >99%
- Concurrent users: 100+
- Page load times: <2 seconds

### Success Criteria

**MVP Requirements:**

- User registration/login
- Basic video upload/playback
- Secure authentication
- Responsive interface
- System monitoring

**Stretch Goals:**

- Live streaming capabilities
- Advanced analytics
- Social features
- Mobile companion appld buffer time, prioritize core features

**Resource Risks:**

- Exceeding AWS Free Tier → Monitor usage, set up alerts

### Mitigation Solutions

**Technical Management:**

- CloudFormation templates
- Phase-by-phase testing
- Dev/staging environments

**Contingency Plans:**

- **MVP:** Basic video upload/playback
- **Core:** User auth + streaming
- **Advanced:** Live streaming (optional)
- Use AWS Educate credits
- Mock services for demos

## 8.Expected Outcomes

### Performance Metrics

**System Performance:**

- Video upload success rate: >95%
- Streaming latency: <3 seconds
- System uptime: >99%
- Concurrent users: 100+
- Page load times: <2 seconds

### Success Criteria

**MVP Requirements:**

- User registration/login
- Basic video upload/playback
- Secure authentication
- Responsive interface
- System monitoring

**Stretch Goals:**

- Live streaming capabilities
- Advanced analytics
- Social features
- Mobile companion app

### Attachments / References

- [Document](https://docs.google.com/document/d/1mFfQa_uaCAm5v0vkNDjvgzCpmMjl6qLx/edit)

- [Video](https://www.youtube.com/watch?v=b-yUXvy9HMY)

- [Slide](https://www.canva.com/design/DAG65vZ0WMw/_fH1tj8I_9JLBne3YUD-wQ/edit)
