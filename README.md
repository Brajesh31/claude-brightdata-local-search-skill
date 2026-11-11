# Claude Bright Data Local Search Skill

A skill for setting up and using a local search pipeline powered by Bright Data's SERP API. It describes query expansion, multi-engine search retrieval, reciprocal-rank fusion reranking, deduplication, and domain clustering through the local unfancy-search service.

## What It Covers

- **Local deployment:** Run the search service through Docker or Node.js.
- **Bright Data SERP integration:** Configure the token and SERP zone.
- **Query expansion:** Optionally use an Anthropic API key to generate related queries.
- **Multi-engine retrieval:** Collect search results from configured engines.
- **Reranking:** Combine result rankings using reciprocal-rank fusion.
- **Search modes:** Basic, expanded, and research-oriented queries.
- **Filtering:** Use geographic targeting and include/exclude domain filters.
- **Job polling:** Submit search jobs and retrieve completed results from the local API.

## Prerequisites

- Bright Data account with SERP API access.
- Docker, or Node.js 18+ with pnpm.
- Anthropic API key for optional AI query expansion.

## Setup

Clone the repository and open the project containing the local search service:

```bash
git clone https://github.com/Brajesh31/claude-brightdata-local-search-skill.git ~/.claude/skills/brightdata-local-search
```

The skill itself describes setup of the separate unfancy-search service:

```bash
git clone https://github.com/yaronbeen/unfancy-search.git
cd unfancy-search
cp .env.example .env
```

Configure the required environment variables in the service's `.env` file:

```env
BRIGHT_DATA_API_TOKEN=your_brightdata_token
BRIGHT_DATA_SERP_ZONE=serp_api1
ANTHROPIC_API_KEY=your_anthropic_key
```

Start locally with Docker:

```bash
docker compose up -d
```

The service runs at `http://localhost:3000` by default.

## Running a Search

Submit a job:

```bash
curl -X POST http://localhost:3000/api/search \
  -H "Content-Type: application/json" \
  -d '{"query":"web scraping best practices","research":true}'
```

Poll the returned job identifier through `/api/search-status/{jobId}` until the job completes.

## Search Options

The service supports query expansion, research mode, engine selection, geographic targeting, result count, and include/exclude domain filters. See [SKILL.md](SKILL.md) for the full parameter table and command examples.

## Important Notes

This skill explicitly uses the local service, not a hosted endpoint. Keep Bright Data and Anthropic credentials private. Search results and cost depend on configured engines, query mode, and service account limits.
