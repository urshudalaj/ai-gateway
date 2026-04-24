# AI Gateway

A fork of [envoyproxy/ai-gateway](https://github.com/envoyproxy/ai-gateway) — an Envoy-based gateway for routing and managing AI/LLM API traffic.

## Overview

AI Gateway provides a unified interface for interacting with multiple AI providers (OpenAI, Anthropic, Google Gemini, Ollama, etc.) through a single Envoy proxy endpoint. It handles:

- **Request routing** — intelligently route requests to different AI backends
- **Authentication** — manage API keys and credentials per provider
- **Rate limiting** — enforce token and request rate limits
- **Observability** — metrics, tracing, and logging for AI workloads
- **Failover** — automatic fallback between providers

## Prerequisites

- Go 1.22+
- Envoy proxy (see [`.envoy-version`](.envoy-version) for required version)
- Docker (optional, for local development)

## Quick Start

### Local Development with Ollama

1. Copy and configure environment variables:
   ```bash
   cp .env.ollama .env
   # Edit .env with your settings
   ```

2. Start the gateway:
   ```bash
   make run
   ```

3. Send a request:
   ```bash
   curl -X POST http://localhost:10000/v1/chat/completions \
     -H 'Content-Type: application/json' \
     -d '{"model": "llama3", "messages": [{"role": "user", "content": "Hello!"}]}'
   ```

## Configuration

Configuration is managed via YAML files and environment variables. See the `config/` directory for examples.

### Environment Variables

| Variable | Description | Default |
|---|---|---|
| `AIGW_PORT` | Gateway listen port | `10000` |
| `AIGW_ADMIN_PORT` | Envoy admin port | `9901` |
| `AIGW_LOG_LEVEL` | Log level (`debug`, `info`, `warn`, `error`) | `info` |

## Architecture

```
Client → Envoy Proxy → AI Gateway Filter → AI Provider
                            ↓
                    (routing, auth, rate limiting)
```

The gateway runs as an Envoy external processor (ext_proc) or HTTP filter, intercepting requests and responses to apply AI-specific logic.

## Development

```bash
# Run tests
make test

# Run linter
make lint

# Build binaries
make build

# Generate code (protobuf, mocks)
make generate
```

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) before submitting pull requests.

## License

Apache License 2.0 — see [LICENSE](LICENSE) for details.
