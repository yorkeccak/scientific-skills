---
name: drug-labels-search
description: Search FDA drug labels and prescribing information with natural language queries. Powered by Valyu's semantic search - no API param parsing needed, just ask in plain English. Returns official labeling, warnings, and usage information.
---

# Drug Labels Search

Search the complete FDA drug labels database including prescribing information, warnings, and official labeling using natural language queries powered by Valyu's semantic search API.

## Why This Skill is Powerful

- **No API Parameter Parsing**: Just pass natural language queries directly - no need to construct complex search parameters
- **Semantic Search**: Understands the meaning of your query, not just keyword matching
- **Full-Text Access**: Returns complete drug label information including indications, dosing, warnings, and adverse reactions
- **Image Links**: Includes label images when available
- **Comprehensive Coverage**: Access to all FDA drug label data

## Requirements

1. Node.js 18+ (uses built-in fetch)
2. Valyu API key from https://platform.valyu.ai ($10 free credits)

## CRITICAL: Script Path Resolution

The `scripts/search` commands in this documentation are relative to this skill's installation directory.

Before running any command, locate the script using:

```bash
DRUG_LABELS_SCRIPT=$(find ~/.claude/plugins/cache -name "search" -path "*/drug-labels-search/*/scripts/*" -type f 2>/dev/null | head -1)
```

Then use the full path for all commands:
```bash
$DRUG_LABELS_SCRIPT "ibuprofen warnings" 15
```

## API Key Setup Flow

When you run a search and receive `"setup_required": true`, follow this flow:

1. **Ask the user for their API key:**
   "To search FDA drug labels, I need your Valyu API key. Get one free ($10 credits) at https://platform.valyu.ai"

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
# Search for warnings
scripts/search "warfarin black box warnings" 15

# Find dosing information
scripts/search "metformin dosing for renal impairment" 20

# Search for indications
scripts/search "pembrolizumab approved indications" 10

# Find contraindications
scripts/search "SSRI contraindications pregnancy" 12
```

### Setup Command

```bash
scripts/search setup <api-key>
```

## Output Format

```json
{
  "success": true,
  "type": "drug_labels_search",
  "query": "ibuprofen warnings",
  "result_count": 10,
  "results": [
    {
      "title": "Drug Label Title",
      "url": "https://fda.gov/...",
      "content": "Label content, warnings, dosing...",
      "source": "drug-labels",
      "relevance_score": 0.95,
      "images": ["https://example.com/label.jpg"]
    }
  ],
  "cost": 0.025
}
```

## Processing Results

### With jq

```bash
# Get drug names
scripts/search "query" 10 | jq -r '.results[].title'

# Get URLs
scripts/search "query" 10 | jq -r '.results[].url'

# Extract full content
scripts/search "query" 10 | jq -r '.results[].content'
```

## Common Use Cases

### Safety Information

```bash
# Find safety data
scripts/search "anticoagulant bleeding risk warnings" 50
```

### Prescribing Guidance

```bash
# Search for dosing
scripts/search "pediatric dosing guidelines for antibiotics" 20
```

### Drug Interactions

```bash
# Find interaction data
scripts/search "CYP450 drug interaction warnings" 15
```

### Regulatory Information

```bash
# Search for approval data
scripts/search "accelerated approval indications oncology" 25
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
