# Distributed_RAGChatbot
Distributed RAG Chatbot Backend with FastAPI, RQ, OpenAI, and Qdrant
This project builds a scalable retrieval-augmented generation (RAG) backend using FastAPI, Redis Queue (RQ), OpenAI, and Qdrant. It allows users to submit chat queries asynchronously, processes them via background workers, and generates responses based on PDF content embeddings.

📁 Project Structure
bash
Copy
Edit
.
├── Main.py              # Entry point for FastAPI server
├── Server.py            # Defines FastAPI endpoints
├── queue/
│   ├── connection.py    # Connects to Redis (Valkey)
│   └── worker.py        # Worker that fetches context and generates response
├── Docker_compose.yml   # Launches Redis (Valkey)
├── .env                 # Stores environment variables (e.g., OpenAI API key)
⚙️ Features
✅ FastAPI REST API for chat query submission

✅ Uses Redis Queue (RQ) for async query processing

✅ Embeds and queries PDF chunks via Qdrant vector store

✅ Answers generated using OpenAI GPT-4.1 based only on relevant document chunks

✅ Dockerized Valkey (Redis) for queue handling

✅ Separation of concerns between API server and background worker

📦 Tech Stack
FastAPI – Lightweight async web framework

RQ + Redis (Valkey) – Background job queueing

Qdrant – Vector DB for document similarity search

OpenAI GPT-4.1 – Chat completion model

LangChain – PDF document chunking and vector search

Docker Compose – Containerized Redis (Valkey)

🔧 Setup Instructions
1. Clone the Repository
bash
Copy
Edit
git clone https://github.com/yourusername/rag-chatbot-backend.git
cd rag-chatbot-backend
2. Install Dependencies
bash
Copy
Edit
pip install -r requirements.txt
Example requirements.txt:

txt
Copy
Edit
fastapi
uvicorn
python-dotenv
redis
rq
langchain
langchain-qdrant
langchain-openai
openai
3. Set Up .env File
env
Copy
Edit
OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
4. Start Redis (Valkey) Server via Docker
bash
Copy
Edit
docker-compose up -d
This runs Valkey on port 6379.

💡 Make sure your Qdrant vector store is also running on http://vector-db:6333.

🚀 Run the API Server
bash
Copy
Edit
python Main.py
This starts the FastAPI server at http://localhost:8000.

📥 POST /chat
Submit a query to the backend:

Request:

http
Copy
Edit
POST http://localhost:8000/chat?query=What is event loop?
Response:

json
Copy
Edit
{
  "status": "queued",
  "job_id": "some-job-id-uuid"
}
🧠 Start the Worker
In a separate terminal, run:

bash
Copy
Edit
rq worker --with-scheduler --path . queue
This will start consuming tasks from the Redis queue and process chat queries using the process_query() logic.

🧩 How It Works
User hits /chat endpoint with a message.

Message is queued into Redis using RQ.

A background worker picks up the message and:

Searches vectorized PDF data in Qdrant

Constructs context

Sends context + query to OpenAI GPT-4.1

Returns and prints the result (can be expanded to persist/store)

Response is logged in console (can be extended to return/store via DB or frontend)

📌 Improvements (Suggested)
Add result polling via /result/{job_id} endpoint

Save chat logs in a database (e.g., PostgreSQL)

Add JWT-based authentication to the API

Add Streamlit or React frontend

Deploy with Docker for full-stack RAG serving

 
