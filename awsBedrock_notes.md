Here is the structured markdown summary of the technical concepts and configurations covered in this portion of the video on **Amazon Bedrock Guardrails**:

# Technical Notes: Amazon Bedrock Guardrails Setup

## 1. Prompt Attacks Filter Configuration

* **Purpose:** Detects and blocks user inputs that attempt to override system instructions or inject malicious prompt templates.
* **Input Tagging:** To prevent false positives (misclassifying legitimate system prompts as prompt attacks), use explicit **input tags** in your application code.
* **API Scope Note:**
* Required when using `InvokeModel` or `InvokeModelResponseStream` for text content.
* Not required for the newer `Converse` and `ConverseStream` APIs.


* **Action Settings:** Can be configured to **Block**.
* **Threshold Adjustment:** Slider ranges from **None**, **Low**, **Medium**, to **High** sensitivity.

---

## 2. Content Filters Tier Selection

* **Classic Tier:** An established solution supporting core languages (English, French, Spanish, and regional variants). Optimized for existing workflows requiring cross-region inference compatibility.
* **Standard Tier:** An improved, more robust architecture offering higher accuracy and expanded coverage across more than 50 languages.
