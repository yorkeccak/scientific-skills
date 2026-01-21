---
name: biorxiv-search
description: Search bioRxiv biological preprints with natural language queries. Powered by Valyu's semantic search - no API param parsing needed, just ask in plain English. Returns full-text content with images.
---

# bioRxiv Search

Search the complete bioRxiv database of biological sciences preprints using natural language queries powered by Valyu's semantic search API.

## Why This Skill is Powerful

- **No API Parameter Parsing**: Just pass natural language queries directly - no need to construct complex search parameters
- **Semantic Search**: Understands the meaning of your query, not just keyword matching
- **Full-Text Access**: Returns complete article content, not just abstracts
- **Image Links**: Includes figures and images from papers
- **Comprehensive Coverage**: Access to all bioRxiv preprints in biological sciences

## Requirements

1. Node.js 18+ (uses built-in fetch)
2. Valyu API key from https://platform.valyu.ai ($10 free credits)

## CRITICAL: Script Path Resolution

The `scripts/search` commands in this documentation are relative to this skill's installation directory.

Before running any command, locate the script using:

```bash
BIORXIV_SCRIPT=$(find ~/.claude/plugins/cache -name "search" -path "*/biorxiv-search/*/scripts/*" -type f 2>/dev/null | head -1)
```

Then use the full path for all commands:
```bash
$BIORXIV_SCRIPT "CRISPR gene editing" 15
```

## API Key Setup Flow

When you run a search and receive `"setup_required": true`, follow this flow:

1. **Ask the user for their API key:**
   "To search bioRxiv, I need your Valyu API key. Get one free ($10 credits) at https://platform.valyu.ai"

2. **Once the user provides the key, run:**
   ```bash
   scripts/search setup <api-key>
   ```

3. **Retry the original search.**

### Example Flow:
```
User: Search bioRxiv for CRISPR advances
→ Response: {"success": false, "setup_required": true, ...}
→ Claude asks: "Please provide your Valyu API key from https://platform.valyu.ai"
→ User: "val_abc123..."
→ Claude runs: scripts/search setup val_abc123...
→ Response: {"success": true, "type": "setup", ...}
→ Claude retries: scripts/search "CRISPR advances" 10
→ Success!
```

## Usage

### Basic Search

```bash
scripts/search "your natural language query" [maxResults]
```

### Examples

```bash
# Search for genetics papers
scripts/search "genome editing efficiency improvements" 15

# Find neuroscience papers
scripts/search "neural circuit mapping techniques" 20

# Search for cell biology papers
scripts/search "mitochondrial dynamics in aging" 10

# Find immunology papers
scripts/search "T cell exhaustion mechanisms" 12
```

### Setup Command

```bash
scripts/search setup <api-key>
```

## Output Format

```json
{
  "success": true,
  "type": "biorxiv_search",
  "query": "CRISPR gene editing",
  "result_count": 10,
  "results": [
    {
      "title": "Article Title",
      "url": "https://biorxiv.org/content/...",
      "content": "Full article text with figures...",
      "source": "biorxiv",
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

### Molecular Biology

```bash
# Find recent molecular biology papers
scripts/search "protein-protein interaction networks" 50
```

### Neuroscience

```bash
# Search for neuroscience research
scripts/search "optogenetics in behavior studies" 20
```

### Genomics

```bash
# Find genomics papers
scripts/search "single cell RNA sequencing analysis" 15
```

### Developmental Biology

```bash
# Search for developmental biology papers
scripts/search "embryonic stem cell differentiation" 25
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
