Here is the structured markdown summary of the technical concepts and configurations covered in this portion of the video on **Amazon Bedrock Guardrails**:

# Technical Notes: Amazon Bedrock Guardrails Setup from the exercise
* prompt filtering
* using gaurd rails
* we can filter topics the model won't talk about

## 1. Prompt Attacks Filter Configuration

* **Purpose:** Detects and blocks user inputs that attempt to override system instructions or inject malicious prompt templates.
* **Input Tagging:** To prevent false positives (misclassifying legitimate system prompts as prompt attacks), use explicit **input tags** in your application code.
- **API Scope Note:**
  - Required when using `InvokeModel` or `InvokeModelResponseStream` for text content.
  - Not required for the newer `Converse` and `ConverseStream` APIs.


* **Action Settings:** Can be configured to **Block**.
* **Threshold Adjustment:** Slider ranges from **None**, **Low**, **Medium**, to **High** sensitivity.

---

## 2. Content Filters Tier Selection

* **Classic Tier:** An established solution supporting core languages (English, French, Spanish, and regional variants). Optimized for existing workflows requiring cross-region inference compatibility.
* **Standard Tier:** An improved, more robust architecture offering higher accuracy and expanded coverage across more than 50 languages.


# Technical Notes: NER, Token-Level Redaction, vs. Embeddings

## 1. Overview of Token-Level Redaction & NER
* **What it is:** A security and data-filtering layer used to intercept inputs (prompts) and outputs (responses) before they interact with or leave a Foundation Model (FM).
* **Why it's needed:** Broad guardrails may miss fine-grained sensitive elements like specific Personal Identifiable Information (PII), proprietary IDs, or forbidden keywords.
* **Role of NER (Named Entity Recognition):** 
  * An NLP technique that scans text and categorizes words based on context (e.g., identifying "Amazon" as an Organization, "London" as a Location, or custom enterprise IDs).
  * Goes beyond simple pattern-matching tools (like Regular Expressions, which only look for rigid formats like credit card strings) by understanding linguistic context.
* **AWS Implementation Example:** Utilizing services like **Amazon Comprehend** in custom pre- or post-processing handlers around inference endpoints (such as Bedrock or SageMaker) to mask tokens dynamically.

---

## 2. Comparison: NER vs. Embeddings

| Feature | Named Entity Recognition (NER) / Redaction | Vector Embeddings |
| :--- | :--- | :--- |
| **Primary Purpose** | Security, censorship, PII compliance, and filtering exact restricted words or entities. | Semantic search, similarity matching, and finding conceptual context (e.g., RAG pipelines). |
| **How It Understands Text** | **Grammatical & Categorical:** Labels words into predefined buckets (e.g., Person, Organization, Location, Custom ID). | **Semantic & Vectorial:** Converts text into numerical coordinates (vectors) in a high-dimensional mathematical space. |
| **Output Type** | Filtered, redacted, or masked plain text strings. | Lists of numbers (vectors) used to measure mathematical distance. |
| **Operational Stage** | Acts as a **gateway filter** *around* the model to block or scrub sensitive data. | Acts as the **representation engine** *inside* systems like vector databases to retrieve relevant knowledge. |


### Prompt Management with Amazon Bedrock

* **Reusable Prompts:** Storing and sharing specialized prompts across multiple applications.
* **Versioning:** Maintaining version history for prompts.
* **Variables & Placeholders:** Inserting dynamic values into prompts using double curly braces (e.g., `Make me a music playlist for {{genre}} music with {{number}} songs`).
* **Structured User Input:** Passing dynamic data directly into applications.

#### Core Workflow

1. **Create Prompt:** Design and save prompt templates.
2. **Test Prompt:** Evaluate model responses inside the console.
3. **Use Prompt:** Integrate and reuse managed prompts across applications.

---

## 3. How They Work Together in an Architecture
* **Embeddings** power the background retrieval system (such as RAG and Vector Stores) to locate documents and information based on overall conceptual meaning.
* **NER and Token-Level Redaction** act as the protective perimeter—ensuring that sensitive data doesn't leak into prompts sent to the model or leak out in the model's responses, regardless of the broader semantic context.


Here is the content formatted in Markdown so you can easily copy and paste it into your notes:

# Amazon Bedrock Prompt Flows

## Core Concepts & Lecture Key Points
* **Stored Prompts Integration:** Stored prompts created and managed in Amazon Bedrock can be directly integrated and used as building blocks within a flow.
* **Prompt & Model Chaining:** Chain multiple prompts, Foundation Models (FMs), and processing stages together to build sophisticated pipelines.
* **Conditional Routing:** Implement condition nodes (e.g., branching based on intent, confidence, or output categories) to route prompts to different models or actions.
* **Pre- & Post-Processing:** Enforce custom pre-processing and post-processing steps directly on input data and model outputs.
* **Visual Graph Interface:** Connect visual nodes—such as Flow Input, Condition, Prompt, Knowledge Base, and Flow Output—using directed data links (e.g., passing string or document types).

---

## Practical Applications & Use Cases

### 1. Retrieval-Augmented Generation (RAG) Pipelines
* **Architecture:** `User Input` → `Knowledge Base Node` → `FM Prompt Node` → `Flow Output`
* **Use Case:** Automatically enrich user queries with relevant enterprise context from vector search/OpenSearch before feeding the prompt to an LLM, ensuring grounded and hallucination-free answers.

* ## Summary: Enforcing Use of Structured Data

### 1. Prompt-Based Enforcement

