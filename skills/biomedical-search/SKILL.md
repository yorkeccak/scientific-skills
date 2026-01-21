---
name: biomedical-search
description: Search across PubMed, bioRxiv, medRxiv, ClinicalTrials.gov, and FDA drug labels with natural language queries. Powered by Valyu's semantic search - no API param parsing needed, just ask in plain English. Returns comprehensive biomedical data.
---

# Biomedical Search

Search across all major biomedical databases (PubMed, bioRxiv, medRxiv, ClinicalTrials.gov, FDA drug labels) simultaneously using natural language queries powered by Valyu's semantic search API.

## Why This Skill is Powerful

- **No API Parameter Parsing**: Just pass natural language queries directly - no need to construct complex search parameters
- **Semantic Search**: Understands the meaning of your query, not just keyword matching
- **Full-Text Access**: Returns complete content from literature, trials, and drug labels
- **Image Links**: Includes figures and images when available
- **Comprehensive Coverage**: Search across PubMed, bioRxiv, medRxiv, clinical trials, and drug labels simultaneously
- **Unified Results**: Get results from all biomedical sources in a single query

## Requirements

1. Node.js 18+ (uses built-in fetch)
2. Valyu API key from https://platform.valyu.ai ($10 free credits)

## CRITICAL: Script Path Resolution

The `scripts/search` commands in this documentation are relative to this skill's installation directory.

Before running any command, locate the script using:

```bash
BIOMEDICAL_SCRIPT=$(find ~/.claude/plugins/cache -name "search" -path "*/biomedical-search/*/scripts/*" -type f 2>/dev/null | head -1)
```

Then use the full path for all commands:
```bash
$BIOMEDICAL_SCRIPT "CAR-T cell therapy" 20
```

## API Key Setup Flow

When you run a search and receive `"setup_required": true`, follow this flow:

1. **Ask the user for their API key:**
   "To search biomedical databases, I need your Valyu API key. Get one free ($10 credits) at https://platform.valyu.ai"

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
# Search for cancer treatments
scripts/search "immunotherapy for triple negative breast cancer" 25

# Find COVID research and trials
scripts/search "COVID-19 vaccine long-term safety" 20

# Search for drug information
scripts/search "GLP-1 agonists for weight loss" 15

# Find rare disease research
scripts/search "gene therapy for sickle cell disease" 30
```

### Setup Command

```bash
scripts/search setup <api-key>
```

## Output Format

```json
{
  "success": true,
  "type": "biomedical_search",
  "query": "CAR-T cell therapy",
  "result_count": 20,
  "results": [
    {
      "title": "Title",
      "url": "https://...",
      "content": "Full content...",
      "source": "pubmed|biorxiv|medrxiv|clinical-trials|drug-labels",
      "relevance_score": 0.95,
      "images": ["https://example.com/figure1.jpg"]
    }
  ],
  "cost": 0.035
}
```

## Processing Results

### With jq

```bash
# Get titles
scripts/search "query" 20 | jq -r '.results[].title'

# Get URLs
scripts/search "query" 20 | jq -r '.results[].url'

# Extract full content
scripts/search "query" 20 | jq -r '.results[].content'

# Filter by source type
scripts/search "query" 20 | jq -r '.results[] | select(.source == "clinical-trials") | .title'
```

## Common Use Cases

### Clinical Research Planning

```bash
# Gather evidence for clinical study design
scripts/search "phase 2 trials checkpoint inhibitors melanoma" 50
```

### Drug Safety Assessment

```bash
# Search literature, labels, and trials for safety data
scripts/search "SGLT2 inhibitors cardiovascular safety" 40
```

### Treatment Protocol Development

```bash
# Find current practice and emerging approaches
scripts/search "pembrolizumab dosing regimens NSCLC" 30
```

### Medical Writing

```bash
# Comprehensive research for medical communications
scripts/search "JAK inhibitors rheumatoid arthritis efficacy" 60
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
