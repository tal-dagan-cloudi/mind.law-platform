# AnythingLLM Complete Documentation Summary

**Source:** https://docs.anythingllm.com/
**Version:** 1.9.1
**Last Crawled:** December 11, 2025

This document contains comprehensive knowledge from the AnythingLLM official documentation for development reference on the Mind.Law platform (forked from AnythingLLM).

---

## Table of Contents

1. [What is AnythingLLM?](#what-is-anythingllm)
2. [Installation Options](#installation-options)
3. [Core Features](#core-features)
4. [Workspaces](#workspaces)
5. [AI Agents](#ai-agents)
6. [MCP (Model Context Protocol) Integration](#mcp-model-context-protocol-integration)
7. [Agent Flows (No-Code Agent Builder)](#agent-flows-no-code-agent-builder)
8. [Large Language Models (LLMs)](#large-language-models-llms)
9. [Vector Databases](#vector-databases)
10. [Embedding Models](#embedding-models)
11. [Chat Modes](#chat-modes)
12. [Embedded Chat Widgets](#embedded-chat-widgets)
13. [Security & Access Control](#security--access-control)
14. [Customization & White-labeling](#customization--white-labeling)
15. [API Access](#api-access)
16. [Custom Agent Skills Development](#custom-agent-skills-development)
17. [Best Practices](#best-practices)

---

## What is AnythingLLM?

AnythingLLM is an **all-in-one AI application** that can do RAG (Retrieval-Augmented Generation), AI Agents, and much more with **no code or infrastructure headaches**.

### Key Characteristics:
- **Zero-setup** and **private** AI application
- Supports local LLMs, RAG, and AI Agents all in one place
- Built by Mintplex Labs, Inc (YCombinator Summer 2022)
- Open-source project with active community

### Why Use AnythingLLM?

**For Individual Users:**
- Zero-setup, private, all-in-one AI application
- Local LLM support without developer setup
- Complete privacy - everything stays local

**For Organizations:**
- Fully-customizable, private ChatGPT alternative
- Multi-user support with permissions
- Any LLM, embedding model, or vector database
- White-labeling capabilities

---

## Installation Options

### Desktop vs Docker Comparison

| Feature | Desktop | Docker |
|---------|---------|--------|
| Multi-user support | ❌ | ✅ |
| Embeddable chat widgets | ❌ | ✅ |
| One-click install | ✅ | ❌ |
| Private documents | ✅ | ✅ |
| Vector database flexibility | ✅ | ✅ |
| Any LLM support | ✅ | ✅ |
| Built-in embedding provider | ✅ | ✅ |
| Built-in LLM provider | ✅ | ❌ |
| White-labeling | ❌ | ✅ |
| Password protection | ❌ | ✅ |
| User management | ❌ | ✅ |
| Workspace access management | ❌ | ✅ |

### Desktop Installation

**Use Desktop if:**
- You want one-click installable app
- No multi-user support needed
- Everything stays only on your device
- No public internet publishing needed

**Platforms:** macOS, Windows, Linux

### Docker Installation

**Use Docker if:**
- Server-based service needed
- Multiple simultaneous users required
- User invitations and sharing needed
- Admin and role-based access control needed
- Publishing chat widgets to public internet
- Browser-based access required

**Quick Start:**

```bash
# Pull latest image
docker pull mintplexlabs/anythingllm:latest

# Run with persistent storage (REQUIRED for data persistence)
export STORAGE_LOCATION=$HOME/anythingllm
mkdir -p $STORAGE_LOCATION
touch "$STORAGE_LOCATION/.env"

docker run -d -p 3001:3001 \
  --cap-add SYS_ADMIN \
  -v ${STORAGE_LOCATION}:/app/server/storage \
  -v ${STORAGE_LOCATION}/.env:/app/server/.env \
  -e STORAGE_DIR="/app/server/storage" \
  mintplexlabs/anythingllm
```

**Access:** http://localhost:3001

**Important Docker Notes:**
- Must mount `/app/server/storage` volume or data will be lost on restart
- Use `host.docker.internal` to access host services (Ollama, Chroma, etc.) from container
- Requires Docker 18.03+ (Win/Mac) or 20.10+ (Linux)
- On Linux, add `--add-host=host.docker.internal:host-gateway`

---

## Core Features

### Document Management
- **Supported formats:** PDF, TXT, DOCX, XLSX, HTML, MD, EPUB, and more
- **Web scraping** with Puppeteer
- **YouTube transcript** extraction
- **Image OCR** with Tesseract.js
- **Audio/video transcription**
- **Drag-n-drop** interface
- **Citation tracking** - clear source references

### Chat Capabilities
- **RAG (Retrieval-Augmented Generation)** with uploaded documents
- **Multi-modal support** (text + images) with compatible LLMs
- **Streaming responses**
- **Chat history** export (CSV, JSON, Alpaca, JSONL for OpenAI fine-tune)
- **System prompt variables** for dynamic prompts
- **Text-to-speech** (Native browser, PiperTTS, OpenAI TTS, ElevenLabs)
- **Speech-to-text** (Native browser, OpenAI Whisper)

### Workspace Management
- **Workspaces** are isolated conversation contexts (like threads)
- Each workspace can have different:
  - Documents attached
  - LLM configuration
  - Embedding settings
  - Chat mode (Query vs Chat)
  - System prompts
  - Agent configuration
- Documents can be shared across workspaces
- Conversations are always isolated per workspace

---

## Workspaces

### Concept
Workspaces function like threads with containerization:
- Each workspace is an isolated conversation context
- Documents attached to workspace are available for RAG
- Documents can be shared across workspaces
- Conversations don't leak between workspaces
- Keeps context clean per workspace

### Per-Workspace Configuration
- **LLM Provider & Model** - Different LLM per workspace
- **Chat Mode** - Query or Chat mode
- **Document Similarity Threshold** - Control RAG strictness
- **Agent Settings** - Enable/disable agents and skills
- **System Prompt** - Customize behavior per workspace
- **Vector Database Settings** - Advanced RAG configuration

---

## AI Agents

### What are AI Agents?

AI Agents are LLMs with access to tools that can:
- Browse the web
- Scrape websites
- Run database queries
- Save files
- Generate charts
- Summarize documents
- Access documents in workspace
- Use MCP tools
- Use custom agent skills

### Agent Setup

1. **Choose LLM for Agent** - Configure in workspace settings
2. **Select Agent Skills** - Enable/disable available tools
3. **Configure Search Provider** (optional) - For web browsing
   - SerpApi, SearchApi, Google, Serper, Bing Search, Serply

**Important:** Not all LLM models work well as agents. Higher quantization models (e.g., 8-bit vs 4-bit) perform better.

### Agent Usage

**Start agent session:**
```
@agent [your question]
```

**End agent session:**
```
/exit
```

**Note:** After initial `@agent` invocation, you don't need to keep mentioning @agent in the same session.

### Built-in Agent Tools

#### 1. RAG Search
- Checks embedded documents in workspace
- Updates agent's memory for recall
- Example: `@agent can you check what you already know about AnythingLLM?`

#### 2. Web Browsing
- Search internet for information
- Requires search provider configuration
- Example: `@agent can you do a web search for "latest AI news"?`

#### 3. Web Scraping
- Scrapes website content
- Automatically embeds content into workspace
- Example: `@agent can you scrape anythingllm.com and summarize features?`

#### 4. Save Files
- Saves information to local file system
- Prompts user for file location
- Example: `@agent can you save this as PDF on my desktop?`

#### 5. List Documents
- Shows all documents accessible in workspace
- Example: `@agent could you tell me the list of files you can access?`

#### 6. Summarize Documents
- Provides document summaries
- Example: `@agent can you summarize the content on [URL]?`

#### 7. Chart Generation
- Creates charts from data or formulas
- Example: `@agent can you plot y=mx+b where m=10 and b=0?`

#### 8. SQL Agent
- Run queries against relational databases
- **Caution:** Use read-only database user
- Available operations:
  - `list-databases` - View connections
  - `list-tables` - View tables in database
  - `check-table-schema` - View table columns
  - `query` - Run SELECT queries
- Example: `@agent can you summarize sales for May 2024 in backend-office DB?`

### Agent FAQs

**Q: How do I know if agent session started/ended?**
- Started: `Agent @agent invoked` log
- Ended: `Agent session completed` log

**Q: Agent says it cannot access internet?**
- LLM model may not be suitable for tool-calling
- Try higher quantization model or larger model
- Smaller 4-bit models struggle with tool-calling

**Q: Agent says it saved file but file doesn't exist?**
- Agent hallucinated - didn't actually call the tool
- Check for `@agent is attempting to call save-file-to-browser` message
- Use more explicit instructions
- Use better tool-calling capable model

---

## MCP (Model Context Protocol) Integration

### What is MCP?

MCP is an **open-source protocol developed by Anthropic** to enable seamless integration between LLM applications and external data sources and tools.

### Benefits
- Standardized way to connect LLMs with external context
- Many pre-built MCP tools available
- Works with any MCP-compatible tool
- Extends agent capabilities beyond built-in tools

### Configuration

MCP servers are configured via `anythingllm_mcp_servers.json` file in the `plugins` directory of AnythingLLM storage.

**Example configuration:**

```json
{
  "mcpServers": {
    "face-generator": {
      "command": "npx",
      "args": ["@dasheck0/face-generator"],
      "env": {
        "MY_ENV_VAR": "my-env-var-value"
      }
    },
    "mcp-youtube": {
      "command": "uvx",
      "args": ["mcp-youtube"]
    },
    "postgres-http": {
      "type": "streamable",
      "url": "http://localhost:3003",
      "headers": {
        "X-API-KEY": "api-key"
      }
    }
  }
}
```

### Supported Transport Types

1. **StdIO** (default) - Text-based protocol, requires `command` field
2. **SSE** (Server-Sent Events) - Streaming responses, requires `url` field
3. **Streamable** - Alternative streaming transport, requires `url` field

### MCP Management UI

Features in AnythingLLM UI:
- Reload/restart all MCP servers
- View MCP server status
- View error logs
- Stop/start servers on the fly
- View available tools from loaded servers
- Delete MCP servers (removes from config)

### Auto-start Prevention

AnythingLLM-specific feature to prevent automatic startup:

```json
{
  "mcpServers": {
    "face-generator": {
      "command": "npx",
      "args": ["@dasheck0/face-generator"],
      "anythingllm": {
        "autoStart": false
      }
    }
  }
}
```

**Note:** Servers without `autoStart: false` start automatically.

---

## Agent Flows (No-Code Agent Builder)

### What are Agent Flows?

Agent Flows are a **no-code way to build agentic skills** using a visual interface.

### Agent Flows vs Agent Skills

| Aspect | Agent Flows | Agent Skills |
|--------|-------------|--------------|
| Approach | No-code visual builder | Code-based development |
| Target Audience | Everyone | Power users & developers |
| Complexity | Simpler | More flexible |
| End Result | Same functionality | Same functionality |

### How to Use

Agent flows work **exactly the same as agent skills**:
- Use `@agent` directive to invoke
- Ask relevant questions in agentic chat
- LLM can chain multiple flows together
- LLM can call series of flows in sequence

### Capabilities

Limited only by:
- Your imagination
- Available tools
- LLM reasoning capability

**Note:** Desktop version has more tools available than Docker version.

---

## Large Language Models (LLMs)

### Multi-Modal Support

Models supporting both text-to-text and image-to-text are fully supported for:
- System-level chat
- Workspace-level chat

### Local LLM Providers

1. **Built-in (default)** - AnythingLLM Desktop only
2. **Ollama** - Popular local LLM runner
3. **LM Studio** - Desktop application for local LLMs
4. **LocalAI** - OpenAI-compatible local API

### Cloud LLM Providers

**Major Providers:**
- OpenAI
- Azure OpenAI
- AWS Bedrock
- Anthropic (Claude)
- Google Gemini Pro
- Cohere
- Mistral API
- Groq

**Aggregator Services:**
- OpenRouter - Access to multiple LLMs
- Together AI
- Fireworks AI
- Perplexity AI
- Hugging Face Inference
- OpenAI (generic) - For OpenAI-compatible APIs

**Other Providers:**
- NVIDIA NIM
- DeepSeek
- xAI
- Z.AI
- Novita AI
- PPIO
- Gitee AI
- Moonshot AI
- Microsoft Foundry Local
- CometAPI
- KoboldCPP
- LiteLLM
- Text Generation Web UI
- Apipie

### Configuration

Each workspace can have different LLM configuration:
- Provider selection
- Model selection
- Temperature settings
- Token limits
- Context window size

**Best Practice:** Use higher capability models for agents (tool-calling support).

---

## Vector Databases

### Default: LanceDB (Built-in)

**Benefits:**
- No external dependencies
- Vectors never leave AnythingLLM
- Zero configuration required
- Fast and efficient

### Local Vector Database Providers

1. **LanceDB** (built-in, default)
2. **PGVector** - PostgreSQL extension
3. **Chroma** - Can run locally or cloud
4. **Milvus** - Self-hosted option

### Cloud Vector Database Providers

1. **Pinecone** - Fully managed cloud
2. **Zilliz** - Cloud version of Milvus
3. **AstraDB** - DataStax managed service
4. **QDrant** - Cloud or self-hosted
5. **Weaviate** - Cloud or self-hosted

### Configuration

Vector database is configured globally at system level:
- All workspaces use same vector database
- Configuration stored in `system_settings` table
- Requires restart after changing

---

## Embedding Models

### What are Embeddings?

Embedding models convert text into vectors for:
- Storage in vector databases
- Semantic similarity search
- Foundation of RAG functionality

### Default: Built-in Embedder

**Model:** `sentence-transformers/all-MiniLM-L6-v2` from Hugging Face
- Runs locally via `@xenova/transformers`
- No external dependencies
- Privacy-preserving
- Downloads on first use (~500MB)

### Local Embedding Providers

1. **Built-in** (default) - sentence-transformers
2. **Ollama** - Local embedding models
3. **LM Studio** - Desktop embedding
4. **LocalAI** - Self-hosted OpenAI-compatible

### Cloud Embedding Providers

1. **OpenAI** - ada-002 and newer
2. **Azure OpenAI** - Same as OpenAI but via Azure
3. **Cohere** - embed-english and multilingual

### Configuration

Embedding provider configured globally:
- All workspaces use same embedder
- Configuration stored in `system_settings`
- Changing requires re-embedding documents

---

## Chat Modes

### Query Mode

**Behavior:**
- **Only uses information from uploaded documents**
- Will explicitly say when information not found
- More accurate and document-focused
- Best for fact-checking and document Q&A

**Use Cases:**
- Technical documentation queries
- Factual information from documents
- Preventing hallucinations/made-up info
- Compliance and legal documents

### Chat Mode

**Behavior:**
- **Uses both documents AND LLM's general knowledge**
- More conversational and flexible
- Can provide context beyond documents
- Good for brainstorming

**Use Cases:**
- Conversational interactions
- Brainstorming and ideation
- Exploring topics broadly
- Getting additional context

### Troubleshooting "No Relevant Information Found"

**Common Causes:**
1. Information worded differently in document
2. Similarity threshold too strict
3. Document chunking issues

**Quick Fixes:**
1. Go to Workspace Settings → Vector Database Settings
2. Change "Document similarity threshold" to "No restriction"
3. Rephrase question using document terminology

**Threshold Levels:**
- **No restriction** - Returns all chunks
- **Low (≥ 0.25)** - Very permissive
- **Medium (≥ 0.50)** - Balanced (default)
- **High (≥ 0.75)** - Very strict

### Best Practices

1. **Start with Query mode** + "No restriction" if not finding information
2. **Use specific terms** from documents in questions
3. **Switch to Chat mode** for broader context
4. **Rephrase questions** if not getting good results
5. **Match document terminology** instead of colloquial terms

---

## Embedded Chat Widgets

**⚠️ DOCKER VERSION ONLY**

### What are Chat Widgets?

Embeddable chat interfaces that can be integrated into any website using a simple `<script>` tag.

### Configuration Options

#### 1. Workspace Selection
- Determines which workspace the chat is based on
- Inherits defaults from selected workspace

#### 2. Allowed Chat Method
- **Chat** - Responds to all questions regardless of context
- **Query** - Only responds to questions related to workspace documents

#### 3. Domain Restrictions
- Block requests from non-whitelisted domains
- Empty = anyone can use widget on any site
- Security feature for public widgets

#### 4. Rate Limiting
- **Max Chats per Day** - Global limit (0 = unlimited)
- **Max Chats per Session** - Per-user limit (0 = unlimited)

#### 5. Dynamic Configuration
- **Dynamic Model Use** - Override workspace LLM
- **Dynamic LLM Temperature** - Override temperature
- **Prompt Override** - Override system prompt

### Implementation

After configuration, AnythingLLM provides:
- Embeddable `<script>` tag
- Unique widget identifier
- Configuration API (if dynamic features enabled)

---

## Security & Access Control

**⚠️ DOCKER VERSION ONLY**

### Single-User Mode

**Best for:**
- Personal use
- Small trusted groups
- No permission requirements

**Features:**
- Optional password protection
- Full instance control for all users
- No per-user permissions
- All users see all chats

**Password Protection:**
- Toggle on/off anytime
- Reset password while logged in
- Simple instance-level authentication

### Multi-User Mode

**Best for:**
- Organizations
- Teams with different access levels
- Workspace-level permissions needed

**⚠️ Warning:** Cannot revert to single-user mode once enabled

#### User Roles

1. **Admin**
   - Full system access
   - User management
   - System configuration
   - Access to all workspaces
   - View logs and analytics

2. **Manager**
   - View all workspaces
   - Manage workspace properties
   - **Cannot** change LLM, Embedder, or Vector DB settings
   - User invitations

3. **Default**
   - Only access explicitly assigned workspaces
   - Cannot see/edit system settings
   - Cannot see other workspaces
   - Basic chat functionality

#### Workspace-Level Permissions

- Admins can assign users to specific workspaces
- Users only see workspaces they're assigned to
- Managers can assign workspace access
- Default users require explicit workspace assignment

#### Setup Process

1. Toggle "Enable multi-user mode"
2. Create first admin account (username + password)
3. Log out and log in with new credentials
4. Invite additional users via Admin panel

---

## Customization & White-labeling

**⚠️ DOCKER VERSION ONLY**

### Custom Logo

- Replace AnythingLLM logo throughout app
- Appears on login page and interface
- Upload custom brand logo
- Supports common image formats

### Custom Welcome Messages

- Customize messages shown before workspace selection
- Can simulate system and user messages
- Use to:
  - Explain workspace purposes
  - Onboard new users
  - Provide instructions
  - Brand the experience

### Custom Footer Links and Icons

- Replace default footer icons
- Add custom links to resources
- Point to company documentation
- Link to support pages
- Add social media links

### Use Cases

- **Internal tools** - Brand as company AI assistant
- **Client-facing** - White-label for customers
- **Product integration** - Match product branding
- **SaaS applications** - Multi-tenant branding

---

## API Access

### Developer API

**Full REST API available at:** `/api/docs` on your instance

**Features:**
- Workspace management
- Document management
- Chat endpoints (streaming and non-streaming)
- User management (multi-user mode)
- System configuration
- Embed widget management

### API Keys

**Management:**
- Create/delete API keys on the fly
- Permission-based key creation
- Keys persist across restarts

**Security:**
- API keys grant full access
- **Do not share or publish keys**
- Treat like passwords
- Rotate keys periodically

### Use Cases

- Custom integrations
- Automated workflows
- Third-party tools
- Mobile applications
- Webhook integrations
- Batch processing

### Swagger Documentation

Interactive API documentation available when server running:
```
http://localhost:3001/api/docs
```

Generate/update Swagger docs:
```bash
cd server
yarn swagger
```

---

## Custom Agent Skills Development

**⚠️ Warning:** Only run custom agent skills you trust! They execute with full NodeJS capabilities.

### Prerequisites

- NodeJS 18+
- Yarn
- AnythingLLM in Docker (v1.2.2+) or Desktop (v1.6.5+)
- **Not available in Cloud offering**

### Development Guidelines

1. **Language:** Written in JavaScript (NodeJS environment)
2. **Dependencies:** Bundle all NodeJS packages in skill folder
3. **Return type:** Must return string value
4. **Documentation:** Include `README.md` with description and usage
5. **Configuration:** Define `plugin.json` at root
6. **Entry point:** Define `handler.js` as entry point
7. **Folder structure:** Wrap in folder matching `hubId` from `plugin.json`

### File Structure

```
plugins/agent-skills/my-custom-agent-skill/
├── plugin.json         # Configuration and metadata
├── handler.js          # Entry point with exported function
├── README.md           # Documentation
└── [additional files]  # Any other code/dependencies
```

### plugin.json Example

```json
{
  "name": "My Custom Agent Skill",
  "hubId": "my-custom-agent-skill",
  "description": "Does something cool",
  "version": "1.0.0",
  "setup_args": {
    "API_KEY": {
      "type": "string",
      "required": true,
      "input": {
        "type": "text",
        "default": "",
        "placeholder": "Enter your API key",
        "hint": "Get your API key from service.com"
      }
    }
  }
}
```

### handler.js Example

```javascript
module.exports.runtime = {
  name: "my-custom-skill",
  description: "Performs a custom action",
  inputs: {
    query: {
      type: "string",
      description: "The search query",
      required: true
    }
  }
};

module.exports.handler = async function({ query }, config) {
  try {
    // Your custom logic here
    const result = await doSomething(query);
    return JSON.stringify(result);
  } catch (error) {
    return `Error: ${error.message}`;
  }
};
```

### Storage Locations

**Docker:**
```
{STORAGE_LOCATION}/plugins/agent-skills/
```

**Desktop:**
Find storage directory first, then:
```
{STORAGE_DIR}/plugins/agent-skills/
```

**Local Development:**
```
server/storage/plugins/agent-skills/
```

### Hot Loading

- Changes take effect without restart
- Must `/exit` current agent session for changes
- Reload page for new skills to appear in UI
- Automatic detection and loading

### Dynamic UI Configuration

Skills can have configurable inputs via `plugin.json`:
- Text inputs
- Number inputs
- Boolean toggles
- Select dropdowns
- API keys
- Configuration values

Users configure these in UI before enabling skill.

### Built-in Utilities

Available helper functions:
- `introspect()` - Log agent thoughts
- `logger()` - Console logging
- Access to full NodeJS capabilities

### Best Practices

1. **Error handling** - Always catch and return error messages as strings
2. **Validation** - Validate all inputs
3. **Documentation** - Comprehensive README
4. **Testing** - Test thoroughly before production
5. **Security** - Never trust external data
6. **Return format** - Always return strings

---

## Best Practices

### Document Management

1. **Chunk size matters** - Adjust text splitting for your document types
2. **Test retrieval** - Use Query mode to test document retrieval
3. **Organize by topic** - Create separate workspaces for different topics
4. **Monitor embedding costs** - Cloud embedders charge per token

### Workspace Organization

1. **Purpose-specific workspaces** - One workspace per use case
2. **Share common docs** - Attach frequently used docs to multiple workspaces
3. **Clear naming** - Use descriptive workspace names
4. **Test with sample questions** - Verify RAG quality before relying on workspace

### Agent Usage

1. **Choose right model** - Higher capability models for agents
2. **Enable only needed skills** - Reduces confusion for LLM
3. **Be specific** - Explicit tool instructions work better
4. **Test individually** - Test each skill separately
5. **Monitor costs** - Agent iterations can be expensive with cloud LLMs

### Performance

1. **Local embeddings** - Built-in embedder is fast and private
2. **LanceDB default** - Zero setup, good performance
3. **Similarity threshold** - Start loose, then tighten
4. **Context window** - Larger context = better but slower

### Security

1. **API keys** - Rotate regularly, never commit to code
2. **Widget domains** - Whitelist domains for public widgets
3. **Rate limiting** - Always set limits on public widgets
4. **Read-only DB** - SQL agent should use read-only user
5. **Multi-user roles** - Use appropriate roles for team members

### Development

1. **Use hot reload** - Custom skills reload automatically
2. **Test locally** - Always test in development before production
3. **Version control** - Track `plugin.json` changes
4. **API documentation** - Reference `/api/docs` for integration
5. **Error logs** - Check logs when troubleshooting

### Cost Optimization

1. **Local LLMs** - Use Ollama/LM Studio when possible
2. **Smaller models** - For simple tasks, smaller models suffice
3. **Cache responses** - Reuse answers when appropriate
4. **Batch processing** - Group operations when using API
5. **Monitor usage** - Track token consumption

---

## Agent Flow Blocks

### Default Blocks (Every Flow Has These)

#### 1. Flow Information Block
- **Purpose:** Defines flow name and description
- **Critical:** LLM uses this to understand when to use the flow
- **Best Practices:**
  - Use descriptive but concise name
  - Include usage examples in description
  - Define variable requirements
  - Explain limitations

#### 2. Flow Variables Block
- **Purpose:** Define input/output variables for the flow
- **Variable Configuration:**
  - Name: Descriptive identifier (e.g., `HackerNewsPostsPath`)
  - Default Value: Optional fallback value
  - LLM can override default values at runtime
- **JSON Object Traversal:** Use dot notation to access nested data
  - Example: `apiResponse.data.users[0].name`
  - Works with API responses, nested structures
  - Enables chaining of multiple API calls

#### 3. Flow Complete Block
- **Purpose:** Visual end marker for flow
- Purely cosmetic, no functional impact

### Available Functional Blocks

#### Web Scraper Block
**Capabilities:**
- Scrapes website and extracts text content (not HTML)
- Returns parsed text for processing

**Inputs:**
- `URL to scrape` - Target website URL
- `Result Variable` - Variable to store scraped content

**Use Cases:**
- Extracting article content
- Gathering web data for analysis
- Real-time information retrieval

**Example:**
```
URL: https://news.ycombinator.com/${hackerNewsURLPath}
Result Variable: pageContentFromSite
```

#### API Call Block
**Capabilities:**
- Make HTTP requests to any API
- Supports all HTTP methods (GET, POST, PUT, DELETE, etc.)
- Dynamic headers, body, and URL via variables

**Inputs:**
- `URL` - API endpoint
- `Method` - HTTP method
- `Headers` - Request headers (supports variables)
- `Body` - Request body for POST (JSON, raw text, form data)
- `Result Variable` - Variable to store API response

**Variable Injection in POST Body:**
```json
{
  "variableProperty": "${variableName}",
  "staticProperty": "staticValue",
  "${variableName}": "staticValue"
}
```

**Use Cases:**
- Third-party API integration
- Data fetching
- Webhook calls
- Service orchestration

#### LLM Instruction Block
**Capabilities:**
- Send instructions to workspace's configured LLM
- Process data, extract information, transform content
- Most flexible and powerful block

**Inputs:**
- `Instructions` - Prompt for LLM (supports variables)
- `Result Variable` - Variable to store LLM output

**Important Notes:**
- Uses workspace agent's configured LLM
- Quality depends on model capability
- Smaller/quantized models may struggle
- More detailed prompts = better results

**Variable Usage in Instructions:**
```
Extract all links from this content that would be relevant to: ${topicOfInterest}
Content:
${pageContentFromSite}
Format your response as a list of markdown links.
```

**Best Practices:**
- Be explicit and detailed in instructions
- Provide format examples
- Handle edge cases (e.g., "If no results, say X")
- Use higher capability models for complex tasks

#### Read File Block
**⚠️ Desktop Only (v1.8.1+)**

**Capabilities:**
- Read text files from local file system
- Only supports text file types

**Inputs:**
- `File Path` - Path to file (supports variables)
- `Result Variable` - Variable to store file content

**Dynamic Path Usage:**
```
/home/user/documents/${fileName}
```

**Use Cases:**
- Processing local files
- Reading configuration files
- Importing data from disk

#### Write File Block
**⚠️ Desktop Only (v1.8.1+)**

**Capabilities:**
- Write content to local file system
- Only supports text output

**Inputs:**
- `File Path` - Destination path (supports variables)
- `Content` - Content to write

**Dynamic Path Usage:**
```
/home/user/output/${reportName}.txt
```

**Use Cases:**
- Exporting flow results
- Logging information
- Saving processed data
- Passing data to other applications

### Flow Tutorial: HackerNews Filter

**Goal:** Scrape HackerNews and filter posts by topic

**Flow Structure:**

1. **Flow Info:**
   - Name: "Hacker News Headline Viewer"
   - Description: Explain available options (front page, newest), provide usage examples

2. **Variables:**
   - `hackerNewsURLPath` - Page to scrape (empty = front page, "newest" = new posts)
   - `topicOfInterest` - Filter topic (default: "Political discussions")
   - `pageContentFromSite` - Stores scraped content

3. **Web Scraper Block:**
   - URL: `https://news.ycombinator.com/${hackerNewsURLPath}`
   - Result: `pageContentFromSite`

4. **LLM Instruction Block:**
   - Instructions: Extract links relevant to `${topicOfInterest}` from `${pageContentFromSite}`
   - Format as markdown links with descriptions

**Usage Examples:**
- "Find AI-related posts on HackerNews"
- "Show me political discussions from newest HackerNews posts"
- "What are the latest cryptocurrency articles on HackerNews?"

---

## MCP Integration Details

### MCP on Desktop

**Availability:** v1.8.0+ required

**Key Points:**
- Supports `Tools` only (no Resources, Prompts, or Sampling)
- Commands must be installed on host machine
- Must be in PATH or use full binary path
- Required commands: `npx`, `uv`, `uvx`, `node`, `bash`

**Startup Behavior:**
- MCP servers NOT started on app boot
- Auto-start when opening "Agent Skills" page
- Auto-start when invoking `@agent`
- Subsequent starts are faster (servers already running)

**Configuration File Location:**
```
{STORAGE_DIR}/plugins/anythingllm_mcp_servers.json
```

**Desktop Storage Locations:**
- **macOS:** `/Users/<usr>/Library/Application Support/anythingllm-desktop/storage`
- **Linux:** `~/.config/anythingllm-desktop/storage/`
- **Windows:** `C:\Users\<usr>\AppData\Roaming\anythingllm-desktop\storage`

**Storage Structure:**
- `lancedb/` - Local vector database tables
- `documents/` - Parsed document content
- `vector-cache/` - Cached embeddings (hashed filenames)
- `models/` - Locally stored LLMs/embedders (GGUF files)
- `anythingllm.db` - SQLite database
- `plugins/` - Custom agent skills and MCP config

**Management:**
- **Reload:** Click "Refresh" in Agent Skills page
- **Start/Stop:** Use gear icon on individual servers
- **Delete:** Removes from config file and kills process
- **Debug:** Check desktop application logs

**File I/O:**
- Can read/write any path on host machine
- Use normal file system paths
- No containerization restrictions

**Troubleshooting:**
- Check application logs for MCP errors
- Ensure commands are in PATH
- Manually install tools via `uv tool install xyz`
- Reload servers after config changes

### MCP on Docker

**Availability:** Self-hosting only (not Cloud)

**Key Points:**
- Base image: `ubuntu:jammy-20240627.1`
- Pre-installed commands: `npx`, `uv`, `uvx`, `node`, `bash`
- Supports generic Ubuntu commands

**Startup Behavior:**
- Same as Desktop (not auto-started on boot)
- Auto-start on "Agent Skills" page or `@agent` invocation

**Configuration File Location:**
```
{STORAGE_LOCATION}/plugins/anythingllm_mcp_servers.json
```

**File I/O:**
- Must use `/app/server/storage/...` path prefix
- Maps to host's `STORAGE_LOCATION` directory
- Required for host machine file access

**Tool Persistence:**
- Tools downloaded inside container
- Lost on container deletion
- Need to re-download on container rebuild
- Manual installations also lost

**Management:**
- **Reload:** Click "Refresh" (no container restart needed)
- **Start/Stop:** Use gear icon (immediate effect)
- **Delete:** Removes from config and kills process
- **Debug:** Check Docker container logs

**Manual Tool Installation:**
```bash
# Shell into container
docker exec -it <container> bash

# Install tool
uv tool install xyz

# Refresh in UI
```

**Troubleshooting:**
- Check Docker container logs
- Ensure sufficient container resources
- Verify tool availability in container
- Test with `docker exec` commands

---

## System Prompt Variables

### What are System Prompt Variables?

Inject **dynamic** and **static** variables into workspace system prompts for personalization and context.

### Default Variables

| Variable | Description | Availability |
|----------|-------------|--------------|
| `{date}` | Current date | All versions |
| `{time}` | Current time | All versions |
| `{datetime}` | Current date and time | All versions |
| `{user.name}` | User's name | Docker multi-user only |
| `{user.bio}` | User's bio field | Docker multi-user only |
| `{os.name}` | Operating system name | Desktop only |
| `{os.arch}` | OS architecture | Desktop only |
| `{workspace.name}` | Workspace name | All versions |
| `{workspace.id}` | Workspace ID | All versions |

**Note:** Time-based variables use machine time (important for Docker deployments).

### Custom Variables

- Create static custom variables via UI
- Click "Add Variable" on System Prompt Variables page
- Static values that don't change per request

### Usage

**In Workspace System Prompt:**
```
You are a helpful assistant.
Today is {date} and the current time is {time}.
The user's name is {user.name}, they work at {company_name}.
About the user: {user.bio}
Current workspace: {workspace.name}
```

**Expanded Example:**
```
You are a helpful assistant.
Today is 2024-01-01 and the current time is 12:00:00.
The user's name is John Doe, they work at Google.
About the user: Rock climbing enthusiast obsessed with AI workflows.
Current workspace: Legal Research
```

**Validation:**
- Invalid variables simply won't expand
- Valid variables highlighted in blue in UI
- No error messages for invalid variables

---

## Docker Images and Deployment

### Available Docker Images

#### `latest` (Recommended for most users)
- **Architectures:** amd64 & arm64
- **Update frequency:** Daily (on every commit to master)
- **Pull:** `docker pull mintplexlabs/anythingllm:latest`
- **Best for:** Always getting latest features and fixes

#### `v*.*.*` (Version-pinned releases)
- **Architectures:** amd64 & arm64
- **Update frequency:** On new releases
- **Pull:** `docker pull mintplexlabs/anythingllm:v1.9.1`
- **Best for:** Stability, controlled updates

#### `render` or `railway` (Platform-specific)
- **Architecture:** amd64 only
- **Update frequency:** On new releases
- **Pull:** `docker pull mintplexlabs/anythingllm:render`
- **⚠️ Warning:** Only use for Render/Railway deployments

#### `pg` (PostgreSQL-optimized)
- **Architectures:** amd64 & arm64
- **Update frequency:** On new releases
- **Pull:** `docker pull mintplexlabs/anythingllm:pg`
- **Features:**
  - Uses PostgreSQL instead of SQLite
  - Defaults to PGVector for vector storage
  - Requires PGVector extension installed
  - Better for horizontal scaling

**PostgreSQL Startup Command:**
```bash
export STORAGE_LOCATION=$HOME/anythingllm
mkdir -p $STORAGE_LOCATION
touch "$STORAGE_LOCATION/.env"

docker run -d -p 3001:3001 \
  --cap-add SYS_ADMIN \
  -v ${STORAGE_LOCATION}:/app/server/storage \
  -v ${STORAGE_LOCATION}/.env:/app/server/.env \
  -e STORAGE_DIR="/app/server/storage" \
  -e DATABASE_URL="postgresql://user:password@172.17.0.1:5432/dbname" \
  mintplexlabs/anythingllm:pg
```

### Cloud Deployment

**Recommended Minimum Specs:**
- **AWS:** t3.small or larger
- **GCP:** e2-standard-2 or larger
- **Azure:** B2ps v2 or larger
- **RAM:** 2GB minimum
- **Storage:** 10GB+ recommended

**One-Click Deployment Templates:**
- Railway: https://railway.app/template/HNSCS1
- Render: https://render.com/deploy?repo=https://github.com/Mintplex-Labs/anything-llm&branch=render

**Cloud Deployment Command:**
```bash
# Pull latest image
docker pull mintplexlabs/anythingllm:master

# Setup storage
export STORAGE_LOCATION="/var/lib/anythingllm"
mkdir -p $STORAGE_LOCATION
touch "$STORAGE_LOCATION/.env"

# Run container
docker run -d -p 3001:3001 \
  --cap-add SYS_ADMIN \
  -v ${STORAGE_LOCATION}:/app/server/storage \
  -v ${STORAGE_LOCATION}/.env:/app/server/.env \
  -e STORAGE_DIR="/app/server/storage" \
  mintplexlabs/anythingllm:master
```

**Important Notes:**
- `--cap-add SYS_ADMIN` required for web scraping (Puppeteer/Chromium)
- Backwards compatibility guaranteed (Mintplex Labs commitment)
- For SSL/HTTPS, use reverse proxy (NGINX + Let's Encrypt)

**Scaling Considerations:**
- SQLite not recommended for horizontal scaling
- Use PostgreSQL (`pg` image) for multi-container deployments
- Centralizes database, enables PGVector

### NGINX Reverse Proxy Configuration

**For SSL/HTTPS support with WebSocket:**

```nginx
# HTTP to HTTPS redirect
server {
  listen 80;
  server_name your-domain.com;
  return 301 https://your-domain.com$request_uri;
}

# HTTPS server with WebSocket support
server {
  listen 443 ssl;
  ssl on;
  server_name your-domain.com;
  ssl_certificate /etc/letsencrypt/live/your-domain.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/your-domain.com/privkey.pem;

  # WebSocket support for agent invocations
  location ~* ^/api/agent-invocation/(.*) {
    proxy_pass http://localhost:3001;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
  }

  # Main application
  location / {
    proxy_connect_timeout 605;
    proxy_send_timeout 605;
    proxy_read_timeout 605;
    send_timeout 605;
    keepalive_timeout 605;
    proxy_buffering off;
    proxy_cache off;
    proxy_pass http://localhost:3001$request_uri;
  }
}
```

### Localhost Connection Issues

**Problem:** Using `localhost`, `127.0.0.1`, or `0.0.0.0` in Docker doesn't work

**Why:** These addresses refer to container network, not host machine

**Solution - Windows/macOS:**
```
http://localhost:11434 → http://host.docker.internal:11434
```

**Solution - Linux:**
```
http://localhost:11434 → http://172.17.0.1:11434
```

**Applies to:**
- Ollama connections
- LM Studio connections
- Chroma vector database
- PostgreSQL database
- Any host-based service

---

## Desktop Installation Details

### macOS Installation

**Two Methods:**

#### Method 1: DMG Installer
- **Intel Macs:** Download `AnythingLLMDesktop.dmg`
- **Apple Silicon (M1/M2/M3):** Download `AnythingLLMDesktop-Silicon.dmg`
- **Performance Note:** M-Series chips run local LLMs significantly faster

**Installation Steps:**
1. Download appropriate DMG
2. Browser may mark as "untrusted" - click "Keep"
3. Open DMG file
4. Drag AnythingLLM to Applications folder
5. Launch via Applications or Cmd+Spacebar search

#### Method 2: Homebrew
```bash
brew install --cask anythingllm
```

### Windows Installation

**Requirements:**
- Windows 10+ (Home or Professional)
- Target OS: Windows 11
- ⚠️ Enterprise/Server versions may not work

**Architectures:**
- x86 64-bit: `AnythingLLMDesktop.exe`
- ARM 64-bit: `AnythingLLMDesktop-Arm64.exe`

**Installation Steps:**
1. Download EXE installer
2. Browser may show warning - click "Keep"
3. **SmartScreen Warning (pre-v1.9.0):**
   - Click "more details"
   - Click "Run anyway"
   - v1.9.0+ is digitally signed (no warning)
4. Run installer
5. **GPU/NPU Support:**
   - Automatic dependency installation during setup
   - Required for NVIDIA/AMD GPU or NPU acceleration
   - Fallback to CPU if dependencies fail
   - Manual install guide available if automatic fails

### Linux Installation

**Supported Architectures:**
- x64 (x86_64)
- ARM64 (v1.9.0+)

**Automated Install Script:**
```bash
# Download installer
curl -fsSL https://cdn.anythingllm.com/latest/installer.sh -o installer.sh

# Make executable
chmod +x installer.sh

# Run installer
./installer.sh
```

**What the Installer Does:**
1. Downloads appropriate AppImage for architecture
2. Creates Ubuntu `apparmor` rule (required for SUID)
3. Creates `.desktop` file for launcher integration
4. Installs to `$HOME` directory by default

**Running the App:**
- Via desktop UI (launcher)
- Via command line: `./AnythingLLMDesktop.AppImage`

**Uninstallation:**
```bash
rm installer.sh
rm AnythingLLMDesktop.AppImage
rm ~/.local/share/applications/anythingllmdesktop.desktop
sudo rm /etc/apparmor.d/anythingllmdesktop
rm -rf ~/.config/anythingllm-desktop
```

**Linux Features (v1.9.0+):**
- Bundled Ollama support (x64 and ARM64)
- No separate Ollama installation needed
- Increased AppImage size but zero setup

### System Requirements

**Minimum Recommended:**
- **RAM:** 2GB
- **CPU:** 2-core (any architecture)
- **Storage:** 5GB

**Scaling Factors:**

**LLM Selection:**
- **Cloud LLMs** (OpenAI, Claude, etc.) - Zero overhead
- **Local LLMs** - Requires GPU for good performance
- **Tip:** Host local LLM on separate GPU machine, connect via API

**Embedder Selection:**
- **Cloud embedders** - Zero overhead
- **Local embedders** - Can run on separate GPU machine

**Vector Database:**
- All supported databases scale to millions of vectors
- LanceDB default handles anything at minimum specs

---

## Desktop-Specific Features

### Built-in Local LLM (Ollama Integration)

**Desktop Includes:**
- Bundled Ollama integration
- No additional installation needed (v1.9.0+ Linux)
- GPU/NPU acceleration support (Windows/Mac)

**Windows GPU Support:**
- Automatic dependency download during install
- Supports NVIDIA GPU, AMD GPU, NPU
- Manual install available if automatic fails
- Falls back to CPU if dependencies missing

**Model Management:**
- Download models via Ollama interface
- Models stored in `models/` directory
- Automatic model context window detection

### Microsoft Foundry Local Integration (v1.9.0+)

**⚠️ Beta Preview (Windows/macOS only)**

**Features:**
- Auto-starts Foundry Local when AnythingLLM starts
- Auto-unloads models to free system resources
- Can pull optimized models for hardware (CPU/GPU/NPU)

**Requirements:**
- Microsoft Foundry Local installed
- Available for Apple Silicon, Windows (x64/ARM64), Linux (x64/ARM64)
- Free to use

**Limitations:**
- Model selection shows only downloaded models
- Model pulling still via `foundry cli`

**Download:** https://github.com/microsoft/Foundry-Local/releases

### Debug Mode

**macOS:**
```bash
cd ~/Applications/AnythingLLM/Content/MacOS
./AnythingLLMDesktop
```

**Windows:**
```cmd
C:\Users\<usr>\AppData\Local\Programs\anythingllm-desktop\AnythingLLMDesktop.exe
```

**Linux:**
```bash
~/.config/anythingllm-desktop/AnythingLLMDesktop.AppImage
```

**Purpose:**
- View verbose error messages
- Monitor application logs in real-time
- Debug MCP server issues
- Troubleshoot startup problems

**Logs Location:**
```
{STORAGE_DIR}/logs/
```

---

## Troubleshooting Guide

### Ollama Connection Issues

**Common Problem:** Cannot connect to Ollama

**Desktop & Docker Auto-detection:**
AnythingLLM automatically tries:
- `http://127.0.0.1:11434`
- `http://host.docker.internal:11434`
- `http://172.17.0.1:11434`

**Step 1: Verify Ollama is Running**
1. Open browser: http://127.0.0.1:11434
2. Should see: "Ollama is running"
3. If not, run: `ollama serve`

**Note:** `ollama run model-name` does NOT start the server!

**Step 2: Fix Docker Connection (if using Docker)**

**Windows/macOS:**
```
http://localhost:11434 → http://host.docker.internal:11434
```

**Linux:**
```
http://localhost:11434 → http://172.17.0.1:11434
```

**Step 3: Remote Ollama**
- Use IP address of machine running Ollama
- Ensure firewall allows connections
- Verify Ollama is accessible from network

**AnythingLLM Cloud + Local Ollama:**
- **Not supported** without SSH tunneling
- Requires exposing local machine to internet (not recommended)
- Security risk - not officially supported

### "Fetch Failed" on Document Upload

**Cause 1: Blocked HuggingFace/AWS Downloads**

**Symptoms:**
- Using default embedder model
- Firewall blocking HuggingFace or AWS

**Fix:**
1. Check for `models/Xenova` folder in storage directory
2. If missing, unblock domains:
   - `huggingface.co`
   - `api.huggingface.co`
   - `https://cdn.anythingllm.com/support/models/`
3. Retry embedding

**Cause 2: Missing Visual C++ Redistributable (Windows)**

**Symptoms:**
- Windows only
- Using default embedder

**Fix:**
1. Download Visual C++ Redistributable v14.x
2. Install it
3. Retry embedding

**Cause 3: Unsupported CPU**

**Symptoms:**
- Using default LanceDB vector database
- CPU doesn't support AVX2

**Fix:**
1. Use machine with AVX2-compatible CPU
2. Or switch to different vector database provider

### Agent Not Using Tools

**Understanding the Issue:**

Agents work by generating JSON tool calls. Problems arise when:
- Model can't generate valid JSON
- Model refuses to call tools (alignment)
- Model doesn't understand tool schemas

**Common Solutions:**

**1. Model Hallucinating Tool Calls**
- **Symptom:** Response claims tool used, but no "thought" output shown
- **Fix:** Use higher quantization or larger model, `/reset` and retry

**2. Model Refuses to Call Tool**
- **Symptom:** "I cannot do that" responses
- **Fix:** Less restricted model, `/reset` and retry, disable unused tools

**3. Model Not Detecting Tools**
- **Symptom:** No tool calls attempted at all
- **Cause:** Poor JSON generation, context overload, quantization issues
- **Fix:**
  - Higher quantization or larger model
  - `/reset` chat history
  - Disable unused tools (reduces prompt size)
  - Be more explicit in requests

**Model Recommendations:**
- **Best:** Cloud models (OpenAI, Claude, Gemini)
- **Good:** Large local models (70B+) with 8-bit quantization
- **Problematic:** Small models (<13B) with 4-bit quantization

### Event Logs & Monitoring

**Available Events:**
- User login attempts (success/failure)
- Messages sent by users
- Settings changes
- Document uploads
- System events

**Event Details:**
- Event type
- Associated user (if applicable)
- Timestamp
- Additional event-specific details

**Use Cases:**
- Monitor instance activity
- Security auditing
- Usage analytics
- Troubleshooting user issues

---

## Privacy & Data Handling

### Anonymous Telemetry

**What's Collected:**
- Installation type (Docker or Desktop)
- Document add/remove events (no content)
- Vector database type
- LLM provider and model tag
- Chat send events (no content)
- No IP or identifying information

**Telemetry Provider:** PostHog (open-source)

**Opt-Out:**
- Set `DISABLE_TELEMETRY=true` in server `.env`
- Or via UI: Sidebar → Privacy → Disable Telemetry

**Verification:**
- Search codebase for `Telemetry.sendTelemetry` calls
- Events written to output logs
- Full transparency

### Data Privacy

**Local Storage:**
- All data stays on your machine/server
- No external data sharing by default
- Cloud providers only get what you send them

**Multi-User Privacy:**
- Users only see assigned workspaces
- Role-based access control
- Chat logs separated by user/workspace

---

## Transcription Models

### Audio/Video Transcription Support

**Local Provider:**
- **Built-in (Xenova)** - Uses Whisper model from HuggingFace
- Runs locally via `@xenova/transformers`
- Privacy-preserving
- Default option

**Cloud Provider:**
- **OpenAI Whisper API** - Cloud-based transcription
- Requires OpenAI API key
- Higher accuracy for complex audio

**Use Cases:**
- Transcribe audio files for document upload
- Video content extraction
- Meeting recordings
- Podcast transcripts

---

## Latest Features & Changelog

### v1.9.1 (Latest - December 9, 2025)

**Major Improvements:**
- Windows installer optimization (faster install time)
- MCP support improvements and bug fixes
- Chat input persistence between workspaces
- Realtime YouTube video scraping via `@agent`
- Ollama bumped to v0.13.0

**New Features:**
- Paperless NGX data connector
- Agent workspace system prompt variables
- Eval_duration from Ollama for accurate TPS
- SerpAPI web search agent provider
- AWS Bedrock API key connection method
- ZAI LLM provider support
- Anthropic prompt caching
- Global default prompt for new workspaces
- Base64 document attachment for chat API
- SSL bypass for local Confluence
- GiteeAI LLM provider
- OpenRouter embedder support
- Ollama batch embedding support

**Bug Fixes:**
- Ollama/LMStudio model caching issues
- MCP panel scrolling issues
- Relevance score display for various vector DBs
- EPub upload failures
- Gemini thinking output hanging
- GitLab loader infinite loop
- Chroma cloud payload size limitations

### v1.9.0 (October 13, 2025)

**Major Features:**
- **@agent Streaming Overhaul**
  - Real-time tool call and response streaming
  - All providers, all models, any hardware
  - Agents can download/ingest web files in real-time (PDFs, Excel, CSV)

**Microsoft Foundry Local Integration:**
- Deep integration with Microsoft Foundry Local
- Auto-start/stop on AnythingLLM boot
- Automatic model unloading for resource management
- Hardware-optimized model pulling (CPU/GPU/NPU)

**Linux Improvements:**
- ARM64 support
- Bundled Ollama (0.11.4) in AppImage
- Improved installation guide
- Auto-created apparmor rules (Ubuntu)
- Auto-created .desktop files (GNOME)

**Other Improvements:**
- Workspace/thread tooltips
- Resize chat area on paste
- Web scraper handles URLs without protocol
- Ollama/LMStudio auto context window detection
- Live HTML rendering in chat
- New system prompt variables (workspace.name, workspace.id)
- CometAPI integration
- Portuguese translations

### v1.8.0 (September 2025)

**Major Features:**
- **MCP Agent Skills** now available in Desktop
- Fresh new landing page
- Hundreds of UI consistency updates
- Japanese translations
- Azure AI model map updates

**Bug Fixes:**
- MSSQL connection string parser
- Agent Flow description usage
- Gemini model list expiration
- Failed tool call loops
- Git HTTPS URL data connector 404s

---

## Plugin.json Complete Reference

### Required Properties

```json
{
  "active": true,                    // Enable/disable skill
  "hubId": "my-skill",              // MUST match folder name
  "name": "My Skill",               // Human-readable name for UI
  "schema": "skill-1.0.0",          // Required - do not change
  "version": "1.0.0",               // Your skill version
  "description": "What it does",    // Required - brief description
  "imported": true                   // Must be true
}
```

### Optional Properties

```json
{
  "author": "@username",             // Creator tag
  "author_url": "https://...",      // Creator URL
  "license": "MIT"                   // License type
}
```

### Setup Arguments (Dynamic UI)

**Purpose:** Create configurable inputs in AnythingLLM UI

```json
{
  "setup_args": {
    "API_KEY": {
      "type": "string",              // string, number, boolean
      "required": false,             // Is field required?
      "input": {
        "type": "text",              // UI input type
        "default": "YOUR_API_KEY",   // Default value
        "placeholder": "sk-1234",    // Placeholder text
        "hint": "Get key from..."    // Help text
      },
      "value": ""                    // Optional preset value
    }
  }
}
```

**Supported Input Types:**
- `text` - Text input field
- `number` - Number input field
- `checkbox` - Boolean toggle
- `select` - Dropdown selection

### Examples Array (Few-Shot Prompting)

**Purpose:** Help LLM understand when and how to use skill

```json
{
  "examples": [
    {
      "prompt": "What is the weather in Tokyo?",
      "call": "{\"latitude\": 35.6895, \"longitude\": 139.6917}"
    },
    {
      "prompt": "What is the weather in London?",
      "call": "{\"latitude\": 51.5074, \"longitude\": -0.1278}"
    }
  ]
}
```

**Best Practices:**
- Provide 1-3 relevant examples
- Match `call` format to handler parameters
- Use diverse, realistic examples
- Helps LLM understand use cases

### Entrypoint Configuration

**Purpose:** Define handler file and expected parameters

```json
{
  "entrypoint": {
    "file": "handler.js",           // Entry point file
    "params": {
      "latitude": {
        "description": "Latitude of location",
        "type": "string"            // string, number, boolean
      },
      "longitude": {
        "description": "Longitude of location",
        "type": "string"
      }
    }
  }
}
```

**Parameter Types:**
- `string` - Text values
- `number` - Numeric values
- `boolean` - True/false values

---

## Advanced Agent Topics

### Why Agents Don't Use Tools

**Root Cause:** Models generate JSON for tool calls - many models struggle with this.

**Technical Flow:**
1. User sends prompt
2. LLM receives prompt + tool definitions
3. LLM generates JSON tool call
4. System validates and executes tool
5. LLM receives result and generates response

**Failure Points:**
- **Invalid JSON** - Syntax errors, wrong format
- **Alignment refusal** - Model trained to refuse certain tools
- **Context overload** - Too many tools in prompt
- **Quantization issues** - Lower precision affects JSON generation

**Model Performance Factors:**
- **Cloud models:** Best (un-quantized, high capability)
- **Large local models (70B+, 8-bit):** Good
- **Medium models (13B-30B, 8-bit):** Fair
- **Small models (<13B, 4-bit):** Poor

**Tool Call Indicators:**
- **Actual call:** "Thought" output visible in UI
- **Hallucination:** Response about tool use, but no "thought"

**Optimization Strategies:**
1. Use cloud LLM for agents, local for regular chat
2. Higher quantization (8-bit > 4-bit)
3. Disable unused tools (reduces prompt size)
4. `/reset` chat history (can impact JSON generation)
5. More explicit tool instructions
6. Test tool-calling capability before deployment

### Agent Streaming (v1.9.0+)

**New Capabilities:**
- Real-time streaming of tool calls
- Real-time streaming of responses
- Works with all providers and models
- Visual feedback during agent operations

**Real-time File Ingestion:**
- Agent can download and process web files on-the-fly
- Supports PDFs, Excel, CSV, etc.
- No manual document upload needed
- Example: "Analyze this PDF: [URL]"

---

## Community & Support

### Official Resources

- **Official Docs:** https://docs.anythingllm.com/
- **GitHub Repository:** https://github.com/Mintplex-Labs/anything-llm
- **Discord Server:** https://discord.gg/6UyHPeGZAC
- **MCP Servers:** https://github.com/modelcontextprotocol/servers
- **Download:** https://anythingllm.com/download
- **Cloud Hosting:** https://anythingllm.com/pricing

### Getting Help

**For AnythingLLM Issues:**
- GitHub Issues for bugs
- Discord for community support
- Documentation for how-to guides

**For MCP Issues:**
- MCP Discussion Board: https://github.com/orgs/modelcontextprotocol/discussions
- **Do NOT** open AnythingLLM GitHub issues for MCP tool problems

**For Tool-Calling Issues:**
- Check model capability first
- Review agent troubleshooting guide
- Test with known-good model (GPT-4, Claude)

### Contributing

AnythingLLM is open-source and accepts contributions:
- See CONTRIBUTING.md in repository
- Active community of volunteers
- YCombinator-backed project

---

## Roadmap (Q3-Q4 2024)

### External Services
- ✅ AnythingLLM Community Hub (sharing plugins, prompts, workspaces)
- 🔄 Private AnythingLLM Hub

### All Platforms
- ✅ Custom AI Agent skills via plugins
- 🔄 Custom Data connectors via plugins
- 🔄 Image Generation (Stable Diffusion, DALL-E)
- 🔄 Simplified Token Tracking SDK
- ✅ No-code AI Agent builder UI (Agent Flows)
- 🔄 UI overhaul

### Desktop Only
- 🔄 Assistant mode
- 🔄 Web macro recording & replaying
- 🔄 More desktop automation (file editing, etc.)

### Docker/Self-Hosted/Cloud Only
- 🔄 Custom Authentication via plugins (OAuth)
- 🔄 Fine-Grained Permission system overhaul

**Legend:**
- ✅ = Completed
- 🔄 = In Progress
- ⬜ = Planned

---

## Additional Resources

- **Official Docs:** https://docs.anythingllm.com/
- **GitHub:** https://github.com/Mintplex-Labs/anything-llm
- **Discord:** https://discord.gg/6UyHPeGZAC
- **MCP Servers:** https://github.com/modelcontextprotocol/servers
- **Download:** https://anythingllm.com/download

---

**Note:** This documentation summary is based on AnythingLLM v1.9.1. Always refer to official documentation for the most up-to-date information.
