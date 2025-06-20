#  AWS SysOps Automation Agent using LangGraph + OpenAI (Colab Ready)

This project demonstrates how to build an **AI-powered SysOps Agent** using Langgraph, Langchain and OpenAI. The agent performs **AWS operational tasks** using **natural language**, starting with Amazon S3 management — listing and creating buckets.

This system can be easily extended to other AWS SysOps tasks like EC2 management, IAM auditing, CloudWatch monitoring, and more.

---

##  Use Case

System administrators, DevOps, or platform engineers can interact with AWS using plain English. The LLM intelligently routes the request to predefined tools like `list_s3_buckets` or `create_s3_bucket`.

> _"Create a bucket called `my-backup-store`"_  
> _"List all the S3 buckets I have in my AWS account"_

---

##  Features

-  Uses AWS credentials stored in Colab securely via `google.colab.userdata`
-  Understands natural language queries via OpenAI's GPT-3.5
-  LangGraph manages tool invocation flow
-  Implements tool-based AWS automation logic (S3 example)
-  Easily extendable to support EC2, RDS, IAM, CloudWatch, etc.
-  Colab-friendly — runs in a browser

---

##  Tech Stack

- Python 3.x
- [LangChain](https://python.langchain.com/)
- [LangGraph](https://github.com/langchain-ai/langgraph)
- OpenAI GPT-3.5
- Boto3 for AWS
- Google Colab (for secrets handling and execution)

---

##  Setup Instructions (Google Colab)

### 1. Install dependencies & Store credentials

```python
!pip install --quiet boto3 langchain langgraph langchain_openai


### 2. Store credentials
# - AWS_ACCESS_KEY_ID
# - AWS_SECRET_ACCESS_KEY
# - AWS_DEFAULT_REGION
# - OPENAI_API_KEY
```

Sample Tools Implemented
1. List S3 Buckets
```
@tool
def list_s3_buckets() -> str:
    response = s3_client.list_buckets()
    buckets = [bucket["Name"] for bucket in response["Buckets"]]
    return f"S3 Buckets: {', '.join(buckets)}"
```
2. Create a New S3 Bucket
```
@tool
def create_s3_bucket(bucket_name: str) -> str:
    s3_client.create_bucket(Bucket=bucket_name)
    return f"✅ Bucket '{bucket_name}' created successfully!"
```

Extending to Other AWS SysOps Tasks
You can extend this framework any service such as:
| AWS Service    | Possible Actions (Tools)                      |
| -------------- | --------------------------------------------- |
| **EC2**        | Start/stop instances, get status, create AMIs |
| **IAM**        | List users, roles, permissions                |
| **RDS**        | List DBs, backup, restore                     |
| **CloudWatch** | View logs, set alarms                         |
| **S3**         | Upload/download files, set permissions        |
| **SNS/SQS**    | Send messages, subscribe topics               |
