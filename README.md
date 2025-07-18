# 🚀🤖 Crawl4AI: Open-source LLM Friendly Web Crawler & Scraper

> **Note**: This is a fork of the original [Crawl4AI](https://github.com/unclecode/crawl4ai) project, customized for MCP (Model Context Protocol) agent usage.

## MCP Agent Setup Guide

This guide helps you set up Crawl4AI as an MCP (Model Context Protocol) agent. 

**Quick Start**: Most users should choose **Option 2 (Local Setup)** as it supports both headed mode (with visible browser) and headless mode. Only use Docker if you specifically need a containerized environment and don't mind headless-only operation.

**[View Original README](README_ORIGINAL.md)** for complete project documentation.

## 📋 Prerequisites

- Python 3.10+
- Docker and Docker Compose (for Docker setup)
- Git

## 🐳 Option 1: Docker Setup (Headless Only)

⚠️ **Important**: Docker setup runs in headless mode only (no visual browser window). If you want to see the browser while crawling, use Option 2 instead.

### Step 1: Configure Environment
```bash
# Copy the example environment file
cp deploy/docker/.llm.env.example .llm.env
```

### Step 2: Set Configuration
```bash
# Copy the headless configuration
cp deploy/docker/config_headless.yml deploy/docker/config.yml
```

### Step 3: Start the Service
```bash
# Build and start the container
docker compose up --build -d
```

## 💻 Option 2: Local Setup (Recommended)

✅ **Recommended**: Local setup supports both headless and headed modes. Choose headed mode to see the browser window while crawling (useful for debugging and development).

### Step 1: Install Crawl4AI
```bash
# Install the package in development mode
pip install -e .

# Install Playwright browser dependencies
python -m playwright install --with-deps chromium
```

### Step 2: Install API Dependencies
```bash
# Navigate to the Docker directory and install requirements
cd deploy/docker/
pip install -r requirements.txt
```

### Step 3: Choose Your Configuration

#### For Headless Mode (No Visual Browser)
```bash
cp config_headless.yml config.yml
```

#### For Headed Mode (With Visual Browser)
```bash
cp config_headed.yml config.yml
```

### Step 4: Start the Server
```bash
# Start the API server
gunicorn --bind 0.0.0.0:11235 --workers 1 --threads 4 --timeout 1800 --graceful-timeout 30 --keep-alive 300 --log-level info --worker-class uvicorn.workers.UvicornWorker server:app
```

## 🔧 Configuration Options

### Browser Modes
- **Headed Mode** (Recommended): Shows the browser window while crawling. Great for:
  - Debugging and development
  - Seeing what the crawler is doing
  - Handling complex sites that need visual interaction
  
- **Headless Mode**: No browser window, runs in background. Good for:
  - Production servers
  - Automated scripts
  - When you don't need to see the browser

### Setup Methods
- **Local Setup**: More flexible, supports both headed and headless modes
- **Docker Setup**: Simpler but headless only, good for consistent environments

## 🚀 Next Steps

Once set up, your MCP agent will be running and ready to handle web crawling requests!

### 🧪 Test with MCP Inspector

After setup, test your installation with the MCP Inspector:

```bash
# Run the MCP inspector
npx @modelcontextprotocol/inspector
```

When prompted by the MCP inspector:
- **URL**: `http://127.0.0.1:11235/mcp/sse`
- **Connection type**: `sse`

Once connected, you can test the setup by calling available tools such as:
- `visit_tool` - Visit and crawl web pages
- `google_search_markdown` - Search Google and get markdown results

This helps verify that your MCP agent is working correctly with the selected website.

### 🔗 API Endpoints

- **Main API**: `http://localhost:11235`
- **Schema**: `http://localhost:11235/mcp/schema` - View the API schema and available endpoints
- **MCP SSE**: `http://localhost:11235/mcp/sse` - Server-Sent Events endpoint for MCP

### 🔄 Convert to StreamableHTTP (Optional)

If you need to convert the SSE (Server-Sent Events) connection to StreamableHTTP for compatibility with certain clients:

```bash
# Install and run supergateway to convert SSE to StreamableHTTP
npx -y supergateway --sse http://127.0.0.1:11235/mcp/sse --outputTransport streamableHttp
```