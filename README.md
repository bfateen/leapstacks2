## Launch Environment for AWS Prototypes (LEAP)

LEAP is a web-based, open-source environment that allows pre-built prototypes to be launched instantly and modified within only 7 steps. It is designed for seamless integration with existing AWS accounts, requiring no modifications to the current AWS user interface. 

This solution  enables rapid deployment of full-stack AWS prototypes through a secure web interface. The system consists of two main components:
1. A script to install the web app into your AWS account
2. Deployable prototype templates

The environment is setup with the following these steps: 1/ User watches a short video that describes the simple setup process and basic info on CloudFormation and Amazon Bedrock model access, 2/ User requests and is granted access to an AI Model, 3/ User uploads the setup CloudFormation script which installs the web app and sets up all the necessary roles and permissions, 4/ User clicks on the URL for the web app to open it in their browser and chooses from pre-loaded prototypes, 5/ The CloudFormation stack is deployed on the backend and returns the new resources, 6/ Links to the assets are shown clearly to the user (frontend URL, API endpoint, link to Lambda function, etc.), 7/ A brief description about the generated resources and how they interact along with clickable links to 30 second videos that explain concepts (i.e. Prompt Engineering) and services (i.e. API Gateway) which creates a personalized learning path on the right side column.


### Main Features:
1.	Dynamic loading of custom-built prototypes from a GitHub repository
2.	One-click prototype deployment system for Generative AI applications
3.	Choose from several Bedrock LLMs including Amazon Nova, Anthropic Claude, etc.
4.	Real-time deployment status monitoring, progress tracking and removal with cleanup
5.	Auto-deleting prototypes with all resources (2 hour default) to prevent unexpected charges
6.	Direct links to AWS console for deployed services (S3, Lambda, API Gateway)
7.	Interactive architecture visualization for deployed prototypes
8.	Integrated explainer system with video tutorials and documentation
9.	Persistent state management using local storage
10.	CORS-enabled API endpoints for secure cross-origin requests

<img width="1450" alt="leap_screenshot1" src="https://github.com/user-attachments/assets/4c249116-7e60-42d5-8c70-0d471bfe3f95" />

<img width="987" alt="leap_screenshot2" src="https://github.com/user-attachments/assets/de6239b7-2a98-46e1-a068-353d59dee3af" />

The following initial custom prototypes for idea validation and MVPs will be included in the first version (to be released following AppSec review on November 15th)
- A full-stack serverless Generative AI chatbot app with a Rest API endpoint
- A ‘chat with your documents’ Knowledge Base web app using Cognito to secure endpoints
- A 'chat with a web site' application that uses a headless browser to take snapshots of a website and use the multimodal features of Amazon nova to analyze the image and answer questions and monitor for changes

   
## 🎯 Manual Installation

1. Download 'leap-installer-setup.yaml'
2. Log into your AWS console and navigate to CloudFormation
3. Click 'Create Stack' -> 'With new resources' -> 'Upload a template file' and select 'leap-installer-setup.yaml'
4. Continue and submit
5. When stack has completed (about 30 seconds) click 'Outputs' for login instructions

<img width="1409" alt="Screenshot 2024-12-12 at 11 08 36 PM" src="https://github.com/user-attachments/assets/38b8fe81-0c4a-432e-a096-80a730fa7c1d" />

<img width="1382" alt="Screenshot 2024-12-12 at 11 08 47 PM" src="https://github.com/user-attachments/assets/db8a73a4-0390-4454-ac52-eb4bd3eb6d89" />


## 🚀 Quick Start

1. Clone this repository
2. Upload the template to an S3 bucket (due to file size) and deploy the environment installer:
```bash

TEMPLATE_URL=$(BUCKET="leap-installer-$(date +%s)" && aws s3 mb s3://$BUCKET && aws s3 cp leap-installer-setup.yaml s3://$BUCKET/ && echo "https://$BUCKET.s3.amazonaws.com/leap-installer-setup.yaml")

aws cloudformation create-stack \
  --stack-name prototype-env \
  --template-url $TEMPLATE_URL \
  --capabilities CAPABILITY_NAMED_IAM \
  --parameters \
    ParameterKey=AdminEmail,ParameterValue=admin@example.com \
    ParameterKey=InitialPassword,ParameterValue=Initial123!
```

3. Access the web interface using the URL from stack outputs
4. Login with provided admin credentials
5. Select and deploy prototypes
   
## 🏗️ Architecture Overview

