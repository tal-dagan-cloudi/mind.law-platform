# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Mind.Law is a full-stack AI-powered legal document management and chat platform, forked from AnythingLLM. It's a complete multi-user system that enables conversations with documents using various LLM providers and vector databases, with full MCP (Model Context Protocol) compatibility and AI agent capabilities.

## Monorepo Structure

This is a monorepo with six main components:

```
mind.law-platform/
├── frontend/          # ViteJS + React UI application
├── server/           # NodeJS Express API server with Prisma ORM
├── collector/        # NodeJS document processing server
├── docker/           # Docker deployment configurations
├── embed/            # Submodule: embeddable chat widget
└── browser-extension/ # Submodule: Chrome extension
```

## Development Setup Commands

### Initial Setup (Run Once)

```bash
# Install dependencies, setup .env files, and initialize Prisma database
yarn setup

# This runs:
# - yarn install in all workspaces
# - Copies .env.example files to .env files
# - yarn prisma:generate
# - yarn prisma:migrate
# - yarn prisma:seed
```

### Daily Development Workflow

```bash
# Option 1: Run all services concurrently in one terminal
yarn dev:all

# Option 2: Run services in separate terminal tabs (recommended for debugging)
yarn dev:server      # Start Express API server (port 3001)
yarn dev:frontend    # Start Vite frontend dev server (port 5173)
yarn dev:collector   # Start document collector (port 8888)
```

### Database Management (Prisma)

```bash
yarn prisma:generate    # Generate Prisma client after schema changes
yarn prisma:migrate     # Create and apply migrations
yarn prisma:seed        # Seed database with initial data
yarn prisma:reset       # Wipe database and re-migrate (destructive!)
```

### Code Quality

```bash
yarn lint              # Run Prettier on server, frontend, and collector
yarn test              # Run Jest tests
```

### Production Build

```bash
yarn prod:frontend     # Build frontend for production
yarn prod:server       # Start production server
```

## Architecture Overview

### Server Architecture (`server/`)

**Entry Point:** `server/index.js`

The server is an Express application with:

- **Prisma ORM** with SQLite (default) or PostgreSQL support
- **WebSocket support** via `@mintplex-labs/express-ws` for real-time agent communications
- **Multi-user authentication** with JWT and role-based access control
- **MCP (Model Context Protocol) integration** for external tool connections
- **Background jobs** via Bree for document syncing and maintenance

**Key Directories:**

- `endpoints/` - API route handlers (system, workspaces, chat, admin, embed, agents, MCP, etc.)
- `models/` - Prisma database models and query helpers
- `utils/` - Core utilities organized by domain:
  - `AiProviders/` - LLM integrations (OpenAI, Anthropic, Ollama, etc.)
  - `EmbeddingEngines/` - Text embedding providers
  - `agents/` - AI agent implementations
  - `agentFlows/` - No-code agent flow builder
  - `MCP/` - Model Context Protocol client and server management
  - `DocumentManager/` - Vector database document management
  - `chats/` - Chat history and streaming
  - `database/` - Database connection helpers
- `storage/` - File storage for documents, vectors, models, and SQLite database
- `prisma/` - Database schema and migrations

**Database Schema (Key Tables):**

- `users` - User accounts with roles (default, admin, manager)
- `workspaces` - Chat workspaces (like threads) with document isolation
- `workspace_documents` - Documents attached to workspaces
- `workspace_chats` - Chat messages within workspaces
- `workspace_threads` - Threaded conversations within workspaces
- `system_settings` - Global configuration
- `embed_configs` - Embeddable chat widget configurations
- `agent_flows` - No-code agent flow definitions
- `mcp_servers` - Model Context Protocol server configurations

### Frontend Architecture (`frontend/`)

**Entry Point:** `frontend/src/main.jsx`

Built with:

- **React 18** with functional components and hooks
- **Vite** for fast development and optimized builds
- **React Router v6** for SPA routing
- **TailwindCSS** for styling
- **i18next** for internationalization (20+ languages)

**Key Directories:**

- `pages/` - Main application pages:
  - `Login/` - Authentication
  - `Main/` - Main dashboard (workspace selection)
  - `Admin/` - Admin panel (user management, system settings)
  - `WorkspaceChat/` - Chat interface with documents
  - `WorkspaceSettings/` - Workspace configuration
  - `OnboardingFlow/` - Initial setup wizard
- `components/` - Reusable UI components
- `models/` - API client functions (organized by domain: workspace, system, admin, etc.)
- `hooks/` - Custom React hooks
- `utils/` - Frontend utilities
- `locales/` - Internationalization translation files (20+ languages)

**Context Providers:**

- `AuthContext` - User authentication state
- `LogoContext` - Customizable branding
- `PfpContext` - User profile pictures
- `PWAContext` - Progressive Web App functionality

### Collector Architecture (`collector/`)

**Entry Point:** `collector/index.js`

A separate Express server that handles document ingestion and processing:

