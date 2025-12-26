# HTU GenAI Assistant 🎓🤖

**Official AI Teaching Assistant for the Generative AI Training Program at Al Hussein Technical University (HTU).**

This project is a production-ready **Retrieval-Augmented Generation (RAG)** system designed to assist students by autonomously answering questions based strictly on the official curriculum. The system features a dual-interface architecture (Web & Telegram) and runs on a self-hosted infrastructure.

![Project Preview](assets/web_ui_preview.png)
*(Customized Web Interface ensuring brand consistency with HTU colors)*

## 🌟 Key Features

* **📚 RAG Architecture:** Connects to a Vector Database (Pinecone) to retrieve precise context from lecture slides and PDFs.
* **🧠 Intelligent Agent:** Uses **GPT-4o-mini** with function calling to distinguish between casual chitchat and technical questions requiring knowledge retrieval.
* **🔄 Automated Ingestion:** A background pipeline watches a Google Drive folder; as soon as a lecturer uploads a new slide, it is processed, embedded, and added to the knowledge base automatically.
* **🎨 Custom Branding:** The n8n native chat interface is heavily customized using CSS to match HTU's visual identity (Red & Navy Blue).
* **🔐 Secure Deployment:** Self-hosted on a VPS and exposed securely to the public web via **Cloudflare Tunnels** (Zero Trust), avoiding open ports.

## 🛠️ Tech Stack

| Component | Technology | Description |
| :--- | :--- | :--- |
| **Orchestration** | [n8n](https://n8n.io/) | Self-hosted workflow automation platform. |
| **LLM** | OpenAI `GPT-4o-mini` | The reasoning engine for the agent. |
| **Embeddings** | `text-embedding-3-small` | For semantic search vectorization. |
| **Vector DB** | Pinecone | Stores the course knowledge base. |
| **Connectivity** | Cloudflare Tunnels | Secure exposure of the local server. |
| **Bot Platform** | Telegram API | Configured via **BotFather**. |

## ⚙️ Architecture & Workflows

The system is composed of three main modular workflows:

### 1. The Ingestion Pipeline
* **Trigger:** New file detected in Google Drive.
* **Process:** PDF Download ➝ Text Extraction ➝ Chunking ➝ Embedding Generation.
* **Output:** Upsert vectors into Pinecone Index.

### 2. The RAG Agent (The Brain)
* **Logic:** Receives user input. The Agent uses a specific tool description to decide:
    * *Is this about the course?* ➔ Call **Retrieval Tool** ➔ Search Pinecone ➔ Answer with context.
    * *Is this a greeting?* ➔ Answer directly.
* **Tool Description used:**
    > *"Useful for when you need to answer questions about the HTU Generative AI Training Program... including AI architectures, training methods, prompt engineering... ALWAYS use this for technical queries."*

### 3. Interfaces (Web & Telegram)
* **Web:** Uses n8n's chat trigger with injected CSS for styling.
* **Telegram:** Listens to webhooks from the bot created via BotFather.

## 📸 Screenshots

| Telegram Bot | Workflow Logic |
| :---: | :---: |
| ![Telegram](assets/telegram_preview.png) | ![n8n](assets/system_diagram.png) |

## 🚀 Setup & Deployment

### Prerequisites
1.  **Self-Hosted n8n Instance.**
2.  **API Keys:** OpenAI, Pinecone, Google Cloud (Drive API).
3.  **Telegram Token:** Get this by talking to **[@BotFather](https://t.me/botfather)** on Telegram and creating a new bot.

### Installation Steps

1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/YourUsername/HTU-GenAI-Assistant.git](https://github.com/YourUsername/HTU-GenAI-Assistant.git)
    ```

2.  **Import Workflows:**
    * Open your n8n dashboard.
    * Import the JSON files located in the `/workflows` folder.

3.  **Configure Credentials:**
    * In n8n, create new credentials for OpenAI, Pinecone, and Telegram.
    * *Note: Credentials are not included in the repo for security.*

4.  **Setup Cloudflare Tunnel:**
    * Run `cloudflared` to tunnel your local port (usually 5678) to a public domain.
    * Example: `cloudflared tunnel run --url http://localhost:5678`

5.  **Apply HTU Branding:**
    * Copy the CSS content from `branding/htu-theme.css`.
    * Paste it into **Settings > Log in & Personalization > Chat Styles** in n8n.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---
*Built with ❤️ by **Ezz El-Din Nazzal** | Data Scientist & AI Engineer*
