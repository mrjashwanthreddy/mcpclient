# Spring AI MCP Client

![Java](https://img.shields.io/badge/Java-21-orange?style=flat-square&logo=openjdk)
![Spring AI](https://img.shields.io/badge/Spring%20AI-1.1.2-green?style=flat-square&logo=spring)
![MCP](https://img.shields.io/badge/Protocol-MCP-blue?style=flat-square)

This repository contains a **Model Context Protocol (MCP) Client** implemented using Spring AI. It demonstrates how to decouple AI capabilities by connecting a Spring Boot application to external MCP Servers to access tools, resources, and prompts dynamically.

## 👤 Author

**Jashwanth Reddy**

* **GitHub**: [@mrjashwanthreddy](https://github.com/mrjashwanthreddy)
* **LinkedIn**: [@jashwanth-java-developer](https://www.linkedin.com/in/jashwanth-java-developer/)
* **Instagram**: [@mrjashwanthreddy](https://www.instagram.com/mr.jashwanthreddy/)

## 📖 Overview

The **Model Context Protocol (MCP)** standardizes how AI models interact with external systems. Instead of hardcoding tools inside your application, this Client connects to separate Servers that host the tools.

This client is designed to work seamlessly with the following implementations:
* **[mcpserverstdio](https://github.com/mrjashwanthreddy/mcpserverstdio)**: A server running as a local process (Standard Input/Output).
* **[mcpserverremote](https://github.com/mrjashwanthreddy/mcpserverremote)**: A server running over HTTP (Server-Sent Events).

## 🚀 Features

* **Dynamic Tool Discovery**: Automatically fetches available tools from connected MCP servers at runtime.
* **Flexible Transport Layers**:
    * **Stdio**: Spawns and communicates with local subprocesses (great for local dev/CLI tools).
    * **SSE (Server-Sent Events)**: Connects to remote web services (great for distributed systems).
* **Spring AI Integration**: Uses `ChatClient` to seamlessly weave MCP tools into LLM conversations.

## 🛠️ Prerequisites

* **Java 21**
* **Spring Boot 3.5.9**
* **Spring AI 1.1.2**
* **Maven**
* **Ollama** (if running local LLMs) or an OpenAI API Key.
* An MCP Server to connect to (see the links in the Overview).

## ⚙️ Configuration & Setup

1.  **Clone the Repository**
    ```bash
    git clone [https://github.com/mrjashwanthreddy/mcpclient.git](https://github.com/mrjashwanthreddy/mcpclient.git)
    cd mcpclient
    ```

2.  **Configure the Connection**
    Open `src/main/resources/application.properties`. You must choose **one** connection method (Stdio or HTTP) depending on which server you are testing.

    ### Option A: Connect to Local Process (Stdio)
    *Best for testing with `mcpserverstdio` jar file.*

    ```properties
    # 1. Disable HTTP if enabled
    # spring.ai.mcp.client.http.enabled=false

    # 2. Enable Stdio
    spring.ai.mcp.client.type=stdio
    
    # 3. Setup mcp servers in mcp-servers.json file
    spring.ai.mcp.client.stdio.servers-configuration=classpath:mcp-servers.json
    ```

    ### Option B: Connect to Remote Server (HTTP/SSE)
    *Best for testing with `mcpserverremote` running on localhost:8080.*

    ```properties
    # 1. Set type to HTTP
    spring.ai.mcp.client.type=http

    # 2. Configure URL for SSE/Streamable http
    spring.ai.mcp.client.http.base-url=http://localhost:8090 (change it with your port number)
    spring.ai.mcp.client.http.timeout=10s
    ```

3.  **Configure the LLM**
    Ensure you have your LLM provider configured (e.g., Ollama).
    ```properties
    spring.ai.ollama.chat.options.model=llama3.2
    ```

## 🏃‍♂️ Running the Application

Build and run the project using Maven:

```bash
./mvnw clean package
./mvnw spring-boot:run