- **Document Processing:**
  - PDFs, DOCX, XLSX, TXT, MD, HTML, EPUB
  - YouTube transcripts (via `youtubei.js`)
  - Web scraping (via Puppeteer)
  - Image OCR (via Tesseract.js)
  - Audio/video transcription
- **Extensions:** Pluggable document processors in `extensions/`
- **Hot Directory:** File system watch for automatic ingestion
- **Link Processing:** Web page content extraction

**Key Files:**

- `processSingleFile/` - File upload and conversion logic
- `processLink/` - Web page scraping and parsing
- `processRawText/` - Plain text ingestion
- `extensions/` - Custom document type handlers

### Embed Widget (`embed/`)

**Submodule:** Separate repository for embeddable chat widget

- Standalone React application
- Generates embeddable `<script>` tags
- Customizable branding and theming
- Works with any website

### Browser Extension (`browser-extension/`)

**Submodule:** Chrome extension for Mind.Law integration

- Web page content capture
- Quick document upload
- Browser integration

## Key Technical Concepts

### Workspaces

A **workspace** is Mind.Law's core organizational unit:

- Isolated conversation context (like a ChatGPT thread)
- Can contain multiple documents
- Documents can be shared across workspaces but conversations are isolated
- Each workspace has its own LLM settings, embeddings, and chat history
- Supports multi-user access with permissions

### Document Processing Flow

1. **Upload** → Frontend sends file/link to Collector
2. **Collector** → Processes document, extracts text, chunks content
3. **Server** → Receives processed document, generates embeddings
4. **Vector DB** → Stores document chunks with embeddings
5. **Chat** → Retrieves relevant chunks via similarity search, sends to LLM

### Vector Database Integration

Mind.Law supports multiple vector databases:

- **LanceDB** (default) - Built-in, no external dependencies
- **PGVector** - PostgreSQL extension
- **Pinecone** - Cloud vector database
- **Chroma** - Local or cloud
- **Qdrant** - Self-hosted or cloud
- **Weaviate** - Self-hosted or cloud
- **Milvus/Zilliz** - Enterprise vector database

Configuration is stored in `system_settings` table and loaded at runtime.

### LLM Provider Integration

All LLM integrations are in `server/utils/AiProviders/`:

- Each provider has a standardized interface
- Supports streaming and non-streaming responses
- Handles multi-modal inputs (text + images)
- Provider-specific configuration stored in `system_settings`

### MCP (Model Context Protocol) Integration

Mind.Law has full MCP compatibility for extending capabilities:

- **MCP Servers:** External tools/APIs exposed via MCP protocol
- **Configuration:** Managed via `mcp_servers` table and UI in Admin panel
- **Client:** `server/utils/MCP/` handles MCP client connections
- **Usage:** Agents and chat can invoke MCP tools during conversations

### Agent System

Two types of agents:

1. **Custom Agents** (`server/utils/agents/`) - Pre-built agents with specific capabilities:
   - Web browsing
   - Document summarization
   - SQL query generation
   - Chart generation

2. **Agent Flows** (`server/utils/agentFlows/`) - No-code drag-and-drop agent builder:
   - Visual flow editor in frontend
   - Stored as JSON in `agent_flows` table
   - Runtime execution engine

### Embedding System

Default: **Mind.Law Native Embedder** (runs locally via `@xenova/transformers`)

Alternative providers in `server/utils/EmbeddingEngines/`:
- OpenAI
- Azure OpenAI
- Cohere
- LocalAI
- Ollama
- LM Studio

## Environment Configuration

### Server Environment Variables (`.env.development`)

**Required:**
- `JWT_SECRET` - Secret for JWT token signing
- `SERVER_PORT` - API server port (default: 3001)
- `STORAGE_DIR` - Path for document/vector storage

**Optional (LLM Configuration):**
- `LLM_PROVIDER` - Default LLM provider (openai, anthropic, ollama, etc.)
- `EMBEDDING_MODEL_PREF` - Embedding provider preference
- `VECTOR_DB` - Vector database choice (lancedb, pinecone, chroma, etc.)

**Multi-User Features:**
- `AUTH_TOKEN` - Password for initial setup
- `DISABLE_TELEMETRY` - Set to "true" to opt out of anonymous telemetry

### Collector Environment Variables (`.env`)

- `COLLECTOR_PORT` - Document collector port (default: 8888)
- `SERVER_PORT` - Backend API port for communication

### Frontend Environment Variables (`.env`)

