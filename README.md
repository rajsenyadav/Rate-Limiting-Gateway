<div align="center">

  <img src="https://raw.githubusercontent.com/mridulrajgaria/LLM-API-Gateway-with-Token-Based-Rate-Limiting/main/public/demo-preview.png" width="80%" alt="Enterprise LLM Gateway UI" />

  <h1>🚀 Enterprise LLM API Gateway</h1>
  
  <p>
    <strong>A high-performance, secure middleware proxy engineered to protect GenAI infrastructure.</strong>
  </p>

  <p>
    <a href="#features">Features</a> • 
    <a href="#architecture">Architecture</a> • 
    <a href="#tech-stack">Tech Stack</a> • 
    <a href="#quick-start">Quick Start</a>
  </p>
</div>

---

## ⚡ The Problem: Tokens vs. Requests
Standard API Gateways rate-limit based on **Requests Per Second (RPS)**. However, AI providers like OpenAI and Anthropic charge by the **Token**. 

If a malicious actor sends just *one* request, but forces the LLM to generate a 10,000-word essay, a standard gateway will let it pass—potentially costing you thousands of dollars in minutes. 

## 🛡️ The Solution
This project acts as a secure tollbooth between your users and the LLM providers. It doesn't just limit requests; it intercepts the response, parses the JSON payload, extracts the exact token usage, and deducts it from the user's allocated budget in MongoDB. 

If a user exceeds their token budget, they are instantly cut off, **protecting your OpenAI API keys from abuse and bankruptcy.**

---

## 🏗️ System Architecture

The gateway is built for high concurrency and zero race conditions, processing requests through a strict 4-step pipeline:

1. 💻 **Client Request:** The user hits the proxy endpoint with a custom `x-api-key`.
2. 🗄️ **MongoDB Auth:** The gateway validates the API key and checks if the user has a sufficient token budget remaining.
3. ⚡ **Redis Limiter:** An atomic Token-Bucket algorithm (written in Lua) executes directly in Redis to prevent DDoS and race conditions under heavy load.
4. 🧠 **AI Proxy:** The gateway forwards the request to OpenAI, awaits the response, extracts the consumed tokens, and asynchronously logs the usage back to MongoDB to prevent blocking the response thread.

---

## ✨ Key Features

- **Interactive Telemetry Dashboard:** Includes a custom-built, dual-pane frontend with live glowing architecture nodes and spinning telemetry gauges (Latency & Tokens) for real-time visualization.
- **Atomic Lua Scripting:** Rate limits are enforced via custom Lua scripts in Redis, guaranteeing O(1) atomic precision even with 5,000+ concurrent requests.
- **Dynamic Mock Endpoint:** Includes a zero-cost `/mock/chat/completions` endpoint for public demos to prevent real API billing charges.
- **Asynchronous Logging:** Token deduction and logging are offloaded from the main thread, adding <15ms overhead to the proxy lifecycle.

---

## 🛠️ Tech Stack

- **Backend:** Node.js, Express.js
- **Database:** MongoDB (Mongoose) for persistent auth and token logging
- **Caching & Limiting:** Redis (ioredis) for atomic operations
-


---
<div align="center">
  <i>Engineered for scale, security, and precision.</i>
</div>
