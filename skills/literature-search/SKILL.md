---
name: literature-search
description: Search across PubMed, arXiv, bioRxiv, and medRxiv with natural language queries. Powered by Valyu's semantic search - no API param parsing needed, just ask in plain English. Returns full-text content with images from all major scientific literature sources.
---

# Literature Search

Search across all major scientific literature databases (PubMed, arXiv, bioRxiv, medRxiv) simultaneously using natural language queries powered by Valyu's semantic search API.

## Why This Skill is Powerful

- **No API Parameter Parsing**: Just pass natural language queries directly - no need to construct complex search parameters
- **Semantic Search**: Understands the meaning of your query, not just keyword matching
- **Full-Text Access**: Returns complete article content, not just abstracts
- **Image Links**: Includes figures and images from papers
- **Comprehensive Coverage**: Search across PubMed, arXiv, bioRxiv, and medRxiv simultaneously
- **Unified Results**: Get results from all sources in a single query

## Requirements

1. Node.js 18+ (uses built-in fetch)
2. Valyu API key from https://platform.valyu.ai ($10 free credits)

## CRITICAL: Script Path Resolution

The `scripts/search` commands in this documentation are relative to this skill's installation directory.

Before running any command, locate the script using:

```bash
LITERATURE_SCRIPT=$(find ~/.claude/plugins/cache -name "search" -path "*/literature-search/*/scripts/*" -type f 2>/dev/null | head -1)
```

Then use the full path for all commands:
```bash
$LITERATURE_SCRIPT "CRISPR gene editing advances" 15
```

## API Key Setup Flow

When you run a search and receive `"setup_required": true`, follow this flow:

1. **Ask the user for their API key:**
   "To search scientific literature, I need your Valyu API key. Get one free ($10 credits) at https://platform.valyu.ai"

2. **Once the user provides the key, run:**
   ```bash
   scripts/search setup <api-key>
   ```

3. **Retry the original search.**

## Usage

### Basic Search

```bash
scripts/search "your natural language query" [maxResults]
```

### Examples

```bash
# Search across all literature sources
scripts/search "transformer architectures in deep learning" 20

# Find biomedical research
scripts/search "CAR-T cell therapy mechanisms" 15

# Search for COVID research
scripts/search "long COVID neurological effects" 25

# Find AI/ML papers
scripts/search "large language model training techniques" 30
```

### Setup Command

```bash
scripts/search setup <api-key>
```

## Output Format

```json
{
  "success": true,
  "type": "literature_search",
  "query": "CRISPR gene editing advances",
  "result_count": 15,
  "results": [
    {
      "title": "Article Title",
      "url": "https://...",
      "content": "Full article text with figures...",
      "source": "pubmed|arxiv|biorxiv|medrxiv",
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
scripts/search "query" 20 | jq -r '.results[].title'

# Get URLs
scripts/search "query" 20 | jq -r '.results[].url'

# Extract full content
scripts/search "query" 20 | jq -r '.results[].content'

# Filter by source
scripts/search "query" 20 | jq -r '.results[] | select(.source == "arxiv") | .title'
```

## Common Use Cases

### Comprehensive Literature Review

```bash
# Search across all sources for thorough review
scripts/search "mechanisms of cellular senescence" 100
```

### Cross-Disciplinary Research

```bash
# Find papers spanning multiple fields
scripts/search "quantum computing applications in drug discovery" 50
```

### Recent Developments

```bash
# Get latest preprints and publications
scripts/search "foundation models for protein folding" 30
```

### Medical Research

```bash
# Search biomedical literature comprehensively
scripts/search "immunotherapy checkpoint inhibitors resistance" 40
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
