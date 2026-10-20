# ai-movie-assistant-mcp

An AI movie assistant: an MCP (Model Context Protocol) server that gives Claude and other MCP clients tools for The Movie Database (TMDB): search movies and TV shows, see what is trending, and read reviews and recommendations through natural language. I built it at a workshop with [PwC](https://www.pwc.no/).

## Tools
| Tool | What it does |
|---|---|
| `tmdb_search_movies` | Search movies by title, year, region, etc. |
| `tmdb_multi_search` | Search movies, TV shows and people in one query |
| `tmdb_trending_search` | Trending movies, TV shows and people (daily/weekly) |
| `tmdb_tv_recommendations` | Recommendations for a TV series |
| `tmdb_review_details` | Details for a single review |
| `tmdb_movie_reviews` | User reviews for a movie |
| `tmdb_tv_reviews` | User reviews for a TV show |

## Tech stack
Python 3.12, MCP Python SDK (`FastMCP`), `requests`, TMDB API v3, `uv`.

## How it is structured
- `services.py` - calls to the TMDB API (Bearer token read from `TMDB_API_KEY`).
- `server.py` - the MCP tool definitions, which format API results as readable text.
- `main.py` - entry point, runs the server over stdio.

## How to run
1. Install dependencies: `uv sync`
2. Get an API read access token from [TMDB](https://www.themoviedb.org/settings/api) and put it in a `.env` file:
   ```
   TMDB_API_KEY="<your-token>"
   ```
   ![TMDB subscription page](public/subscribe_screenshot.png)
3. Start the server: `uv run main.py` (prints `Starting MCP server...`).

### Use it from Claude Desktop
Add this to `claude_desktop_config.json`, with the absolute path to this repo:
```json
{
  "mcpServers": {
    "tmdb-server": {
      "command": "uv",
      "args": ["--directory", "/ABSOLUTE/PATH/TO/ai-movie-assistant-mcp", "run", "main.py"]
    }
  }
}
```
On macOS you may need the full path to `uv`, for example `/opt/homebrew/bin/uv`.

## What I learned
- How MCP exposes functions as tools an LLM can discover and call.
- Wrapping a REST API with Bearer-token authentication, and keeping the key out of the code with environment variables.
- Separating API access (`services.py`) from the tool layer (`server.py`), and extending the server with more endpoints (TV reviews, review details, trending).

Not affiliated with TMDB.
