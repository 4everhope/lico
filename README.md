# LICO

[English Introduction](https://github.com/4everhope/lico/blob/main/README_eng.md)

一个基于Langchain和LLM的聊天机器人系统。

“LICO”是一个没用的自娱自乐的个人项目，旨在拼凑各种现有技术成个人AI助理，实现全自动信息处理、记忆、提醒、控制等功能，兼顾成本和人机功效。

### 开源版目前支持功能：
- [x] 连接到OpenAI API
- [x] 向量数据库永久记忆
- [x] TTS语音生成
- [x] 一个简陋的网页前端

### 后续预计会支持的功能：
- 没想好

# 安装及使用：

> 目前暂时只有AMD64架构镜像，不过这里有源码你可以自行编译。请保证你的docker可以正常连接到Pinecone和Openai的服务。

本项目被打包成了一个Docker镜像文件，推荐使用Docker Compose进行部署。

#### 1. 注册Pinecone向量数据库并获取你的token

请自行搜索如何注册Pingcone并获得token，免费的。

#### 2. 创建Docker Compose文件

这里有一个Docker Compose配置文件的示例。***本配置文件不需要修改任何内容，如果需要修改变量，请修改`.env`文件。***

```
version: '3'
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
      - internal_network  # 确保服务连接到内部网络

volumes:
  nginx-config:  # 定义了一个名为 nginx-config 的持久化卷

networks:
  internal_network:  # 定义了一个名为 internal_network 的内部网络，使用 bridge 驱动
    driver: bridge

```

#### 对应的`.env`文件示例（请创建.env文件并放在你的docker-compose同目录）

注：此文件内包含敏感信息，请勿分享给他人并使用你自己的有效的信息，本项目和作者不为此负责。

```
# 这个是用来使用官方的嵌入/向量生成API，请使用官方的OpenAI api_token，和长期记忆有关
OPENAI_API_KEY=sk-OhW8l60S2RJrPZ3M0Rw5T3BlbfdsGkdyagyefs3

# 这个是Pinecone的API KEY，请自己去注册pinecone的账号，然后创建一个环境，然后创建一个向量空间，然后把这个向量空间的名字填在下面
PINECONE_API_KEY=59b05a2b-9812-4cb1-8798-899dsfaffgewefeadbca7

# 记忆库类型，这里保持默认就好
PINECONE_ENVIRONMENT=gcp-starter

# 记忆库名字，填入你自己的记忆库名字
VECTOR_SPACE_NAME=your-test-chat-history-space

# 这个是你的OpenAI api_token，可以使用第三方的接入点
CHATGPT_ACCESS_TOKEN=sk-hKJHt3378h9ddsfabsafaffdsaf1dAdAcDfD1Fa6cFc6d

# 这个是系统提示词，可以自己改
SYSTEM_PROMPT="This is a conversation with an AI assistant. The assistant is helpful, creative, clever, and very friendly."

# 这个是你的API接入点，可以使用第三方的接入点， 即使你使用默认的官方接入点也需要改成官方的地址
API_BASE=https://openai.114514.sample/v1

# 这个是你的模型类型，可以自己改，目前支持gpt3.5和gpt4，请使用官方API里的模型名称全称。
MODEL_NAME=gpt-3.5-turbo
```
