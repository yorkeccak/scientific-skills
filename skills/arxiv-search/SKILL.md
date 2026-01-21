---
name: arxiv-search
description: Search arXiv preprints in physics, math, CS, and quantitative biology with natural language queries. Powered by Valyu's semantic search - no API param parsing needed, just ask in plain English. Returns full-text content with images.
---

# arXiv Search

Search the complete arXiv database of preprints across physics, mathematics, computer science, and quantitative biology using natural language queries powered by Valyu's semantic search API.

## Why This Skill is Powerful

- **No API Parameter Parsing**: Just pass natural language queries directly - no need to construct complex search parameters
- **Semantic Search**: Understands the meaning of your query, not just keyword matching
- **Full-Text Access**: Returns complete article content, not just abstracts
- **Image Links**: Includes figures and images from papers
- **Comprehensive Coverage**: Access to all of arXiv's preprint archive across multiple disciplines

## Requirements

1. Node.js 18+ (uses built-in fetch)
2. Valyu API key from https://platform.valyu.ai ($10 free credits)

## CRITICAL: Script Path Resolution

The `scripts/search` commands in this documentation are relative to this skill's installation directory.

Before running any command, locate the script using:

```bash
ARXIV_SCRIPT=$(find ~/.claude/plugins/cache -name "search" -path "*/arxiv-search/*/scripts/*" -type f 2>/dev/null | head -1)
```

Then use the full path for all commands:
```bash
$ARXIV_SCRIPT "quantum entanglement" 15
```

## API Key Setup Flow

When you run a search and receive `"setup_required": true`, follow this flow:

1. **Ask the user for their API key:**
   "To search arXiv, I need your Valyu API key. Get one free ($10 credits) at https://platform.valyu.ai"

2. **Once the user provides the key, run:**
   ```bash
   scripts/search setup <api-key>
   ```

3. **Retry the original search.**

### Example Flow:
```
User: Search arXiv for transformer architecture papers
→ Response: {"success": false, "setup_required": true, ...}
→ Claude asks: "Please provide your Valyu API key from https://platform.valyu.ai"
→ User: "val_abc123..."
→ Claude runs: scripts/search setup val_abc123...
→ Response: {"success": true, "type": "setup", ...}
→ Claude retries: scripts/search "transformer architecture papers" 10
→ Success!
```

## Usage

### Basic Search

```bash
scripts/search "your natural language query" [maxResults]
```

### Examples

```bash
# Search for AI/ML papers
scripts/search "attention mechanisms in neural networks" 15

# Find physics papers
scripts/search "quantum computing error correction" 20

# Search for math papers
scripts/search "topology and algebraic geometry" 10

# Find quantitative biology papers
scripts/search "protein folding prediction methods" 12
```

### Setup Command

```bash
scripts/search setup <api-key>
```

## Output Format

```json
{
  "success": true,
  "type": "arxiv_search",
  "query": "quantum entanglement",
  "result_count": 10,
  "results": [
    {
      "title": "Article Title",
      "url": "https://arxiv.org/abs/...",
      "content": "Full article text with figures...",
      "source": "arxiv",
      "relevance_score": 0.95,
      "images": ["https://example.com/figure1.jpg"]
    }
  ],
  "cost": 0.025
}
```

## Processing Results

### With jq

```bash
# Get article titles
scripts/search "query" 10 | jq -r '.results[].title'

# Get URLs
scripts/search "query" 10 | jq -r '.results[].url'

# Extract full content
scripts/search "query" 10 | jq -r '.results[].content'
```

## Common Use Cases

### AI/ML Research

```bash
# Find recent machine learning papers
scripts/search "large language model architectures" 50
```

### Physics Research

```bash
# Search for quantum physics papers
scripts/search "topological quantum computation" 20
```

### Mathematics

```bash
# Find math papers
scripts/search "representation theory and Lie algebras" 15
```

### Computer Science

```bash
# Search for CS theory papers
scripts/search "distributed systems consensus algorithms" 25
```

## Error Handling

All commands return JSON with `success` field:

```json
{
  "success": false,
  "error": "Error message"
}
```

Exit codes:
- `0` - Success
- `1` - Error (check JSON for details)

## API Endpoint

- Base URL: `https://api.valyu.ai/v1`
- Endpoint: `/search`
- Authentication: X-API-Key header

## Architecture

```
scripts/
├── search          # Bash wrapper
└── search.mjs      # Node.js CLI
```

Direct API calls using Node.js built-in `fetch()`, zero external dependencies.
