# 🤖 AWS SysOps Automation Agent using LangGraph + OpenAI (Colab Ready)

This project demonstrates how to build an **AI-powered SysOps Agent** using [LangGraph](https://github.com/langchain-ai/langgraph), [OpenAI](https://openai.com), and [LangChain](https://github.com/langchain-ai/langchain). The agent performs **AWS operational tasks** using **natural language**, starting with Amazon S3 management — listing and creating buckets.

This system can be easily extended to other AWS SysOps tasks like EC2 management, IAM auditing, CloudWatch monitoring, and more.

---

## 🧠 Use Case

System administrators, DevOps, or platform engineers can interact with AWS using plain English. The LLM intelligently routes the request to predefined tools like `list_s3_buckets` or `create_s3_bucket`.

> _"Create a bucket called `my-backup-store`"_  
> _"List all the S3 buckets I have in my AWS account"_

---

## ✅ Features

- 🔐 Uses AWS credentials stored in Colab securely via `google.colab.userdata`
- 🧠 Understands natural language queries via OpenAI's GPT-3.5
- 🪢 LangGraph manages tool invocation flow
- ⚙️ Implements tool-based AWS automation logic (S3 example)
- 📈 Easily extendable to support EC2, RDS, IAM, CloudWatch, etc.
- 📦 Colab-friendly — runs in a browser

---

## 🛠️ Tech Stack

- Python 3.x
- [LangChain](https://python.langchain.com/)
- [LangGraph](https://github.com/langchain-ai/langgraph)
- OpenAI GPT-3.5
- Boto3 for AWS
- Google Colab (for secrets handling and execution)

---

## 🔧 Setup Instructions (Google Colab)

### 1. 👩‍💻 Install dependencies

```python
!pip install --quiet boto3 langchain langgraph langchain_openai


from google.colab import userdata

# Store these using `userdata.set(...)` if you're the notebook owner:
# - AWS_ACCESS_KEY_ID
# - AWS_SECRET_ACCESS_KEY
# - AWS_DEFAULT_REGION
# - OPENAI_API_KEY


import os
os.environ["AWS_ACCESS_KEY_ID"] = userdata.get("AWS_ACCESS_KEY_ID")
os.environ["AWS_SECRET_ACCESS_KEY"] = userdata.get("AWS_SECRET_ACCESS_KEY")
os.environ["AWS_DEFAULT_REGION"] = userdata.get("AWS_DEFAULT_REGION")

🧪 Sample Tools Implemented
1. List S3 Buckets
python
Copy
Edit
@tool
def list_s3_buckets() -> str:
    response = s3_client.list_buckets()
    buckets = [bucket["Name"] for bucket in response["Buckets"]]
    return f"S3 Buckets: {', '.join(buckets)}"
2. Create a New S3 Bucket
python
Copy
Edit
@tool
def create_s3_bucket(bucket_name: str) -> str:
    s3_client.create_bucket(Bucket=bucket_name)
    return f"✅ Bucket '{bucket_name}' created successfully!"
🧠 Agent Logic
The LangGraph state machine:

Accepts user message

Adds system prompt context

Routes to tool if needed (via tools_condition)

Executes the right AWS function

Loops until user receives a natural language response

python
Copy
Edit
builder = StateGraph(MessagesState)
builder.add_node("llm_decision_step", function_1)
builder.add_node("tools", ToolNode(tools))
builder.add_edge(START, "llm_decision_step")
builder.add_conditional_edges("llm_decision_step", tools_condition)
builder.add_edge("tools", "llm_decision_step")
💡 Example Prompts
text
Copy
Edit
"Can you list all S3 buckets?"
"Create an S3 bucket named `langgraph-test-18jun`"
🧩 Extending to Other AWS SysOps Tasks
You can extend this framework to:

AWS Service	Possible Actions (Tools)
EC2	Start/stop instances, get status, create AMIs
IAM	List users, roles, permissions
RDS	List DBs, backup, restore
CloudWatch	View logs, set alarms
S3	Upload/download files, set permissions
SNS/SQS	Send messages, subscribe topics

📦 Just wrap Boto3 commands as @tool functions and bind them to LangGraph!

🧱 Architecture Diagram
mermaid
Copy
Edit
graph TD
  A[User Prompt] --> B[LLM Decision Node]
  B -->|Tool Call| C[ToolNode - AWS Operation]
  C --> B
  B --> D[Final LLM Response]
📌 Summary
This project showcases how an LLM-powered AWS SysOps agent can:

Understand user intent in natural language

Take action using AWS APIs via tools

Follow structured execution flow via LangGraph

Run securely within Colab or Python

👨‍💻 Author
Built by [Your Name] using:

LangGraph

OpenAI

Boto3

Google Colab

📄 License
MIT License

yaml
Copy
Edit

---

Let me know if you'd like:
- a prebuilt `.ipynb` version with markdown cells
- additional tools (like EC2 or IAM) added to this README and agent logic
- a hosted version on Streamlit or Gradio instead of Colab
