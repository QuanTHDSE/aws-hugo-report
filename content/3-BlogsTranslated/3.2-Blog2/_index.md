---
title: "Blog 2"
date: "2025-11-30"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

## [**AWS Web3 Blog**](https://aws.amazon.com/blogs/web3/)

**Using a DAO to Govern LLM Training Data, Part 3: From IPFS to the Knowledge Base**  
Author: Shankar Subramaniam | Date: 01/11/2024 | Category: [Advanced (300)](https://aws.amazon.com/blogs/web3/category/learning-levels/advanced-300/), [Amazon Bedrock](https://aws.amazon.com/blogs/web3/category/artificial-intelligence/amazon-machine-learning/amazon-bedrock/), [Artificial Intelligence](https://aws.amazon.com/blogs/web3/category/artificial-intelligence/), [Blockchain](https://aws.amazon.com/blogs/web3/category/blockchain/), [Generative AI](https://aws.amazon.com/blogs/web3/category/artificial-intelligence/generative-ai/), [Technical How-to](https://aws.amazon.com/blogs/web3/category/post-types/technical-how-to/) | [Permalink](https://aws.amazon.com/blogs/web3/use-a-dao-to-govern-llm-training-data-part-3-from-ipfs-to-the-knowledge-base/) | [Share](https://aws.amazon.com/vi/blogs/web3/use-a-dao-to-govern-llm-training-data-part-3-from-ipfs-to-the-knowledge-base/#)

In [Part 1](https://aws.amazon.com/blogs/database/part-1-use-a-dao-to-govern-llm-training-data-retrieval-augmented-generation/) of this series, we introduced the concept of using a [Decentralized Autonomous Organization](https://ethereum.org/en/dao/) (DAO) to govern the lifecycle of an AI model, focusing on **accepting training data**. We outlined the overall architecture, set up a **knowledge base for LLMs** using [Amazon Bedrock](https://aws.amazon.com/bedrock/), and synchronized it with Ethereum Improvement Proposals (EIPs).

In [Part 2](https://aws.amazon.com/blogs/database/part-2-use-a-dao-to-govern-llm-training-data-the-smart-contract/), we created and deployed a minimal smart contract on the Ethereum Sepolia testnet using Remix and MetaMask, thereby establishing a mechanism to govern which training data can be uploaded to the knowledge base and who has the permission to do so.

In this post, we will set up [Amazon API Gateway](https://aws.amazon.com/api-gateway) and deploy [AWS Lambda](http://aws.amazon.com/lambda) functions to copy data from the InterPlanetary File System (IPFS) to [Amazon Simple Storage Service](http://aws.amazon.com/s3) (Amazon S3) and initiate a data ingestion task to the knowledge base.

### **Solution Overview**

In Part 2, we created a smart contract containing IPFS file identifiers and Ethereum addresses that are permitted to upload the contents of those IPFS files for model training.

In this post, we focus on the path that data can take from a trusted data provider (the owner of one of those Ethereum addresses) to the LLM knowledge base. The following diagram illustrates the data flow.

![][image1]

We will create the following components to implement this data flow:

* A Lambda function called s32kb, which is used to refresh the contents of the knowledge base when new content is added to the S3 bucket

* An S3 trigger to invoke the s32kb Lambda function

* A Lambda function called ipfs2s3, which is used to download content from IPFS to the S3 bucket

* An API Gateway to invoke the ipfs2s3 function

### **Prerequisites**

Review the prerequisites mentioned in [Part 1](https://aws.amazon.com/blogs/database/part-1-use-a-dao-to-govern-llm-training-data-retrieval-augmented-generation/) of this series, and complete the steps in [Part 1](https://aws.amazon.com/blogs/database/part-1-use-a-dao-to-govern-llm-training-data-retrieval-augmented-generation/) and [Part 2](https://aws.amazon.com/blogs/database/part-2-use-a-dao-to-govern-llm-training-data-the-smart-contract/) to set up the necessary components of the solution.

### **Setting Up the s32kb Lambda Function**

In this section, we will go through the steps to set up the s32kb function.

#### **Creating IAM Role for s32kb**

Before creating the Lambda function, you need to create an [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM) **role** that the Lambda function will use during execution. Perform the following steps:

1. Open terminal [AWS CloudShell](https://aws.amazon.com/cloudshell/) and upload the following files:

   * [s32kb\_trust\_policy.json](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/s32kb_trust_policy.json) – trust policy used to create a role for the s32kb function

   * [s32kb\_inline\_policy\_template.json](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/s32kb_inline_policy_template.json) – inline policy used to create a role for the s32kb function

   * [s32kb.py](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/s32kb.py) – Python script to create a Lambda function that automatically updates the knowledge base when new files are uploaded to the S3 bucket

   * [s32kb.py.zip](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/s32kb.py.zip) – .zip file containing Lambda source code ([s32kb.py](http://s32kb.py))

2. Create a JSON document for the inline policy (this policy is necessary to grant the Lambda function permission to write logs to [Amazon CloudWatch](http://aws.amazon.com/cloudwatch)):  
   ACCOUNT=$(aws sts get-caller-identity \--query "Account" \--output text) && \\  
   cat s32kb\_inline\_policy\_template.json | sed \-e s+ACCOUNT+$ACCOUNT+g \> s32kb\_inline\_policy.json  
     
     
     
3. Create IAM role:   
   aws iam create-role \\  
   \--role-name s32kb \\  
   \--assume-role-policy-document file://s32kb\_trust\_policy.json  
4. Attach managed policy **AmazonBedrockFullAccess**:  
   aws iam attach-role-policy \--role-name s32kb \--policy-arn arn:aws:iam::aws:policy/AmazonBedrockFullAccess

5. Attach the inline policy just created:  
   aws iam put-role-policy \\  
   \--role-name s32kb \\  
   \--policy-name s32kb\_inline\_policy \\  
   \--policy-document file://s32kb\_inline\_policy.json  
   

#### **Creating the s32kb Lambda Function**

Perform the following steps to create the s32kb function:

1. Open the s32kb.py file with your preferred editor (using vi here) and view the contents inside.

    This file initializes an Amazon Bedrock agent and uses this agent to start a knowledge base ingestion job. Additionally, you need to set up two environment variables:

   * KB\_ID – contains the ID of the knowledge base

   * KB\_DATA\_SOURCE\_ID – contains the ID of the data source (S3 bucket)

2. Perform the following steps to look up those values:

   * On the Amazon Bedrock console, select **Knowledge bases** in the navigation pane.

   * Select the crypto-ai-kb knowledge base.

   * Record the ID of the knowledge base in the **Knowledge base overview** section.

   * In the **Data source** section, select the EIPs data source.

   * Record the ID of the data source in the **Data source overview** section.

3. Save these values in CloudShell:

export KB\_ID=*\<Knowledge base ID\>*

export KB\_DATA\_SOURCE\_ID=*\<Data source ID\>*

4. Create the Lambda function:

ACCOUNT=$(aws sts get-caller-identity \--query "Account" \--output text) && \\

aws lambda create-function \\

\--function-name s32kb \\

\--timeout 300 \\

\--runtime python3.12 \\

\--architectures x86\_64 \\

\--zip-file fileb://s32kb.py.zip \\

\--handler s32kb.handler \\

\--role arn:aws:iam::$ACCOUNT:role/s32kb \\

\--environment  Variables=\\{KB\_ID=$KB\_ID,KB\_DATA\_SOURCE\_ID=$KB\_DATA\_SOURCE\_ID\\}

### **Creating an S3 Trigger to Invoke the s32kb Lambda Function**

Complete the following steps to configure a trigger to automatically run the s32kb function whenever a new file is uploaded to the S3 bucket:	

1. On the Lambda console, select **Functions** in the navigation pane.

2. Select the s32kb function.

3. Select **Add trigger**.

4. In the **Trigger configuration** section, at **Select a source**, select **S3**.

5. For **Bucket**, select crypto-ai-kb-*\<your\_account\_id\>*

6. For **Event types**, select **All object create events** and **All object delete events**.

7. Select the acknowledgement check box.

8. Select **Add**.

---

### **Testing the s32kb Lambda Function**

Let's add a new file to the bucket and verify that the Lambda function has been invoked through the trigger. To continue previous research on **danksharding** in [Part 1](https://aws.amazon.com/blogs/database/part-1-use-a-dao-to-govern-llm-training-data-retrieval-augmented-generation/), we will enrich the knowledge base with documentation of the network upgrade specification "Cancun":

Open the CloudShell terminal and enter the following commands:

ACCOUNT=$(aws sts get-caller-identity \--query "Account" \--output text) && \\

wget https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/cancun.md && \\

aws s3 cp ./cancun.md s3://crypto-ai-kb-$ACCOUNT/

Verify that the Lambda function has been executed successfully:

* On the CloudWatch console, navigate to the /aws/Lambda/s32kb log group.

* Verify that there is a log stream with **Last Event Time** matching the current time, and select it.

* View the log and confirm that the Lambda function returns 202 HTTPStatusCode.

Additionally, check the status of the Amazon Bedrock job:

* On the Amazon Bedrock console, navigate to the crypto-ai-kb knowledge base.

* In the *Data source* section, confirm that the **Last sync time** value matches the current time.

If you want to go further, you can query the knowledge base for specific information mentioned in this network upgrade specification.

### **Setting Up the ipfs2s3 Lambda Function**

In this section, we will go through the steps to set up the ipfs2s3 function.

#### **Creating IAM Role for ipfs2s3**

Complete the following steps to create an IAM role for ipfs2s3:

1. Open the **CloudShell** terminal and upload the following files:

   * [ipfs2s3\_trust\_policy.json](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/ipfs2s3_trust_policy.json) – trust policy used to create a role.

   * [ipfs2s3\_inline\_policy\_template.json](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/ipfs2s3_inline_policy_template.json) – inline policy used to create a role.

   * [ipfs2s3.py](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/ipfs2s3.py) – Python script to create a Lambda function that downloads files from IPFS to the S3 bucket.

   * [ipfs2s3.py.zip](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/ipfs2s3.py.zip) – .zip file containing Lambda code (ipfs2s3.py).

2. Create a JSON document for the inline policy (this policy is necessary to grant the Lambda function permission to write logs to CloudWatch):

ACCOUNT=$(aws sts get-caller-identity \--query "Account" \--output text) && \\

cat ipfs2s3\_inline\_policy\_template.json | sed \-e s+ACCOUNT+$ACCOUNT+g \> ipfs2s3\_inline\_policy.json

3. Create Lambda execution role:

aws iam create-role \\

\--role-name ipfs2s3 \\

\--assume-role-policy-document file://ipfs2s3\_trust\_policy.json

4. Attach managed policy AmazonS3FullAccess:

aws iam attach-role-policy \--role-name ipfs2s3 \--policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess

5. Attach the inline policy just created:

aws iam put-role-policy \\

\--role-name ipfs2s3 \\

\--policy-name ipfs2s3\_inline\_policy \\

\--policy-document file://ipfs2s3\_inline\_policy.json

#### **Creating the ipfs2s3 Lambda Function**

To download a file from the IPFS network and upload it to Amazon S3, we create the ipfs2s3 Lambda function that connects to a public IPFS gateway to download the CID passed in as an event parameter to Lambda. The function then uploads the downloaded file to the S3 bucket configured in the environment variable. You also provide the file name to use as an event parameter.

To increase security and reliability, you can create an **IPFS node (or cluster)** in your environment and update the IPFS\_GW\_ENDPOINT environment variable to point to your own IPFS gateway. For detailed instructions on how to create your own IPFS infrastructure, refer to the [IPFS on AWS](https://aws.amazon.com/blogs/database/part-1-ipfs-on-aws-discover-ipfs-on-a-virtual-machine/) series.

Open the [ipfs2s3.py](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/ipfs2s3.py) file and view the contents. Then, create the Lambda function using the following command:

ACCOUNT=$(aws sts get-caller-identity \--query "Account" \--output text) && \\

aws lambda create-function \\

\--function-name ipfs2s3 \\

\--timeout 30 \\

\--runtime python3.12 \\

\--architectures x86\_64 \\

\--zip-file fileb://ipfs2s3.py.zip \\

\--handler ipfs2s3.handler \\

\--role arn:aws:iam::$ACCOUNT:role/ipfs2s3 \\

\--environment Variables=\\{IPFS\_GW\_ENDPOINT=https://ipfs.io,S3\_TARGET\_BUCKET=crypto-ai-kb-$ACCOUNT\\}

### **Creating an API Gateway**

Complete the following steps to create an API Gateway:

1. On the **API Gateway console**, select **APIs** in the navigation pane.

2. Select **Create API**.

3. In the **HTTP API** section, select **Build**.

4. For **API name**, enter a name (e.g., crypto-ai).

5. In ***Create and configure integrations***, select ***Add integration***.

6. Select ***Lambda***.

7. For **Lambda function**, select the ipfs2s3 function.

8. For **Version**, select **2.0**.

9. Select **Next**.

10. Review the default route and select **Next**.

11. Review the default stage and select **Next**.

12. Select **Create**.

13. Open the API just created and record the default endpoint:

export API\_ENDPOINT=*\<Default endpoint\>*

### **Testing the ipfs2s3 Lambda Function**

Now you can verify that you can invoke the Lambda function through the API Gateway endpoint.

Let's assume we want to supplement the knowledge base with [Amazon Managed Blockchain (AMB) – Ethereum Developer Guide](https://docs.aws.amazon.com/pdfs/managed-blockchain/latest/ethereum-dev/amazon-managed-blockchain-ethereum-dev.pdf).

1. Upload this guide to an IPFS pinning service like Filebase and record the CID (your CID may be different):

export CID=QmWGTo7gVXvdX2YWJg3hKH5JksXsL5tRBfiKLY9MxRbuLS && \\

export FILENAME=amazon-managed-blockchain-ethereum-dev.pdf

2. Call the API endpoint:

curl $API\_ENDPOINT/ipfs2s3?cid=$CID\\\&filename=$FILENAME

You will receive a message similar to the following:

"Successfully uploaded [amazon-managed-blockchain-ethereum-dev.pdf](https://console.filebase.com/object/crypto-ai/amazon-managed-blockchain-ethereum-dev.pdf) from CID QmWGTo7gVXvdX2YWJg3hKH5JksXsL5tRBfiKLY9MxRbuLS"

3. Verify that the amazon-managed-blockchain-ethereum-dev.pdf file has been uploaded to the S3 bucket and the knowledge base data source has been synchronized.

Additionally, you can query the knowledge base for specific information mentioned in this guide.

### **Clean Up**

You can keep the components built in this post because they will be reused in the next section. Or, you can follow the cleanup instructions in Part 4 to delete them.

### **Conclusion**

In Part 3 of this four-part series, we showed how to create two Lambda functions and an API Gateway that allow you to automatically update an Amazon Bedrock knowledge base with data from the IPFS network. In [Part 4](https://aws.amazon.com/blogs/database/part-4-use-a-dao-to-govern-llm-training-data-metamask-authentication/), we will show how to create a frontend and use MetaMask to authenticate users with their web3 identity.

### **About the Authors**

**Guillaume Goutaudier** is a Senior Enterprise Architect at AWS. He supports companies in building strategic technical partnerships with AWS. He is also passionate about blockchain technologies and is a member of the Technical Field Community on blockchain.

**Shankar Subramaniam** is a Senior Enterprise Architect in the AWS Partner Organization, responsible for collaboration and governance activities within the Strategic Partnership Collaboration and Governance (SPCG) framework. He is a member of the Technical Field Community on Artificial Intelligence and Machine Learning.