#  Kong AI Tracing Plugin

A **Kong Gateway plugin** for **AI observability and tracing**, designed to automatically capture AI request/response data and send complete traces to [Langfuse](https://langfuse.com).

---

##  Features

- Auto-detects AI providers (OpenAI-compatible, vLLM, etc.)
- Extracts user/session metadata from headers and body
- Captures prompt, completion, latency, token usage, and parameters
- Asynchronous export to **Langfuse** ingestion API
- Lightweight and high-performance (no blocking I/O)

---

##  Configuration

| Parameter | Type | Default | Description |
|------------|------|----------|-------------|
| `langfuse_enabled` | boolean | `false` | Enable or disable Langfuse integration |
| `langfuse_endpoint` | string | `https://cloud.langfuse.com/api/public/ingestion` | Langfuse ingestion endpoint |
| `langfuse_public_key` | string | — | Langfuse public API key |
| `langfuse_secret_key` | string | — | Langfuse secret API key |
| `langfuse_timeout` | number | `5000` | HTTP timeout in ms |
| `environment` | string | `production` | Environment tag |
| `log_level` | string | `info` | Log level for debugging |

---

##  Installation

1. Clone the repo:
   ```bash
   git clone https://github.com/ramtin-dev/kong-ai-tracing.git
   cd kong-ai-tracing 
   ```
2. Build and install via LuaRocks:
   ```bash
    luarocks make rockspec/kong-plugin-ai-tracing-1.0.0-1.rockspec
   ```
3. Enable plugin in your kong.conf:
    ```bash
    plugins = bundled,ai-tracing
    ```
4. Configure via Admin API:
    ```bash
    curl -X POST http://localhost:8001/services/ai-service/plugins \
      --data "name=ai-tracing" \
      --data "config.langfuse_enabled=true" \
      --data "config.langfuse_public_key=pk-lf-xxxxxxxxxxxx" \
      --data "config.langfuse_secret_key=sk-lf-xxxxxxxxxxxx" \
      --data "config.langfuse_endpoint=https://cloud.langfuse.com/api/public/ingestion" \
      --data "config.langfuse_timeout=5000"
    ```

or if you use kong from docker you can bind the plugin's directory to the kong container by docker-compose. yml
```yml
    volumes:
      - ./kong-langfuse-tracing/kong/plugins/ai-tracing:/usr/local/kong/plugins/ai-tracing

```
and then you should restart the containers .

## Example Trace in Langfuse

After enabling the plugin, each AI API call (e.g. /v1/chat/completions) will automatically be traced and visible in Langfuse, with full context:

user_id / session_id

model name & tokens

latency & throughput

input/output content


## Contact

Developed by Ramtin
DevOps Engineer — 2025

## License

MIT License © 2025 Ramtin


