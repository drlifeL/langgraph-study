# LangGraph ReAct Agent Template

[![Version](https://img.shields.io/badge/version-v0.1.0-blue.svg)](https://github.com/webup/langgraph-up-react)
[![LangGraph](https://img.shields.io/badge/LangGraph-v0.6.6-blue.svg)](https://github.com/langchain-ai/langgraph)
[![Build](https://github.com/webup/langgraph-up-react/actions/workflows/unit-tests.yml/badge.svg)](https://github.com/webup/langgraph-up-react/actions/workflows/unit-tests.yml)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![README CN](https://img.shields.io/badge/README-中文-red.svg)](./README_CN.md)
[![DeepWiki](https://img.shields.io/badge/Powered_by-DeepWiki-blue.svg)](https://deepwiki.com/webup/langgraph-up-react)
[![Twitter](https://img.shields.io/twitter/follow/zhanghaili0610?style=social)](https://twitter.com/zhanghaili0610)

This template showcases a [ReAct agent](https://arxiv.org/abs/2210.03629) implemented using [LangGraph](https://github.com/langchain-ai/langgraph), works seamlessly with [LangGraph Studio](https://docs.langchain.com/langgraph-platform/quick-start-studio#use-langgraph-studio). ReAct agents are uncomplicated, prototypical agents that can be flexibly extended to many tools.

![Graph view in LangGraph studio UI](./static/studio_ui.png)

The core logic, defined in `src/react_agent/graph.py`, demonstrates a flexible ReAct agent that iteratively reasons about user queries and executes actions. The template features a modular architecture with shared components in `src/common/`, MCP integration for external documentation sources, and comprehensive testing suite.

**⭐ Star this repo if you find it helpful!** Visit our [webinar series](https://space.bilibili.com/31004924) for tutorials and advanced LangGraph development techniques.

## Features

### Multi-Provider Model Support
- **Qwen Models**: Complete Qwen series support via `langchain-qwq` package, including Qwen-Plus, Qwen-Turbo, QwQ-32B, QvQ-72B
- **OpenAI**: GPT-4o, GPT-4o-mini, etc.
  - **OpenAI-Compatible**: Any provider supporting OpenAI API format via custom API key and base URL
- **Anthropic**: Claude 4 Sonnet, Claude 3.5 Haiku, etc.

### Agent Tool Integration Ecosystem
- **Model Context Protocol (MCP)**: Dynamic external tool loading at runtime
- **DeepWiki MCP Server**: Optional MCP tools for GitHub repository documentation access and Q&A capabilities  
- **Web Search**: Built-in traditional LangChain tools (Tavily) for internet information retrieval

### LangGraph v0.6 Features

> [!NOTE]
> **New in LangGraph v0.6**: [LangGraph Context](https://docs.langchain.com/oss/python/context#context-overview) replaces the traditional `config['configurable']` pattern. Runtime context is now passed to the `context` argument of `invoke/stream`, providing a cleaner and more intuitive way to configure your agents.

- **Context-Driven Configuration**: Runtime context passed via `context` parameter instead of `config['configurable']`
- **Simplified API**: Cleaner interface for passing runtime configuration to your agents
- **Backward Compatibility**: Gradual migration path from the old configuration pattern

### LangGraph Platform Development Support
- **Local Development Server**: Complete LangGraph Platform development environment
- **70+ Test Cases**: Unit, integration, and end-to-end testing coverage with complete DeepWiki tool loading and execution testing
- **ReAct Loop Validation**: Ensures proper tool-model interactions

## What it Does

The ReAct agent:

1. Takes a user **query** as input
2. Reasons about the query and decides on an action
3. Executes the chosen action using available tools
4. Observes the result of the action
5. Repeats steps 2-4 until it can provide a final answer

The agent comes with web search capabilities and optional DeepWiki MCP documentation tools, but can be easily extended with custom tools to suit various use cases.

### Sample Execution Traces

See these LangSmith traces to understand how the agent works in practice:

- **[DeepWiki Documentation Query](https://smith.langchain.com/public/d0594549-7363-46a7-b1a2-d85b55aaa2bd/r)** - Shows agent using DeepWiki MCP tools to query GitHub repository documentation
- **[Web Search Query](https://smith.langchain.com/public/6ce92fd2-c0e4-409b-9ce2-02499ae16800/r)** - Demonstrates Tavily web search integration and reasoning loop

## Getting Started

### Setup with uv (Recommended)

1. Install uv (if not already installed):
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

2. Clone the repository:
```bash
git clone https://github.com/webup/langgraph-up-react.git
cd langgraph-up-react
```

3. Install dependencies (including dev dependencies):
```bash
uv sync --dev
```

4. Copy the example environment file and fill in essential keys:
```bash
cp .env.example .env
```

### Environment Configuration

1. Edit the `.env` file with your API keys:

2. Define required API keys in your `.env` file:

```bash
# Required: Web search functionality
TAVILY_API_KEY=your-tavily-api-key

# Required: If using Qwen models (default)
DASHSCOPE_API_KEY=your-dashscope-api-key

# Optional: OpenAI model service platform keys
OPENAI_API_KEY=your-openai-api-key
# Optional: If using OpenAI-compatible service platforms
OPENAI_API_BASE=your-openai-base-url

# Optional: If using Anthropic models
ANTHROPIC_API_KEY=your-anthropic-api-key

# Optional: Regional API support for Qwen models  
REGION=international  # or 'prc' for China mainland (default)

# Optional: Always enable DeepWiki documentation tools
ENABLE_DEEPWIKI=true
```

The primary [search tool](./src/common/tools.py) uses [Tavily](https://tavily.com/). Create an API key [here](https://app.tavily.com/sign-in).

## Model Configuration

The template uses `qwen:qwen-flash` as the default model, defined in [`src/common/context.py`](./src/common/context.py). You can configure different models in three ways:

1. **Runtime Context** (recommended for programmatic usage)
2. **Environment Variables** 
3. **LangGraph Studio Assistant Configuration**

### API Key Setup by Provider

#### OpenAI
```bash
OPENAI_API_KEY=your-openai-api-key
```
Get your API key: [OpenAI Platform](https://platform.openai.com/api-keys)

#### Anthropic  
```bash
ANTHROPIC_API_KEY=your-anthropic-api-key
```
Get your API key: [Anthropic Console](https://console.anthropic.com/)

#### Qwen Models (Default)
```bash
DASHSCOPE_API_KEY=your-dashscope-api-key
REGION=international  # or 'prc' for China mainland
```
Get your API key: [DashScope Console](https://dashscope.console.aliyun.com/)

#### OpenAI-Compatible Providers
```bash
OPENAI_API_KEY=your-provider-api-key
OPENAI_API_BASE=https://your-provider-api-base-url/v1
```
Supports SiliconFlow, Together AI, Groq, and other OpenAI-compatible APIs.

## How to Customize

### Add New Tools
Extend the agent's capabilities by adding tools in [`src/common/tools.py`](./src/common/tools.py):

```python
async def my_custom_tool(input: str) -> str:
    """Your custom tool implementation."""
    return "Tool result"

# Add to the tools list in get_tools()
```

### Add New MCP Tools
Integrate external MCP servers for additional capabilities:

1. **Configure MCP Server** in [`src/common/mcp.py`](./src/common/mcp.py):
```python
MCP_SERVERS = {
    "deepwiki": {
        "url": "https://mcp.deepwiki.com/mcp",
        "transport": "streamable_http",
    },
    # Example: Context7 for library documentation
    "context7": {
        "url": "https://mcp.context7.com/sse",
        "transport": "sse",
    },
}
```

2. **Add Server Function**:
```python
async def get_context7_tools() -> List[Callable[..., Any]]:
    """Get Context7 documentation tools."""
    return await get_mcp_tools("context7")
```

3. **Enable in Context** - Add context flag and load tools in `get_tools()` function:
```python
# In src/common/tools.py
if context.enable_context7:
    tools.extend(await get_context7_tools())
```

> [!TIP]
> **Context7 Example**: The MCP configuration already includes a commented Context7 server setup. Context7 provides up-to-date library documentation and examples - simply uncomment the configuration and add the context flag to enable it.

### Model Configuration Methods

#### 1. Runtime Context (Recommended)

Use the new LangGraph v0.6 context parameter to configure models at runtime:

```python
from common.context import Context
from react_agent import graph

# Configure model via context
result = await graph.ainvoke(
    {"messages": [("user", "Your question here")]},
    context=Context(model="openai:gpt-4o-mini")
)
```

#### 2. Environment Variables

Set the `MODEL` environment variable in your `.env` file:

```bash
MODEL=anthropic:claude-3.5-haiku
```

#### 3. LangGraph Studio Assistant Configuration

In LangGraph Studio, configure models through [Assistant management](https://docs.langchain.com/langgraph-platform/configuration-cloud#manage-assistants). Create or update assistants with different model configurations for easy switching between setups.

### Supported Model Formats

**Model String Format**: `provider:model-name` (follows LangChain [`init_chat_model`](https://python.langchain.com/api_reference/langchain/chat_models/langchain.chat_models.base.init_chat_model.html#init-chat-model) naming convention)

```python
# OpenAI models
"openai:gpt-4o-mini"
"openai:gpt-4o"

# Qwen models (with regional support)
"qwen:qwen-flash"          # Default model
"qwen:qwen-plus"           # Balanced performance
"qwen:qwq-32b-preview"     # Reasoning model
"qwen:qvq-72b-preview"     # Multimodal reasoning

# Anthropic models
"anthropic:claude-4-sonnet"
"anthropic:claude-3.5-haiku"
```

### Customize Prompts
Update the system prompt in [`src/common/prompts.py`](./src/common/prompts.py) or via the LangGraph Studio interface.

### Modify Agent Logic
Adjust the ReAct loop in [`src/react_agent/graph.py`](./src/react_agent/graph.py):
- Add new graph nodes
- Modify conditional routing logic  
- Add interrupts or human-in-the-loop interactions

### Configuration Options
Runtime configuration is managed in [`src/common/context.py`](./src/common/context.py):
- Model selection
- Search result limits
- Tool toggles

## Development

### Development Server
```bash
make dev        # Start LangGraph development server (uv run langgraph dev --no-browser)
make dev_ui     # Start with LangGraph Studio Web UI in browser
```

### Testing
```bash
make test                    # Run unit and integration tests (default)
make test_unit               # Run unit tests only
make test_integration        # Run integration tests  
make test_e2e               # Run end-to-end tests (requires running server)
make test_all               # Run all test suites
```

### Code Quality
```bash
make lint       # Run linters (ruff + mypy)
make format     # Auto-format code
make lint_tests # Lint test files only
```

### Development Features
- **Hot Reload**: Local changes automatically applied
- **State Editing**: Edit past state and rerun from specific points
- **Thread Management**: Create new threads or continue existing conversations
- **LangSmith Integration**: Detailed tracing and collaboration

## Architecture

The template uses a modular architecture:

- **`src/react_agent/`**: Core agent graph and state management
- **`src/common/`**: Shared components (context, models, tools, prompts, MCP integration)
- **`tests/`**: Comprehensive test suite with fixtures and MCP integration coverage
- **`langgraph.json`**: LangGraph Agent basic configuration settings

Key components:
- **`src/common/mcp.py`**: MCP client management for external documentation sources
- **Dynamic tool loading**: Runtime tool selection based on context configuration
- **Context system**: Centralized configuration with environment variable support

This structure supports multiple agents and easy component reuse across different implementations.

## Development & Community

### Roadmap & Contributing
- 📋 **[ROADMAP.md](./ROADMAP.md)** - Current milestones and future plans
- 🐛 **Issues & PRs Welcome** - Help us improve by [raising issues](https://github.com/webup/langgraph-up-react/issues) or submitting pull requests
- 🤖 **Built with Claude Code** - This template is actively developed using [Claude Code](https://claude.ai/code)

### Getting Involved
We encourage community contributions! Whether it's:
- Reporting bugs or suggesting features
- Adding new tools or model integrations  
- Improving documentation
- Sharing your use cases and templates

Check out our roadmap to see what we're working on next and how you can contribute.

## Learn More

- [LangGraph Documentation](https://github.com/langchain-ai/langgraph) - Framework guides and examples
- [LangSmith](https://smith.langchain.com/) - Tracing and collaboration platform  
- [ReAct Paper](https://arxiv.org/abs/2210.03629) - Original research on reasoning and acting
- [Claude Code](https://claude.ai/code) - AI-powered development environment