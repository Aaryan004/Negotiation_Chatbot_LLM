# Bargainix Chatbot

## Overview

The Bargainix Chatbot is a negotiation chatbot built as part of the Bargainix.ai technical assignment. This project demonstrates how to:
- Process raw negotiation dialogues into structured data.
- Generate negotiation responses using an open-source LLM with prompt engineering.
- Deploy the negotiation bot as a web API using FastAPI.

The bot is designed to simulate a witty, tough negotiator for high-demand fashion items.

## Setup and Installation

### Prerequisites
- **Python 3.8 or later**.
- A **virtual environment** to manage dependencies.
- [Optional] **GPU support** for faster inference. If no GPU is available, the model will run on CPU.

### Environment Setup

1. **Create the Project Folder:**
   - Create a folder named `bargainix_chatbot`.

2. **Create Subfolders:**
   - Inside `bargainix_chatbot`, create the following subfolders:
     - `data` (for raw and processed data)
     - `scripts` (for Python scripts)
     - `docs` (for documentation)

3. **Set Up a Virtual Environment:**
   - Open a terminal in the project folder and run:
     ```bash
     cd path/to/bargainix_chatbot
     python -m venv venv
     ```
   - Activate the virtual environment:
     - **Windows:**
       ```bash
       venv\Scripts\activate
       ```
     - **macOS/Linux:**
       ```bash
       source venv/bin/activate
       ```

4. **Install Required Packages:**
   - With the virtual environment activated, run:
     ```bash
     pip install fastapi uvicorn transformers torch pydantic
     ```

## Data Processing

### Raw Data Preparation
- **File Location:** `data/raw_conversation.txt`
- **Example Content:**
- User: Hey, I like this jacket. How much is it? Bot: It's $80. User: Can I get a discount? Bot: Hmm... I can offer $75 if you buy now. User: $70? Bot: Sorry, I can't go below $73. Deal? User: Alright, I'll take it. Bot: Great! Here’s your discount code: DISC10.

Run the Script:
  ```bash
  cd scripts
  python process_data.py
  ```
This creates data/negotiation_data.json with structured conversation data.

## Negotiation Bot
### Overview
  File: scripts/negotiation_bot.py
 
  Purpose: Loads an LLM to generate negotiation responses based on a system prompt. The bot is set to use GPU if available (device=0), otherwise use CPU (device=-1).

### Negotiation Bot Script
- If memory is a concern, a smaller model like EleutherAI/gpt-neo-125M is used instead of EleutherAI/gpt-neo-2.7B.

## FastAPI API Deployment
### Overview
File: scripts/api.py

Purpose: Deploys the negotiation bot as a web API using FastAPI. The API provides a /bargain endpoint to handle POST requests with negotiation details and returns the bot's response.

### Running the API Server
Activate your virtual environment:

  ```bash Windows:
  venv\Scripts\activate
  ```
  ``` bash macOS/Linux:
  source venv/bin/activate
  ```

Navigate to the scripts folder and start the server:

  ```bash
  cd scripts
  python api.py
  ```
You should see log messages indicating the server is running on http://0.0.0.0:8000.

### Testing the API
Swagger UI:

Open your browser and navigate to http://localhost:8000/docs.

Use the interactive interface to test the /bargain endpoint. Enter a sample payload like:

```json
{
  "user_offer": 30,
  "base_price": 50
}
```
Using CURL:

  ```bash
  curl -X POST "http://localhost:8000/bargain" -H "Content-Type: application/json" -d "{\"user_offer\": 30, \"base_price\": 50}"
  ```
Additional Notes

Model Caching:
The Hugging Face Transformers library automatically caches downloaded models. If you restart the script, it will load from the cache (unless you clear it manually).

Memory Considerations:
Large models require significant memory. If you experience memory issues, consider using a smaller model (e.g., EleutherAI/gpt-neo-125M or distilgpt2) by modifying the model parameter in negotiation_bot.py.

GPU vs. CPU:
The negotiation bot script uses GPU (device=0) if available. If you only have a CPU, change device=0 to device=-1.

## Conclusion
This project demonstrates the complete pipeline from processing raw negotiation dialogues to deploying an interactive API using FastAPI. It can be further extended and modified to meet specific needs. For questions or further assistance, refer to this documentation or contact the project maintainer.
