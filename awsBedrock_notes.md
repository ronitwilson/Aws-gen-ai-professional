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

---

## 3. How They Work Together in an Architecture
* **Embeddings** power the background retrieval system (such as RAG and Vector Stores) to locate documents and information based on overall conceptual meaning.
* **NER and Token-Level Redaction** act as the protective perimeter—ensuring that sensitive data doesn't leak into prompts sent to the model or leak out in the model's responses, regardless of the broader semantic context.
