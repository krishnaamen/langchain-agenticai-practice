# LangChain Learning Journey 🚀

A comprehensive guide to learning **LangChain** from scratch, covering fundamental concepts to building intelligent agents with multiple tools.

> **Status**: Currently at 1 hour 52 minutes of the tutorial series  
> **Last Updated**: June 2026

---

## 📋 Table of Contents

- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Topics Covered](#topics-covered)
- [Prerequisites](#prerequisites)
- [Installation & Setup](#installation--setup)
- [Notebook Breakdown](#notebook-breakdown)
- [Key Concepts](#key-concepts)
- [Getting Started](#getting-started)
- [API Keys Required](#api-keys-required)
- [Project Highlights](#project-highlights)
- [Next Steps](#next-steps)
- [Resources](#resources)
- [License](#license)

---

## 🎯 Overview

This repository contains practical implementations and learnings from following a comprehensive **LangChain tutorial**. The project demonstrates how to build LLM-powered applications, from basic model interactions to creating intelligent agents with multiple tools.

LangChain is a framework that enables developers to:
- Connect Large Language Models (LLMs) to various data sources
- Build tool-equipped agents that can perform multiple tasks
- Create complex workflows and chains for AI applications
- Integrate with multiple LLM providers (OpenAI, Groq, etc.)

---

## 📁 Repository Structure

```
langchain-learning/
│
├── 1-langchainintro.ipynb          # Introduction to LangChain basics
├── 2-tools.ipynb                   # Creating and binding tools
├── 3-genai-inbuilt.ipynb           # Agent framework implementation
├── 4-agentwithmultipletools.ipynb  # Multi-tool agent systems
├── README.md                        # This file
└── .env.example                     # Environment variables template
```

---

## 📚 Topics Covered

### 1. **Introduction to LangChain**
- Framework overview and architecture
- LLM model initialization
- Basic text generation with models
- Understanding LangChain components

### 2. **Working with Tools**
- Creating custom tools using `@tool` decorator
- Tool definition with type hints
- Binding tools to models
- Tool invocation and execution
- Tool execution loops (request → invoke → response)

### 3. **Agent Framework**
- Creating agents with `create_agent()`
- Agent system prompts
- Model and tool binding to agents
- Single tool agent workflows

### 4. **Multi-Tool Agents**
- Building agents with multiple tools
- Integrating external APIs (TavilySearch)
- Creating custom calculation tools
- Tool orchestration and management
- Streaming responses from agents

---

## ✅ Prerequisites

Before starting, ensure you have:

- **Python 3.10+** installed
- **pip** package manager
- Basic understanding of Python programming
- API keys for:
  - Groq (for LLM models)
  - OpenAI (optional, for GPT models)
  - Tavily (for web search functionality)
- Terminal/Command line familiarity

---

## 🔧 Installation & Setup

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/langchain-learning.git
cd langchain-learning
```

### 2. Create Virtual Environment
```bash
# On macOS/Linux
python3 -m venv .venv
source .venv/bin/activate

# On Windows
python -m venv .venv
.venv\Scripts\activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Set Up Environment Variables
```bash
# Copy example file
cp .env.example .env

# Edit .env with your API keys
nano .env  # or use your preferred editor
```

### 5. Required Packages
```bash
pip install langchain
pip install langchain-groq
pip install langchain-tavily
pip install python-dotenv
pip install jupyter
```

---

## 📖 Notebook Breakdown

### **1-langchainintro.ipynb**
**What You'll Learn:**
- Setting up LangChain environment
- Initializing chat models (`init_chat_model`)
- Basic LLM interaction
- Generating text responses
- Understanding AIMessage objects

**Key Code:**
```python
from langchain.chat_models import init_chat_model
model = init_chat_model("groq:llama-3.1-8b-instant")
response = model.invoke("write me an essay on AI")
```

---

### **2-tools.ipynb**
**What You'll Learn:**
- Creating tools with `@tool` decorator
- Tool binding to models
- Tool call detection
- Implementing tool execution loops
- Processing tool results

**Key Concepts:**
- **Tool Definition**: Functions decorated with `@tool` become callable by LLMs
- **Model Binding**: Using `model.bind_tools()` to connect tools to models
- **Tool Calls**: Models recognize when to use tools
- **Execution Loop**: Request → Tool Invocation → Result → Response

**Key Code:**
```python
from langchain.tools import tool

@tool
def get_weather(location: str) -> str:
    """Get the weather at a location"""
    return f"It's a sunny day in {location}"

model_with_tools = model.bind_tools([get_weather])
```

---

### **3-genai-inbuilt.ipynb**
**What You'll Learn:**
- LangChain agent framework (`create_agent`)
- ChatGroq model integration
- Agent initialization with tools
- System prompts for agents
- Building intelligent assistants

**Key Code:**
```python
from langchain_groq import ChatGroq
from langchain.agents import create_agent

model = ChatGroq(model="qwen/qwen3-32b")
agent = create_agent(
    model=model,
    tools=[get_weather],
    system_prompt="You are a helpful assistant"
)
```

---

### **4-agentwithmultipletools.ipynb**
**What You'll Learn:**
- Building agents with multiple tools
- TavilySearch integration for web searches
- Creating calculation tools
- Tool orchestration
- Streaming agent responses
- Handling complex multi-step queries

**Key Features:**
- **TavilySearch**: Web search capability with advanced parameters
- **Calculator Tool**: Custom arithmetic operations
- **Agent Streaming**: Real-time response generation
- **Tool Coordination**: Agent decides which tool to use and when

**Key Code:**
```python
from langchain_tavily import TavilySearch
from langchain.agents import create_agent

tool1 = TavilySearch(max_results=5, topic="general")
calc = Calculator()

agent = create_agent(
    model=model,
    tools=[tool1, calc]
)

# Streaming responses
for step in agent.stream({"messages": user_input}):
    step["messages"][-1].pretty_print()
```

---

## 🧠 Key Concepts

### **1. LLM Models**
Language models that understand and generate text. In this project, we use:
- **Groq Models**: Fast, cost-effective LLMs
- **OpenAI Models**: Advanced GPT models (optional)

### **2. Tools**
Functions that agents can call to perform specific tasks:
- Weather information
- Web searches
- Calculations
- Database queries

### **3. Agents**
Intelligent systems that:
- Receive user queries
- Determine which tools to use
- Execute tools appropriately
- Generate final responses

### **4. Tool Execution Loop**
```
User Input
    ↓
Model Processes & Decides on Tool
    ↓
Tool Executes
    ↓
Result Returned to Model
    ↓
Model Generates Final Response
    ↓
User Receives Output
```

### **5. Streaming**
Real-time response generation for better user experience, showing responses as they're generated rather than waiting for complete execution.

---

## 🚀 Getting Started

### Run First Notebook
```bash
jupyter notebook 1-langchainintro.ipynb
```

### Execute a Simple Agent Query
```python
from langchain_groq import ChatGroq
from langchain.agents import create_agent
from langchain.tools import tool

# Define tool
@tool
def get_weather(location: str) -> str:
    return f"It's sunny in {location}"

# Create model
model = ChatGroq(model="qwen/qwen3-32b")

# Create agent
agent = create_agent(
    model=model,
    tools=[get_weather],
    system_prompt="You are a helpful weather assistant"
)

# Get response
response = agent.invoke({"messages": "What's the weather in Copenhagen?"})
print(response)
```

---

## 🔑 API Keys Required

### **1. Groq API Key**
- Sign up at: https://console.groq.com
- Get your API key from dashboard
- Add to `.env`: `GROQ_API_KEY=your_key_here`

### **2. OpenAI API Key** (Optional)
- Sign up at: https://platform.openai.com
- Navigate to API keys section
- Add to `.env`: `OPENAI_API_KEY=your_key_here`

### **3. Tavily API Key**
- Sign up at: https://tavily.com
- Get your API key
- Add to `.env`: `TAVILY_API_KEY=your_key_here`

### **.env File Example**
```
GROQ_API_KEY=your_groq_key_here
OPENAI_API_KEY=your_openai_key_here
TAVILY_API_KEY=your_tavily_key_here
```

---

## ✨ Project Highlights

| Feature | Status | Notebook |
|---------|--------|----------|
| Basic LLM Integration | ✅ Complete | 1-langchainintro.ipynb |
| Tool Creation & Binding | ✅ Complete | 2-tools.ipynb |
| Agent Framework | ✅ Complete | 3-genai-inbuilt.ipynb |
| Multiple Tools | ✅ Complete | 4-agentwithmultipletools.ipynb |
| Web Search Integration | ✅ Complete | 4-agentwithmultipletools.ipynb |
| Streaming Responses | ✅ Complete | 4-agentwithmultipletools.ipynb |

---

## 🎓 Learning Outcomes

After completing this journey, you will understand:

- ✅ How to initialize and use LLM models
- ✅ How to create custom tools for agents
- ✅ How to build intelligent agents
- ✅ How to orchestrate multiple tools
- ✅ How to integrate external APIs
- ✅ How to stream responses in real-time
- ✅ How to debug and optimize agent behavior

---

## 📝 Next Steps

### Phase 2 (To be added):
- [ ] Memory management in agents
- [ ] Prompt engineering best practices
- [ ] Error handling and retries
- [ ] Persisting conversation history
- [ ] Building custom workflows

### Phase 3 (To be added):
- [ ] RAG (Retrieval-Augmented Generation)
- [ ] Vector databases integration
- [ ] Document processing pipelines
- [ ] Building production-ready applications

---

## 📚 Resources

### Official Documentation
- **LangChain Docs**: https://docs.langchain.com
- **Groq API**: https://console.groq.com/docs
- **Tavily Search**: https://tavily.com/docs

### Learning Resources
- **YouTube Tutorial**: Follow along with your source video
- **LangChain GitHub**: https://github.com/langchain-ai/langchain
- **Community Discussions**: https://github.com/langchain-ai/langchain/discussions

### Related Technologies
- **LangGraph**: Advanced workflow orchestration
- **LlamaIndex**: Data indexing for LLMs
- **OpenAI API**: Premium language models

---

## 🤝 Contributing

This is a personal learning repository, but suggestions and improvements are welcome!

### To contribute:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

---

## 📖 Citation

If you reference this learning project, please cite:

```bibtex
@repository{langchain_learning,
  title={LangChain Learning Journey},
  author={Your Name},
  year={2026},
  url={https://github.com/yourusername/langchain-learning}
}
```

---

## ⚠️ Important Notes

- **API Costs**: Groq offers free tier, but monitor usage
- **Rate Limits**: Be aware of API rate limits
- **API Keys**: Never commit `.env` files to version control
- **Notebook Updates**: LangChain updates frequently; versions may vary

---

## 📧 Support

If you encounter issues:

1. Check the [LangChain Documentation](https://docs.langchain.com)
2. Review the [GitHub Issues](https://github.com/langchain-ai/langchain/issues)
3. Check notebook comments for solutions
4. Verify API keys are correctly set

---

## 📄 License

This learning project is open source and available under the MIT License.

```
MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

---

## 🎉 Acknowledgments

- Thanks to the **LangChain team** for this amazing framework
- Special thanks to the **tutorial creators**
- Community contributions and discussions

---

## 📊 Progress Tracker

| Checkpoint | Status | Date |
|-----------|--------|------|
| Started Learning | ✅ | Jun 2026 |
| Completed Intro | ✅ | Jun 2026 |
| Completed Tools | ✅ | Jun 2026 |
| Completed Agents | ✅ | Jun 2026 |
| Completed Multi-Tool | ✅ | Jun 2026 |
| **Current Progress** | **~92 min of 112 min** | **Jun 22, 2026** |

---

## 🚀 Happy Learning!

> "The best way to learn is by doing. Each notebook is designed to be hands-on and interactive."

Keep exploring, keep learning, and keep building! 🎓

---

**Last Updated**: June 22, 2026  
**Tutorial Progress**: 1 hour 52 minutes / ~2 hours total  
**Repository Status**: 🔄 Active Development