- `VITE_API_BASE` - Backend API URL (default: http://localhost:3001/api)

## Development Best Practices

### Database Changes

1. Modify `server/prisma/schema.prisma`
2. Run `yarn prisma:generate` to update Prisma client
3. Run `yarn prisma:migrate` to create migration
4. Migration files are created in `server/prisma/migrations/`

### Adding a New LLM Provider

1. Create provider class in `server/utils/AiProviders/[provider-name]/`
2. Implement standardized interface:
   - `getChatCompletion()` - Non-streaming
   - `streamGetChatCompletion()` - Streaming
   - `getChatModelLimit()` - Token limits
3. Export from `server/utils/AiProviders/index.js`
4. Add UI configuration in `frontend/src/pages/GeneralSettings/LLMPreference/`

### Adding a New Endpoint

1. Create endpoint file in `server/endpoints/[feature].js`
2. Export endpoint function that takes `apiRouter`
3. Register in `server/index.js`
4. Create corresponding frontend model in `frontend/src/models/[feature].js`

### Working with Docker

```bash
cd docker

# Pull latest image
docker pull mintplexlabs/mindlaw

# Run with mounted storage (recommended)
docker run -d \
  -p 3001:3001 \
  -v /local/path/storage:/app/server/storage \
  --add-host=host.docker.internal:host-gateway \
  mintplexlabs/mindlaw

# Or use docker-compose
docker-compose up -d
```

**Important Docker Notes:**

- Mount `/app/server/storage` to persist data across container updates
- Use `host.docker.internal` to access host services (Ollama, Chroma, etc.) from container
- Requires Docker 18.03+ (Win/Mac) or 20.10+ (Linux) for `host.docker.internal`
- On Linux, add `--add-host=host.docker.internal:host-gateway`

### Frontend Component Patterns

- Use functional components with hooks
- Use `@/` path alias for imports (configured in Vite)
- Error boundaries for graceful failure handling
- Loading skeletons via `react-loading-skeleton`
- Toast notifications via `react-toastify`

### Server API Patterns

- Use `reqBody()` helper from `utils/http` to safely parse request bodies
- Middleware for authentication: `validApiKey`, `validUser`, `validWorkspace`
- Return consistent JSON responses: `{ success: true/false, data?, error? }`
- Stream responses for LLM chat using Server-Sent Events (SSE)

### Testing

- Jest test files in `__tests__/` directories
- Integration tests for API endpoints
- Unit tests for utility functions
- Run tests with `yarn test`

## Multi-User System

**Roles:**

- `admin` - Full system access, user management
- `manager` - Can create/manage workspaces and users
- `default` - Basic user with workspace access

**Permissions:**

- Workspace-level permissions in `workspace_users` table
- System-level permissions via user `role` field
- Invite system for onboarding new users

## Common Gotchas

1. **Collector Communication:** Server and Collector must be able to reach each other. In Docker, use `host.docker.internal`.

2. **Vector Database Initialization:** Vector DB must be initialized before chat works. Check `system_settings` for configuration.

3. **Embedding Model:** Default native embedder downloads models on first use (~500MB). Ensure sufficient disk space.

4. **SQLite vs PostgreSQL:** Default is SQLite. To use PostgreSQL, update `server/prisma/schema.prisma` datasource and run migrations.

5. **CORS in Development:** Collector and Server must have CORS properly configured. Both use `cors({ origin: true })` by default.

6. **Storage Directory:** Server creates `storage/` directory for documents, vectors, and SQLite. Ensure write permissions.

7. **Submodules:** `embed/` and `browser-extension/` are git submodules. Use `git submodule update --init --recursive` if missing.

## Debugging Tips

### Enable HTTP Request Logging

```bash
# In .env.development
ENABLE_HTTP_LOGGER=true
ENABLE_HTTP_LOGGER_TIMESTAMPS=true
```

### Check Database State

```bash
# Open Prisma Studio GUI
cd server
npx prisma studio
```

### View Server Logs

Server logs to console and to `server/storage/logs/` directory.

### Frontend API Debugging

Open browser DevTools → Network tab. API calls to `/api/*` endpoints show request/response details.

### Document Processing Issues

Check collector logs. Collector returns detailed error messages for failed document processing.

## Useful File Paths

- **Main Configuration:** `server/.env.development`
- **Database:** `server/storage/mindlaw.db` (SQLite)
- **Documents Storage:** `server/storage/documents/`
- **Vector Cache:** `server/storage/vector-cache/`
- **Prisma Schema:** `server/prisma/schema.prisma`
- **API Routes:** `server/endpoints/`
- **Frontend Routes:** `frontend/src/pages/`
- **LLM Providers:** `server/utils/AiProviders/`
- **Vector DB Providers:** `server/utils/vectorDbProviders/`

## API Testing

Swagger documentation available when server is running:

```
http://localhost:3001/api/docs
```

Generate/update Swagger docs:

```bash
cd server
yarn swagger
```

## Additional Resources

- [Docker Deployment Guide](./docker/HOW_TO_USE_DOCKER.md)
- [Document Processing](./server/storage/documents/DOCUMENTS.md)
- [Vector Cache](./server/storage/vector-cache/VECTOR_CACHE.md)
- [Embedder Models](./server/storage/models/README.md)
- [Ollama README](./server/utils/AiProviders/ollama/README.md)

## Telemetry

Mind.Law includes anonymous telemetry (PostHog) that can be disabled:

- Set `DISABLE_TELEMETRY=true` in server `.env`
- Or disable via UI: Sidebar → Privacy → Disable Telemetry

Only high-level usage events are tracked (no message content, no identifying information).
