# Lecture Summary: Dealing with Unstructured Text (Lecture 38)

## Overview
Raw, unstructured text (such as plain documents or transcripts) often loses its inherent structure, causing generative AI models and RAG pipelines to lose vital context (like headings, sections, metadata, and tables). A key technique to solve this is formatting unstructured data into structured structures like **HTML** before processing.

## Key Concepts & Techniques
* **Why Structure Matters**: 
  * Raw text gets lost in a sea of data. 
  * Generative AI models perform significantly better when given clear structural markers (such as headings, sections, metadata, and tables).

* **Using HTML for Preservation**:
  * One effective trick is to convert unstructured text into **HTML** (or a similar markup format) because most foundation models understand HTML syntax very well.
  * Structural elements like `<h1>`, `<h2>`, `<p>`, and `<table>` naturally preserve boundaries, hierarchy, and tabular relationships.

* **Segmenting Data with Divider Strings**:
  * When chunking data into a vector store, standard chunking can split content awkwardly.
  * You can inject custom divider strings or tags (or use preprocessors) to mark logical boundaries like section and subsection breaks, ensuring the vector database chunks context cleanly.

I will format the provided Amazon Bedrock Data Automation notes into a clean, well-structured Markdown format.

🚀 Amazon Bedrock Data Automation (BDA) — Key Takeaways

🎯 Purpose of BDA

Automatically extracts, parses, and transforms unstructured data (such as documents, images, and forms) into structured JSON formats. No complex, manual template creation is required.

📋 BDA Blueprints for Custom Output

🔹 Basic Fields: Can be explicit (directly extracted from the document text) or implicit/transformed (derived, translated, or calculated based on instructions).
🔹 Table Fields: Designed specifically for extracting row-and-column data found in invoices, financial reports, or structured forms.
🔹 Groups: Used to logically bundle related fields together (e.g., grouping first name, last name, and email under a "User Details" parent structure).
🔹 Custom Types: Lets you build reusable, composite data structures (e.g., defining an "Address" type containing street, city, state, and ZIP code).

⚙️ Blueprint Creation & Management

✍️ Prompt-Based Generation: Define and generate custom blueprints using simple natural language prompts instead of coding rigid extraction schemas.
📦 Pre-existing Blueprints: AWS provides ready-to-use blueprints for standard, common document types (such as passports, driver's licenses, and tax documents) to speed up development.
