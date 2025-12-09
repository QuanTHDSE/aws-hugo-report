---
title: "Blog 1"
date: "2025-11-30"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

## [AWS Storage Blog](https://aws.amazon.com/blogs/storage/)

# Automating Paper-to-Electronic Healthcare Claims Processing with AWS

By Anil Chinnam and Chris Bladon | May 29, 2025 | In [Amazon Bedrock](https://aws.amazon.com/blogs/storage/category/artificial-intelligence/amazon-machine-learning/amazon-bedrock/), [Amazon Simple Storage Service (S3)](https://aws.amazon.com/blogs/storage/category/storage/amazon-simple-storage-services-s3/), [AWS Lambda](https://aws.amazon.com/blogs/storage/category/compute/aws-lambda/), [AWS Transfer Family](https://aws.amazon.com/blogs/storage/category/migration/aws-transfer-family/), [Generative AI](https://aws.amazon.com/blogs/storage/category/artificial-intelligence/generative-ai/), [Intermediate (200)](https://aws.amazon.com/blogs/storage/category/learning-levels/intermediate-200/), [Migration & Transfer Services](https://aws.amazon.com/blogs/storage/category/migration/), [Storage](https://aws.amazon.com/blogs/storage/category/storage/), [Technical How-to](https://aws.amazon.com/blogs/storage/category/post-types/technical-how-to/) | [Permalink](https://aws.amazon.com/blogs/storage/automating-paper-to-electronic-healthcare-claims-processing-with-aws/) | [Comments](https://aws.amazon.com/blogs/storage/automating-paper-to-electronic-healthcare-claims-processing-with-aws/#Comments) | [Share](https://aws.amazon.com/vi/blogs/storage/automating-paper-to-electronic-healthcare-claims-processing-with-aws/#)

Healthcare insurance programs process billions of electronic claims annually. The Council for Affordable Quality Healthcare (CAQH) estimates that approximately 10% of claims still arrive as paper documents, equivalent to hundreds of millions of paper records sent annually in the United States. These paper claims create processing bottlenecks and consume disproportionate operational resources, with manual processing costs up to 10 times higher than electronic processing, extending claim cycles from hours to weeks.

Operations teams currently process various paper forms—CMS-1500 for professional services, UB-04 for hospital services, ADA dental forms, and custom proprietary forms—through labor-intensive processes including form digitization and data entry. Unlike electronic claims that flow through standard EDI (Electronic Data Interchange) channels, paper claims require separate systems using OCR technology and proprietary formats. This parallel approach leads to higher error rates and increased HIPAA compliance risks. For an average health insurance program, these inefficiencies can result in millions of dollars in administrative costs annually that could otherwise be allocated to member services and care management initiatives.

To address these paper processing challenges, we built a solution that integrates seamlessly with existing electronic claims processing systems while minimizing manual intervention. In this post, we demonstrate how health insurance programs can automate the conversion from paper to electronic claims using [AWS Transfer Family web apps](https://aws.amazon.com/aws-transfer-family/), [Amazon Bedrock Data Automation (BDA)](https://aws.amazon.com/bedrock/bda/), and [AWS B2B Data Interchange](https://aws.amazon.com/b2b-data-interchange/). Although this solution focuses on converting UB-04 hospital claim forms to 5010 X12 837 format (HIPAA-mandated electronic standard), the same architecture can be applied to automate paper-based processes in other industries such as insurance, supply chain, retail, and manufacturing, where businesses need to transform physical documents into standard EDI electronic data.

### **Solution Overview**

This solution transforms healthcare claims from paper to standardized electronic transactions through a streamlined three-stage process using AWS serverless architecture, as illustrated below. The combination of document processing, AI-powered data extraction, and EDI conversion capabilities enables insurance programs to eliminate manual data entry while maintaining HIPAA compliance.

The following diagram illustrates the architecture:

![][image1]

*Figure 1. Architecture diagram showing paper claim data transformation to 837 transactions*

The end-to-end workflow proceeds as follows:

### **Stage 1: Secure File Reception with AWS Transfer Family Web App**

1. Claims operations teams upload scans of paper claims through the Transfer Family web application interface, which provides secure, browser-based access to specific [Amazon S3](https://aws.amazon.com/s3/) buckets.

### **Stage 2: Intelligent Data Extraction with BDA**

2. Amazon S3 detects files uploaded to the **input bucket prefix** and emits a corresponding [Amazon EventBridge](https://aws.amazon.com/eventbridge/) event. Amazon EventBridge rules detect these events and trigger [AWS Lambda](https://aws.amazon.com/lambda/) functions to process the claims.

3. The Lambda function calls BDA to intelligently extract structured data from claim forms.

### **Stage 3: EDI Conversion and Generation with AWS B2B Data Interchange**

4. B2B Data Interchange monitors the S3 location for new JSON documents and automatically converts extracted data into standardized 837 EDI transactions.

5. The resulting EDI files are delivered to an S3 bucket, ready for integration with the health insurance program's existing claims processing system.

### This architecture delivers significant business results directly impacting insurance program profitability:

* Reduce claim processing costs by up to 80% through intelligent data extraction and automated processing

* Compress processing time from multiple days to minutes, accelerating reimbursement cycles for healthcare providers

* Improve data accuracy with lower error rates compared to manual processing

* Enhance HIPAA compliance through end-to-end encryption, granular access controls, and comprehensive audit logging

* Increase operational efficiency, enabling staff to focus on high-value member service activities instead of data entry

* Enable seamless scalability from hundreds to millions of claims annually without capacity planning concerns

All services used in this solution are [HIPAA Eligible Services](https://aws.amazon.com/compliance/hipaa-eligible-services-reference/), meaning they qualify for workloads involving protected electronic health information (ePHI) when covered by a [Business Associate Addendum (BAA)](https://aws.amazon.com/compliance/hipaa-compliance/) with AWS.

Unlike traditional OCR and EDI solutions requiring significant upfront investment, this serverless approach delivers consistent performance during processing peaks while minimizing infrastructure management costs.

In the following sections, we explore each stage of this process in detail, examining how AWS services work together to create a seamless claims processing experience while addressing key operational challenges: reducing manual processing errors, accelerating reimbursement cycles, and cutting costs.

### **Stage 1: Secure File Reception with AWS Transfer Family Web App**

The first challenge in automating paper claims processing is secure, user-friendly document reception. Operations teams need a clear and secure method to upload digitized versions of paper claims. **Transfer Family web apps** are fully-managed, code-free, browser-based applications allowing authenticated users to list, upload, download, copy, and delete files within specific [Amazon S3](https://aws.amazon.com/s3/) buckets. This solution provides operations teams a **zero-code interface** for securely submitting healthcare forms.

The service authenticates users through [AWS Identity and Access Management (IAM) Identity Center](https://aws.amazon.com/iam/identity-center/), allowing employees to use existing organizational credentials through industry-standard protocols (SAML 2.0 or OIDC). After authentication, users access a customized and branded web portal where they can upload claim forms (CMS-1500, UB-04, ADA dental, or custom forms) using intuitive drag-and-drop functionality, as shown in Figure 2.

![][image2]

*Figure 2. Transfer Family web app interface showing file management options*

The web interface not only simplifies the upload process but also provides inspection capabilities and comprehensive audit logging of each action, supporting HIPAA compliance requirements. Users can perform both individual and batch uploads as needed. Figure 3 illustrates how uploaded UB-04 forms are organized.

![][image3]  
*Figure 3. Uploading UB-04 forms to the destination directory*

**Security is maintained throughout the process:** all files are encrypted in transit using **SSL/TLS** and at rest using [AWS Key Management Service (AWS KMS)](https://aws.amazon.com/kms/) in Amazon S3. When new forms arrive in the **designated input prefix**, an EventBridge rule automatically triggers a Lambda function, moving them to **Stage 2** of the pipeline, where BDA performs intelligent data extraction.

### **Stage 2: Intelligent Data Extraction with BDA**

After secure reception, the next critical challenge is accurately extracting structured data from complex medical forms. Bedrock Data Automation (BDA) is a fully-managed capability within [Amazon Bedrock](https://aws.amazon.com/bedrock/) that simplifies extracting valuable information from unstructured multi-modal content like documents, images, audio, and video. BDA provides a unified API experience, eliminating the need to manage multiple AI models and services, while including built-in visual grounding and confidence scores for validating extracted data.

For document and image types, BDA allows you to define when data should be extracted as-is ([standard](https://docs.aws.amazon.com/bedrock/latest/userguide/bda-standard-output.html)) and when it should be inferred ([custom output](https://docs.aws.amazon.com/bedrock/latest/userguide/bda-custom-output-idp.html)), providing complete control over the process. This flexibility is provided through blueprint configurations.

**Extracting healthcare data with BDA blueprints**

In BDA, a blueprint defines how data is extracted and processed from documents. Each blueprint includes field definitions, instructions, validation rules, and output schema specifications, all customizable for different healthcare form types. Blueprints can be created using natural language prompts in the console, or manually defined for more precise control.

Technical teams should create separate blueprints for CMS-1500, UB-04, ADA dental forms, and custom forms, refining them over time as needed. Each blueprint specifies exactly which data elements should be extracted from each form type and how they should be structured.

Figure 4 illustrates a UB-04 form blueprint configuration in the AWS console, where you can define fields such as patient information, diagnosis codes, and procedure details.

![][image4]

*Figure 4. Extracting UB-04 form data in the AWS console showing field-level instructions, results, and confidence scores*

When processing claim forms, the Lambda function triggered by EventBridge calls BDA APIs to analyze the document, classify it into a specific form type, apply the appropriate extraction blueprint, and extract relevant healthcare data. The resulting structured JSON document contains all information from the original paper claim, ready for further processing.

Figure 5 displays the structured JSON output produced by Amazon Bedrock Data Automation from a UB-04 form. This transformation from unstructured document data to structured, machine-readable fields is critical for subsequent processing, with each key-value pair representing important claim information extracted from the paper form.

![][image5]  
*Figure 5. JSON document with fields extracted from UB-04 form using BDA*

Here's an example of data extracted in JSON from a sample UB-04 document, highlighting some key fields:

JSON  
{  
    "claim": {  
        "admit\_diagnosis": "E871",  
        "statement\_covers\_period": "01012024 through 03012024",  
        "patient\_birthdate": "08221967",  
        "provider\_address": "555 Main St",  
        "npi": "0011002200"  
    }  
}

Once extraction completes, the Lambda function writes this structured JSON document to an S3 prefix monitored by B2B Data Interchange, creating seamless transition to the EDI conversion stage without manual intervention.

### **Stage 3: EDI Conversion and Generation with AWS B2B Data Interchange**

The structured data extracted from paper claims means the final critical step is converting this information into standardized HIPAA 5010 X12 837 transactions, processable by existing claims systems. Electronic Data Interchange (EDI) is the standard format used throughout healthcare for electronic transactions between providers, payers, and other stakeholders.

B2B Data Interchange automates converting and generating Electronic Data Interchange (EDI) documents from and to JSON and XML data formats. This service provides health insurance plans with a low-code interface and pay-as-you-go pricing model, reducing time, complexity, and costs associated with managing and exchanging transaction data across organizational boundaries.

**Understanding the EDI conversion process**

B2B Data Interchange simplifies converting extracted UB-04 JSON data into standard 837 Institutional transactions through four components:

* **Profiles**: Store organizational business details and contact information. This information is shared with trading partners.

* **Transformers**: Define how extracted JSON data maps to standard EDI segments in 837 format.

* **Trading capabilities**: Establish automated processing pipelines to monitor S3 locations and trigger conversion processes.

* **Partnerships**: Connect components together to enable automated document exchange.

A key innovation in B2B Data Interchange is AI-powered EDI mapping, powered by [Amazon Bedrock](https://aws.amazon.com/bedrock/). The "Generate Mapping" feature shown in Figure 6 analyzes extracted claim data and sample EDI transactions to automatically suggest appropriate mappings, reducing implementation time from days to minutes while making EDI conversion more accessible to teams without deep expertise.

![][image6]

*Figure 6. AWS B2B Data Interchange with mapping generation capability*

After configuration, the entire conversion process is automated:

1. B2B Data Interchange monitors the Amazon S3 prefix for new JSON documents from BDA  
2. Upon detection, it converts the structured JSON data into X12 837 documents  
3. Optionally validates converted documents against X12 standards  
4. Resulting EDI files are delivered to a specified output folder for integration

Here's a sample snippet from a generated X12 file, showing main claim components:

ST\*837\*0001\*005010\~ (Transaction start segment)

BHT\*0019\*00\*0123\*20250422\*2020\*CH\~ (Header information)

NM1\*41\*2\*Acme Hospital\*\*\*\*\*46\*12345\~ (Provider data)

PER\*IC\*JOHN DOE\*TE\*1234567890\~ (Contact information)

The final output of this process is a set of standardized EDI files ready for integration with your existing claims processing system. Each file contains all structured healthcare information extracted from the original paper form. The files follow standard naming conventions and are organized by trading partner ID for easy integration.

![][image7]

*Figure 7. HIPAA 5010 837 X12 files written to output S3 prefix*

All conversion operations are logged to [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) and emit events to Amazon EventBridge, enabling comprehensive monitoring of document counts, conversion status, and errors. This allows operations teams to track document volumes, conversion status, and processing errors through customized CloudWatch dashboards.

### **Next Steps**

Now that you understand how the solution works, you can deploy it in your AWS environment. Before launching the CloudFormation stack, complete these prerequisites:

### Prerequisites

* Configure AWS IAM Identity Center

  * Enable AWS IAM Identity Center per [this documentation](https://docs.aws.amazon.com/singlesignon/latest/userguide/enable-identity-center.html#to-enable-identity-center-instance).

  * Configure your identity source (AWS Managed or external IdP).

  * Create a group named 'ClaimsOperationsTeam' per [quick start guide](https://docs.aws.amazon.com/singlesignon/latest/userguide/quick-start-default-idc.html).

  * Record the Group ID in the General Information section.

  * Configure MFA settings as needed.

* Enable Access Grants in your region

  * Follow [this documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-grants-instance-create.html) to create an access grant instance and associate it with your Identity Center ARN.

### **Deploying the Solution**

Use the **"Launch Stack"** button below to deploy the [AWS CloudFormation](https://aws.amazon.com/cloudformation/) template to your AWS account.

[**![][image8]**](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateURL=https://awsstorageblogresources.s3.us-west-2.amazonaws.com/blog1555/paper_claim_automation_transfer_family.yaml)

When prompted, provide the following parameters:

* BusinessName: Your company name

* GranteeIdentifier: Group ID from step 1

* IdentityCenterInstanceArn: Identity Center ARN (format: arn:aws:sso:::instance/ssoins-xxxxxxxxxxxxxxxx)

* InboundPath: inbound/

* NamePrefix: paper-claims

* OutboundPath: outbound/

### **Verifying Deployment**

After deployment completes:

1. Assign the **'ClaimsOperationsTeam'** group to the web app in the [AWS Transfer Family console](https://console.aws.amazon.com/transfer/home).

2. Access the web app URL and log in.

3. Upload a sample UB-04 file to the **inbound** directory (download [sample file](https://awsstorageblogresources.s3.us-west-2.amazonaws.com/blog1555/example-paper-claim-ub04.pdf)).

4. Verify that EDI results appear in the outbound directory.

---

### **Cleanup**

To avoid incurring future costs, delete the CloudFormation stack. For instructions, see the [delete stack](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-delete-stack.html) section in the [CloudFormation console](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-delete-stack.html).

### **Conclusion**

In this post, we demonstrated how to transform paper healthcare claims into electronic format using AWS services. This solution enables health insurance plans to reduce processing costs by up to 80%, compress processing time from days to minutes, and improve data accuracy—all while meeting HIPAA requirements. By automating these labor-intensive manual processes, organizations can redirect valuable resources away from data entry toward member-focused initiatives. Begin your claims processing automation journey with our CloudFormation template and discover how it can address your specific processing challenges.

**TAGS:** [Amazon Simple Storage Service (Amazon S3)](https://aws.amazon.com/blogs/storage/tag/amazon-simple-storage-service-amazon-s3/), [AWS Cloud Storage](https://aws.amazon.com/blogs/storage/tag/aws-cloud-storage/), [AWS Lambda](https://aws.amazon.com/blogs/storage/tag/aws-lambda/), [AWS Transfer Family](https://aws.amazon.com/blogs/storage/tag/aws-transfer-family/)

### Anil Chinnam

Anil Chinnam is a Solutions Architect at AWS. He is passionate about Generative AI and dedicated to transforming advanced technologies into tangible business value for healthcare customers. As a trusted technical advisor, he helps customers drive cloud technology adoption and business transformation outcomes.

### Chris Bladon

Chris Bladon is a Senior Solutions Architect at AWS. He specializes in supporting healthcare payers and life sciences customers in building innovative solutions using AWS generative AI services such as Amazon Bedrock and Amazon Q Developer.