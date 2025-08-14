# Cognito AI – Company Document Q&A Chatbot{RAG AGENT}

This n8n workflow creates an **AI-powered HR / Company Q&A assistant** that answers employee questions based on documents stored in **Google Drive**.

It integrates:
- **Google Drive** – Monitors a specific folder for new or updated documents.
- **Google Gemini** – Generates embeddings for documents and produces conversational responses.
- **Pinecone** – Stores and retrieves vector embeddings for semantic search.
- **n8n AI Agent** – Serves as the conversational interface for employees.

---

## 📌 What This Workflow Does

1. **Folder Monitoring**
   - Watches a specific Google Drive folder for newly created or updated files.

2. **Automatic Document Indexing**
   - On detecting a change:
     - Downloads the new/updated document from Google Drive.
     - Splits the document into chunks using a recursive text splitter.
     - Generates embeddings using **Google Gemini**.
     - Stores the embeddings in a **Pinecone** vector database (`company-files` index).

3. **Intelligent Q&A Chat**
   - Listens for incoming chat messages from employees.
   - Retrieves the most relevant document sections from Pinecone.
   - Uses Google Gemini’s language model to generate a clear, concise answer.
   - If no relevant match is found, replies:  
     > "I cannot find the answer in the available resources."

4. **Real-time Updates**
   - Indexes documents automatically whenever files are updated/added, ensuring the chatbot always has current information.

---

## 🛠 Requirements

Before importing the workflow, ensure you have:

- **Google Cloud Project**
  - Vertex AI API enabled
  - Google Gemini API key from [Google AI Studio](https://aistudio.google.com/)

- **Pinecone Account**
  - API key from your [Pinecone Dashboard](https://www.pinecone.io/)
  - Vector index named `company-files`

- **Google Drive Folder**
  - A dedicated folder to store all company policy and reference documents.

- **n8n Instance**
  - Ability to add credentials for:
    - **Google Drive OAuth2**
    - **Google Gemini API**
    - **Pinecone API**

---

## ⚙️ Setup Instructions

1. **Create API Keys & Accounts**
   - Get your Google Gemini API key.
   - Create a Pinecone index (`company-files`).
   - Create a Google Drive folder for your company documents.

2. **Configure Credentials in n8n**
   - Add **Google Drive OAuth2** credentials.
   - Add **Google Gemini API** credentials (PaLM/Gemini key).
   - Add **Pinecone API** credentials.

3. **Import the Workflow**
   - In n8n, choose **Import From File** and select the provided JSON file.

4. **Update Node Settings**
   - Point the **Google Drive Trigger** nodes to your specific company documents folder.
   - Verify the Pinecone Vector Store nodes are set to use the `company-files` index.

5. **Activate the Workflow**
   - Turn the workflow on in n8n.
   - Upload a test company policy PDF to the monitored Google Drive folder.
   - Send a test chat query.

---

## 📂 Workflow Components

**Triggers**
- **Google Drive File Created / Updated** – Starts the indexing process when new or updated files are detected.

**Processing**
- **Download File From Google Drive** – Gets the file content.
- **Recursive Text Splitter** – Breaks documents into chunks.
- **Google Gemini Embeddings** – Converts text chunks into embeddings.
- **Pinecone Vector Store** – Stores embeddings for semantic search.

**Chat Agent**
- **When chat message received** – Receives employee queries.
- **AI Agent (Google Gemini)** – Answers questions by retrieving data from Pinecone and generating a response.
- **Window Buffer Memory** – Maintains context between chat turns.

---

## 📝 Notes

- The JSON file does **not** contain credentials — you must add them in your own n8n environment.
- Supported input formats depend on the Google Drive loader configuration (PDF, DOCX, TXT, etc.).
- Default bot behavior if answer not found:  
  `"I cannot find the answer in the available resources."`

---

## 🚀 Example Workflow Use Case

> **Scenario:**  
> Your HR team uploads the latest "Remote Work Policy" PDF to Google Drive.  
> Within a minute, the workflow indexes the document.  
> An employee asks:  
> *"How many remote days are allowed per month?"*  
> The chatbot searches the Pinecone index, retrieves the relevant policy section, and answers instantly.


