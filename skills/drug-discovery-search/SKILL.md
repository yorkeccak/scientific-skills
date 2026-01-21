---
name: drug-discovery-search
description: Search across ChEMBL, DrugBank, FDA drug labels, and Open Targets with natural language queries. Powered by Valyu's semantic search - no API param parsing needed, just ask in plain English. Returns comprehensive drug discovery data.
---

# Drug Discovery Search

Search across all major drug discovery databases (ChEMBL, DrugBank, FDA drug labels, Open Targets) simultaneously using natural language queries powered by Valyu's semantic search API.

## Why This Skill is Powerful

- **No API Parameter Parsing**: Just pass natural language queries directly - no need to construct complex search parameters
- **Semantic Search**: Understands the meaning of your query, not just keyword matching
- **Full-Text Access**: Returns complete compound, target, and drug information
- **Image Links**: Includes molecular structures and data visualizations
- **Comprehensive Coverage**: Search across ChEMBL, DrugBank, drug labels, and Open Targets simultaneously
- **Unified Results**: Get results from all drug discovery sources in a single query

## Requirements

1. Node.js 18+ (uses built-in fetch)
2. Valyu API key from https://platform.valyu.ai ($10 free credits)

## CRITICAL: Script Path Resolution

The `scripts/search` commands in this documentation are relative to this skill's installation directory.

Before running any command, locate the script using:

```bash
DRUG_DISCOVERY_SCRIPT=$(find ~/.claude/plugins/cache -name "search" -path "*/drug-discovery-search/*/scripts/*" -type f 2>/dev/null | head -1)
```

Then use the full path for all commands:
```bash
$DRUG_DISCOVERY_SCRIPT "JAK2 inhibitors" 20
```

## API Key Setup Flow

When you run a search and receive `"setup_required": true`, follow this flow:

1. **Ask the user for their API key:**
   "To search drug discovery databases, I need your Valyu API key. Get one free ($10 credits) at https://platform.valyu.ai"

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
# Search for kinase inhibitors
scripts/search "selective CDK4/6 inhibitors mechanism" 25

# Find GPCR ligands
scripts/search "dopamine D2 receptor antagonists" 20

# Search for target validation
scripts/search "BRAF V600E inhibitors melanoma" 15

# Find drug interactions
scripts/search "CYP3A4 substrates drug interactions" 30
```

### Setup Command

```bash
scripts/search setup <api-key>
```

## Output Format

```json
{
  "success": true,
  "type": "drug_discovery_search",
  "query": "JAK2 inhibitors",
  "result_count": 20,
  "results": [
    {
      "title": "Compound/Drug/Target Title",
      "url": "https://...",
      "content": "Full data including compounds, targets, mechanisms...",
      "source": "chembl|drugbank|drug-labels|open-targets",
      "relevance_score": 0.95,
      "images": ["https://example.com/structure.png"]
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

# Filter by source
scripts/search "query" 20 | jq -r '.results[] | select(.source == "chembl") | .title'
```

## Common Use Cases

### Target Identification

```bash
# Find validated targets and compounds
scripts/search "BTK inhibitors for autoimmune diseases" 50
```

### Lead Optimization

```bash
# Search SAR and compound data
scripts/search "EGFR inhibitors blood-brain barrier penetration" 40
```

### Drug Repurposing

```bash
# Find repurposing opportunities
scripts/search "mTOR inhibitors cancer and aging" 30
```

### Safety Assessment

```bash
# Gather safety and interaction data
scripts/search "tyrosine kinase inhibitors cardiac toxicity" 60
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
