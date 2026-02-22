# Paperless Stack

Docker Compose stack for running Paperless-ngx with optional local AI capabilities.

## Quick Start

1. **Clone and configure**

   ```bash
   git clone https://github.com/timothystewart6/paperless-stack.git
   cd paperless-stack
   ```

2. **Edit environment files**

   Update passwords and secrets in each service's `.env` file:
   - `./paperless/.env` - Paperless configuration
   - `./postgres/.env` - Database credentials (must match Paperless config)
   - Other service `.env` files as needed

3. **Start the stack**

   ```bash
   docker compose up -d
   ```

4. **Create admin account**

   Open <http://localhost:8000> and create your Paperless admin account.

5. **Optional: Configure AI services**

   For AI features, you'll need to:
   - Open Open WebUI at <http://localhost:3001> and pull models:
     - `llama3.2:3b` (lightweight, for metadata suggestions and document reasoning)
     - `minicpm-v:8b` (vision model, for improved OCR)
     - These should match the models listed in  `./paperless-ai/.env` and `./paperless-gpt/.env
   - In Paperless, go to Profile → API Tokens → Generate
   - Copy the token and add it to `./paperless-ai/.env` and `./paperless-gpt/.env`
   - Update `./paperless-ai/.env` with Paperless username
   - Restart services: `docker compose restart`

**The AI components are entirely optional** and can be disabled by commenting them out. Paperless works great without AI.

## Access

| Service | URL |
| ------- | --- |
| Paperless-ngx | <http://localhost:8000> |
| Open WebUI | <http://localhost:3001> |
| Paperless-AI | <http://localhost:3000> |
| Paperless-GPT | <http://localhost:3002> |
| Dozzle (logs) | <http://localhost:8080> |

## What's Included

- Pre-configured environment files for all services
- Paperless-ngx with OCR, tagging, and search capabilities
- PostgreSQL database and Redis cache
- Document conversion and text extraction (Gotenberg, Tika)
- Optional local AI features:
  - Ollama for local LLM inference
  - Open WebUI for model management
  - Paperless-AI for metadata suggestions
  - Paperless-GPT for vision-based OCR improvements and metadata suggestions
- Log viewer with Dozzle

## Basic Usage

- **Add documents**: Drop files in `./paperless/consume/` folder
- **Search documents**: Use the search bar in Paperless web interface
- **View logs**: Use Dozzle at <http://localhost:8080>

## Important Notes

- **Security**: Use a VPN for remote access. Don't expose these services directly to the internet.
- **Backups**: Back up `./paperless/data/`, `./paperless/media/`, and `./postgres/data/`
- **Updates**: Run `docker compose pull && docker compose up -d`

## Resources

- **Detailed Guide**: [Self-Hosted Paperless-ngx + Optional Local AI](https://technotim.com/posts/paperless-ngx-local-ai/)
- **Video Tutorial**: [Paperless-ngx + Local AI (Optional): Better OCR, Self-Hosted, No Cloud](https://www.youtube.com/watch?v=NMAwHjleqHg)

## Troubleshooting

- Check logs: `docker compose logs [service-name]` or use Dozzle at <http://localhost:8080>
- Check service status: `docker compose ps`
- Verify database credentials match between paperless and postgres .env files
- For AI issues, ensure Ollama is running and models are downloaded

**For detailed troubleshooting including AI setup, vision OCR, and workflows, see the guide above.**

## Acknowledgments

This stack is built using these awesome open-source projects:

- **[Paperless-ngx](https://github.com/paperless-ngx/paperless-ngx)** - Document management system with OCR and search
- **[Ollama](https://github.com/ollama/ollama)** - Local LLM inference
- **[Open WebUI](https://github.com/open-webui/open-webui)** - Web interface for Ollama
- **[Paperless-AI](https://github.com/clusterzx/paperless-ai)** - AI-powered metadata suggestions
- **[Paperless-GPT](https://github.com/icereed/paperless-gpt)** - Vision OCR for Paperless
- **[PostgreSQL](https://github.com/postgres/postgres)** - Database system
- **[Redis](https://github.com/redis/redis)** - Cache and message broker
- **[Gotenberg](https://github.com/gotenberg/gotenberg)** - Document conversion API
- **[Apache Tika](https://github.com/apache/tika)** - Content detection and extraction
- **[Dozzle](https://github.com/amir20/dozzle)** - Real-time log viewer for Docker

Special thanks to the maintainers and contributors of these projects for making self-hosted document management and local AI accessible.