* **Prompt Instructions:** You can instruct the model directly within the prompt to respond strictly in structured formats (such as JSON) rather than freeform text.
* **Schema Definition:** Clearly specify the required schema and fields (e.g., `reviewId`, `sentiment`, `summary`) within the instructions.
* **Limitations:** Relying purely on prompts can be unpredictable, requiring faith that the model won't output markdown formatting wrappers, omitted fields, or incorrect data types.

---

### 2. Tool Use in Amazon Bedrock (Converse API)

* **Agentic Tool Integration:** Modern foundation models support connecting to external tools to pass structured data back and forth.
* **Simulated Tool Calling:** You can configure a mock tool via the `toolConfig` parameter in Bedrock's Converse API. By providing an `inputSchema` that expects your desired JSON structure and forcing the model to select that tool using `toolChoice`, the model produces data adhering to the schema.
* **Real-World Effectiveness:** In practice, using tool definitions does not necessarily yield better or worse results than specifying the schema directly in the prompt—under the hood, the foundation model may simply be handling both via prompt processing.

---

### Key Exam Takeaway

* For the AWS Certified Generative AI Developer exam, the essential concept is knowing that you can extract and enforce **structured JSON output** from foundation models, both through direct prompt engineering and by configuring tool use schemas in the Bedrock Converse API.

### 2. Multi-Stage Classification & Conditional Routing
* **Architecture:** `Input` → `Classifier Prompt Node` → `Condition Node` → Branch A / Branch B
* **Use Case:** Route incoming customer requests based on classification:
  * Technical questions


  # Lecture 35: Enterprise Integration & Amazon OpenSearch

## 1. Bedrock Knowledge Bases & Enterprise Integration

* **Integration Points**: Serve as the core connection for internal data and document management systems (e.g., [Amazon S3](https://www.udemy.com/course/ultimate-aws-certified-generative-ai-developer-professional/), SharePoint, Atlassian Confluence).
* **Automated Processing**: Bedrock automatically synchronizes and ingests this data into your vector stores.

## 2. Cross-Account Access Architecture

* **Exam Relevance**: A frequent and critical topic for the [AWS Certified Generative AI Developer - Professional](https://www.udemy.com/open-badges/1938088334) exam, particularly when Bedrock models and OpenSearch reside in separate AWS accounts.
* **Remote-Inference Connectors**: OpenSearch uses a remote-inference connector to handle cross-account semantic search capabilities.
* **IAM Role Requirements**:
* Proper permissions must be configured across accounts.
* The Bedrock account requires an IAM role that permits `InvokeModel` access from the OpenSearch account.



---

## 3. Deep Dive: Amazon OpenSearch Service

Amazon OpenSearch Service is a distributed search, analytics, and data-storage engine (originally forked from Elasticsearch and Kibana) that acts as a foundational building block for GenAI and enterprise search on AWS.

### Core Roles in Generative AI

* **Vector Database for RAG**: Acts as a high-scale vector store, indexing high-dimensional vector embeddings from enterprise documents to run approximate nearest neighbor ($k$-NN) vector searches.
* **Semantic & Neural Search**: Matches queries based on context and meaning rather than relying solely on literal keyword (BM25 lexical) matches.
* **Hybrid Search**: Combines vector similarity scores, traditional full-text keyword searches, and metadata filters into a single blended result to improve context retrieval quality for LLMs.
* **OpenSearch Serverless (AOSS)**: Provides a vector search collection type that eliminates the need to manually manage clusters, scale instances, or configure shard sizing.

### Connection to Enterprise Integration

* **Managed Vector Store**: Often serves as the default managed vector store created automatically behind the scenes when setting up a Bedrock Knowledge Base.
* **ML Commons Integration**: Utilizes machine learning frameworks with built-in connectors to call external foundation models (like Amazon Titan Text Embeddings or Cohere on Bedrock) directly during ingestion and query execution.
* **Centralized vs. Workload Accounts**: In large organizations, OpenSearch typically runs in a centralized data or analytics account, while Bedrock models reside in a dedicated AI account. A remote-inference connector bridges them by assuming an IAM role in the Bedrock account to run `bedrock:InvokeModel` dynamically.


# Overview of Sparse One-Hot Embeddings

## What They Are
Sparse one-hot embeddings (commonly known as one-hot encoding) represent categorical data or text as numeric vectors where most values are zero and a single value is one.

## How They Work
Each unique word or category in a vocabulary gets assigned a unique index. A vector is then created with a length equal to the total vocabulary size ($V$). Every position is filled with a `0` except for the index corresponding to the specific word, which is set to `1`.

**Example with a 4-word vocabulary (`["apple", "banana", "cat", "dog"]`):**
* **apple** = `[1, 0, 0, 0]`
* **banana** = `[0, 1, 0, 0]`
* **cat** = `[0, 0, 1, 0]`
* **dog** = `[0, 0, 0, 1]`

## Key Characteristics
* **High Dimensionality:** As vocabulary sizes grow (e.g., 50,000 to 100,000+ words), vectors become extremely long and mostly filled with zeros (sparse).
* **Orthogonality:** Because each vector has its `1` in a distinct position, the dot product between any two different word vectors is always zero.
* **No Semantic Awareness:** They fail to capture semantic relationships or similarities between words (e.g., "cat" and "dog" are mathematically just as distant from each other as "cat" and "airplane").

## Why They Are Not Used for Modern RAG
Modern Retrieval-Augmented Generation (RAG) and vector databases (such as Amazon Bedrock Knowledge Bases) rely on **dense embeddings** (like Amazon Titan). 
* Dense embeddings compress text into much smaller continuous dimensions (e.g., 512, 1024, or 1536 dimensions) where every value is a floating-point number.
* This allows systems to capture semantic meaning and contextual similarity, placing related concepts close to each other in vector space.
