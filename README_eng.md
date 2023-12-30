# LICO

A chatbot system based on Langchain and LLM.

"LICO" is a humble personal project, aimed at cobbling together various existing technologies into a personal AI assistant. It aims to achieve fully automated information processing, memory retention, reminders, control functionalities, balancing cost and human-machine effectiveness.

If you're interested in related knowledge, feel free to visit my [blog](https://blog.licolico.top). It might just be a case of casting pearls before swine, but hopefully, it might provoke some insightful thoughts.

### Current functionalities supported by the open-source version:

- [x] Connectivity to OpenAI API
- [x] Permanent memory using vector databases
- [x] TTS (Text-to-Speech) voice generation
- [x] A rudimentary web frontend

### Planned functionalities for future releases:

- Yet to be determined

### Personal version task list

- [x] Fuzzy commands
- [x] RSS subscriptions
- [ ] Full voice interaction
- [x] Offline voice wake-up
- [ ] Voiceprint recognition
- [ ] Scene adaptation
- [ ] Music player
- [ ] Self-iterative capabilities
- [ ] Proactive responses
- [ ] Guided hierarchical memory

# Installation and Usage:

> Currently, only AMD64 architecture images are available. However, the source code is provided for compilation. Ensure your Docker can successfully connect to Pinecone and OpenAI services.

This project is packaged as a Docker image file. It's recommended to deploy using Docker Compose.

#### 1. Register with Pinecone vector database and obtain your token

Please search for how to register with Pinecone and obtain a token, which is free.

#### 2. Create a Docker Compose file

Here's an example Docker Compose configuration. ***This configuration file does not require any modifications. If you need to alter variables, please modify the `.env` file.***

```docker-compose
(version: '3'
services:
  web:
    image: licoppppp/licopub:0.1.3-public
    environment:
      - PYTHONUNBUFFERED=1
      - OPENAI_API_KEY=${OPENAI_API_KEY}
      - PINECONE_API_KEY=${PINECONE_API_KEY}
      - PINECONE_ENVIRONMENT=${PINECONE_ENVIRONMENT}
      - VECTOR_SPACE_NAME=${VECTOR_SPACE_NAME}
      - CHATGPT_ACCESS_TOKEN=${CHATGPT_ACCESS_TOKEN}
      - SYSTEM_PROMPT=${SYSTEM_PROMPT}
      - API_BASE=${API_BASE}
      - MODEL_NAME=${MODEL_NAME}
    ports:
      - "8000:8000"
    networks:
      - internal_network  # Ensure service connects to internal network

volumes:
  nginx-config:  # Defines a persistent volume named nginx-config

networks:
  internal_network:  # Defines an internal network named internal_network using the bridge driver
    driver: bridge
```

#### Corresponding `.env` file example (Create an .env file in the same directory as your docker-compose)

Note: This file contains sensitive information. Do not share it with others and use your own valid information. The project and author are not responsible for misuse.

```.env
# This is used for accessing the official embedding/vector generation API. Please use the official OpenAI api_token, related to long-term memory
OPENAI_API_KEY=sk-OhW8l60S2RJrPZ3M0Rw5T3BlbfdsGkdyagyefs3

# This is the Pinecone API KEY. Register with Pinecone, create an environment, then create a vector space. Fill in the name of this vector space below.
PINECONE_API_KEY=59b05a2b-9812-4cb1-8798-899dsfaffgewefeadbca7

# Type of memory bank; keep this default
PINECONE_ENVIRONMENT=gcp-starter

# Name of your memory bank; enter your own memory bank name
VECTOR_SPACE_NAME=your-test-chat-history-space

# This is your OpenAI api_token, usable through third-party endpoints
CHATGPT_ACCESS_TOKEN=sk-hKJHt3378h9ddsfabsafaffdsaf1dAdAcDfD1Fa6cFc6d

# This is the system prompt phrase; you can modify it as desired
SYSTEM_PROMPT="This is a conversation with an AI assistant. The assistant is helpful, creative, clever, and very friendly."

# This is your API endpoint; you can use third-party endpoints. Even if you use the default official endpoint, change it to the official address.
API_BASE=https://openai.114514.sample/v1

# This is your model type; you can modify it yourself. Currently supports gpt3.5 and gpt4. Please use the full official API model name.
MODEL_NAME=gpt-3.5-turbo
```