### Environment Installer Stats
- **Total AWS Services**: 12
- **Custom Resources**: 4
- **IAM Roles**: 5
- **Lambda Functions**: 6
- **API Gateway Endpoints**: 3

## Security Features

While our emphasis in this solution is on building rapid prototyping capabilities, security remains a critical consideration even as we optimize for speed and simplicity. Our primary security strategy centers on securing all endpoints and on isolation – so we will run all prototypes in dedicated sandbox AWS accounts, separate from production environments. This approach leverages AWS's strongest security boundary - account isolation - while allowing us to simplify other security controls that might otherwise create friction in the prototyping process.

We will use Coginto to secure the access to ensure:

- Only login page is publicly accessible
- Protected content requires authentication
- Error page for unauthorized access
- Proper CORS and security headers
- The Cognito JWT token received after login expires (default is 1 hour)
- The signed S3 URL generated by the Lambda function expires after 5 minutes

1. **Authentication & Authorization**
   - Cognito User Pool with secure password policies
   - API Gateway authorization
   - IAM role least privilege principle

2. **Data Protection**
   - S3 bucket encryption
   - HTTPS only access
   - Secure parameter handling

3. **Network Security**
   - Private subnet deployment options
   - CORS configuration
   - API Gateway resource policies
  
  <img width="1367" alt="Screenshot 2024-11-05 at 1 23 43 PM" src="https://github.com/user-attachments/assets/891ffb26-1102-4065-b226-edcf09020317">

To validate the security posture of prototypes created through this solution, we will follow the Content Security Review process as well as use ‘Prowler’, an industry-standard automated security assessment tool that is widely used within AWS security teams. Prowler provides comprehensive scanning for misconfigurations and potential security issues, offering more reliability than manual security reviews. This automated approach helps identify subtle security gaps that could be easily overlooked during rapid prototyping phases.

While we streamline security configurations to enable rapid experimentation, we maintain vigilance over fundamental security controls. Each prototype environment ensures proper implementation of basic security features such as client-side encryption and appropriate logging configurations. These foundational security measures are non-negotiable even in prototype environments, establishing good security practices from the start.


## 🔧 Technical Details

### Custom Resources
The solution uses several custom resources for advanced functionality:

#### 1. S3 Content Uploader
```yaml
ContentUploader:
  Type: AWS::Lambda::Function
  Properties:
    Handler: index.handler
    Code:
      ZipFile: |
        import boto3
        import cfnresponse
        
        def handler(event, context):
            try:
                if event['RequestType'] in ['Create', 'Update']:
                    s3 = boto3.client('s3')
                    bucket = event['ResourceProperties']['BucketName']
                    body = event['ResourceProperties']['Body']
                    
                    s3.put_object(
                        Bucket=bucket,
                        Key='protected.html',
                        Body=body,
                        ContentType='text/html'
                    )
```

#### 2. Cognito User Creator
```yaml
CognitoUserCreator:
  Type: AWS::Lambda::Function
  Properties:
    Handler: index.handler
    Code:
      ZipFile: |
        def handler(event, context):
            if event['RequestType'] in ['Create', 'Update']:
                cognito = boto3.client('cognito-idp')
                user_pool_id = event['ResourceProperties']['UserPoolId']
                email = event['ResourceProperties']['AdminEmail']
```

### Available Prototypes

### 1. Bedrock Chat Application
- **Services**: Lambda, API Gateway, S3
- **Features**: 
  - Real-time chat interface
  - Integration with AWS Bedrock
  - Serverless architecture
- **Deployment Time**: ~5 minutes

```yaml
Resources:
  ChatFunction:
    Type: AWS::Lambda::Function
    Properties:
      Handler: index.lambda_handler
      Code:
        ZipFile: |
          def lambda_handler(event, context):
              client = boto3.client("bedrock-runtime")
              response = client.converse(
                  modelId="anthropic.claude-3-5-sonnet-20240620-v1:0",
                  messages=[msg]
              )
```

### 2. Document Chat Application with Amazon Bedrock

A full-stack application that enables intelligent document interactions using Amazon Bedrock's Knowledge Base and Agent capabilities. This prototype demonstrates Retrieval-Augmented Generation (RAG) architecture for building AI-powered document question-answering systems.

- **Services**: Cognito, OpenSearch Serverless Collections, Amazon Bedrock, Lambda, API Gateway, S3

#### Features
- **Vector Search**: Utilizes OpenSearch Serverless for efficient document embedding storage
- **RAG Implementation**: Combines Knowledge Base retrieval with LLM generation
- **Real-time Chat**: Interactive web interface for document queries
- **Secure Storage**: Encrypted S3 buckets for document management
- **Custom Agents**: Bedrock agent with specialized instruction sets

##### Vector Store Setup
```yaml
Collection:
  Type: 'AWS::OpenSearchServerless::Collection'
  Properties:
    Name: !Ref AOSSCollectionName
    Type: VECTORSEARCH
    StandbyReplicas: DISABLED
    Description: Collection to hold vector search data
```

##### Knowledge Base Configuration
```yaml
KnowledgeBaseWithAoss:
  Type: AWS::Bedrock::KnowledgeBase
  Properties:
    Name: !Ref KnowledgeBaseName
    KnowledgeBaseConfiguration:
      Type: "VECTOR"
      VectorKnowledgeBaseConfiguration:
        EmbeddingModelArn: !Sub "arn:${AWS::Partition}:bedrock:${AWS::Region}::foundation-model/amazon.titan-embed-text-v1"
    StorageConfiguration:
      Type: "OPENSEARCH_SERVERLESS"
      OpensearchServerlessConfiguration:
        CollectionArn: !GetAtt Collection.Arn
        VectorIndexName: !Ref AOSSIndexName
```

##### Bedrock Agent Setup
```yaml
AgentResource:
  Type: AWS::Bedrock::Agent
  Properties:
    AgentName: !Ref AgentName
    FoundationModel: "amazon.titan-text-premier-v1:0"
    Instruction: "You are an HR bot tasked with matching candidates to suitable roles based on their skill sets, experience, and qualifications."
    KnowledgeBases:
      - KnowledgeBaseId: !Ref KnowledgeBaseWithAoss
        KnowledgeBaseState: ENABLED
```

#### Resource Count
| Component | Count |
|-----------|--------|
| Lambda Functions | 5 |
| IAM Roles | 4 |
| S3 Buckets | 2 |
| Custom Resources | 3 |
| OpenSearch Collections | 1 |
| Bedrock Components | 3 |

#### Security Features
- Encrypted document storage
- Private S3 buckets with strict access policies
- IAM role-based access control
- HTTPS-only API endpoints
- Secure websocket connections for real-time chat

#### Custom Resource Highlights

##### Vector Index Creation
```yaml
OpenSearchVectorIndexLambda:
  Type: AWS::Lambda::Function
  Properties:
    Handler: index.handler
    Code:
      ZipFile: |
        def create_vector_index(endpoint, index_name, awsauth):
            index_body = {
                "settings": {
                    "index.knn": True
                },
                "mappings": {
                    "properties": {
                        "vector": {
                            "type": "knn_vector",
                            "dimension": 1536,
                            "method": {
                                "name": "hnsw",
                                "space_type": "l2",
                                "engine": "faiss"
                            }
                        }
                    }
                }
            }
```


#### Limitations and Considerations
- Maximum document size: 5MB
- Supported file formats: PDF, TXT, DOCX
- Vector dimension: 1536 (Titan Embedding Model)
- Maximum concurrent users: Based on Lambda concurrency limits
- API Gateway throttling: 10,000 requests per second

This prototype showcases the integration of various AWS services to create a powerful document interaction system. It's ideal for scenarios requiring intelligent document analysis, Q&A systems, or knowledge base applications.

## 🛠️ Advanced Configuration

### Environment Variables
The installer supports several configuration parameters:

```yaml
Parameters:
  AdminEmail:
    Type: String
    Description: Email address for the admin user
  InitialPassword:
    Type: String
    Description: Initial password for the admin user
    NoEcho: true
```

### Custom Prototype Submission
To submit a new prototype, please provide the following metadata in addition to the CloudFormation template:

<img width="529" alt="format" src="https://github.com/user-attachments/assets/4e97ee18-45e9-4397-a9b2-849332f620a8">


### Custom Domain Configuration
To use a custom domain:

1. Add certificate in ACM
2. Configure API Gateway custom domain
3. Update CloudFront distribution

## 📊 CloudFormation Resource Count

| Resource Type | Count |
|--------------|-------|
| Lambda Functions | 6 |
| IAM Roles | 5 |
| S3 Buckets | 1 |
| API Gateway APIs | 3 |
| Cognito Resources | 3 |
| Custom Resources | 4 |

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
