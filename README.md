# Scientific Skills for Claude

<video src="demo.mp4" controls></video>

Natural language scientific literature search skills with semantic semantic over PubMed, arXiv, ChEMBL, DrugBank, bioRxiv, medRxiv, clinical trials, and more with zero API complexity.

## Why These Skills Are Powerful

- **No API Parameter Parsing**: Just pass natural language queries - no complex search syntax or parameters needed
- **Semantic Search**: Understands meaning and context, not just keyword matching
- **Full-Text Access**: Returns complete article content, not just abstracts
- **Image Links**: Includes figures and images from papers
- **Comprehensive Coverage**: Access to ALL scientific literature across multiple databases

## Available Skills

### Individual Data Sources (9 skills)

| Skill | Data Source | Coverage |
|-------|-------------|----------|
| `pubmed-search` | PubMed | Biomedical and life sciences literature |
| `arxiv-search` | arXiv | Physics, mathematics, computer science, quantitative biology preprints |
| `biorxiv-search` | bioRxiv | Biology preprints |
| `medrxiv-search` | medRxiv | Medical and health sciences preprints |
| `chembl-search` | ChEMBL | Bioactive molecules with drug-like properties |
| `drugbank-search` | DrugBank | Comprehensive drug and drug target database |
| `clinical-trials-search` | ClinicalTrials.gov | Clinical trials registry |
| `drug-labels-search` | FDA Drug Labels | Official FDA drug information |
| `open-targets-search` | Open Targets | Drug targets and disease associations |
| `patents-search` | Patent Databases | Global patent filings |

### Aggregated Skills (3 skills)

| Skill | Combined Sources | Best For |
|-------|------------------|----------|
| `literature-search` | PubMed + arXiv + bioRxiv + medRxiv | General scientific literature review |
| `biomedical-search` | PubMed + bioRxiv + medRxiv + ClinicalTrials + Drug Labels | Medical and clinical research |
| `drug-discovery-search` | ChEMBL + DrugBank + Drug Labels + Open Targets | Drug discovery and development |

## Installation

**Quick install via npx:**

```bash
npx skills i yorkeccak/scientific-skills
```

**Or browse all skills:** [skills.sh/yorkeccak/scientific-skills](https://skills.sh/yorkeccak/scientific-skills)

### Manual Installation

```bash
# Clone the repository
cd ~/dev
git clone https://github.com/yorkeccak/scientific-skills.git

# Add as a local plugin
/plugin add ~/dev/scientific-skills
```

## Quick Start

### 1. Get Your API Key

Get a free API key with $10 credits at [platform.valyu.ai](https://platform.valyu.ai)

### 2. First Use (Automatic Setup)

Just start using any skill! Claude will automatically:
1. Detect that no API key is configured
2. Ask you to paste your API key
3. Save it to `~/.valyu/config.json`
4. Run your search

```
You: Search PubMed for CRISPR advances in 2024
Claude: "To search PubMed, I need your Valyu API key from https://platform.valyu.ai"
You: val_abc123...
Claude: [saves key and runs search automatically]
```

### 3. Manual Setup (Optional)

If you prefer to set up manually:

**Environment Variable** (recommended):
```bash
# For Zsh (macOS default)
echo 'export VALYU_API_KEY="your-api-key-here"' >> ~/.zshrc
source ~/.zshrc

# For Bash
echo 'export VALYU_API_KEY="your-api-key-here"' >> ~/.bashrc
source ~/.bashrc
```

**Config File**:
```bash
mkdir -p ~/.valyu
echo '{"apiKey": "your-api-key-here"}' > ~/.valyu/config.json
```


## Output Format

All skills return consistent JSON output:

```json
{
  "success": true,
  "type": "pubmed_search",
  "query": "your query",
  "result_count": 10,
  "results": [
    {
      "title": "Article Title",
      "url": "https://...",
      "content": "Full article text with figures...",
      "source": "pubmed",
      "relevance_score": 0.95,
      "images": ["https://example.com/figure1.jpg"]
    }
  ],
  "cost": 0.025
}
```

## Processing Results with jq

```bash
# Get article titles
scripts/search "query" 10 | jq -r '.results[].title'

# Get URLs
scripts/search "query" 10 | jq -r '.results[].url'

# Extract content
scripts/search "query" 10 | jq -r '.results[].content'

# Filter by relevance score
scripts/search "query" 20 | jq '.results[] | select(.relevance_score > 0.9)'
```

## Common Use Cases

### Literature Review

Search across all scientific literature:

```bash
cd ~/dev/scientific-skills/skills/literature-search
scripts/search "epigenetic modifications in cancer" 50
```

### Drug Discovery

Find compounds and targets:

```bash
cd ~/dev/scientific-skills/skills/drug-discovery-search
scripts/search "kinase inhibitors for melanoma" 30
```

### Clinical Research

Find trials and patient outcomes:

```bash
cd ~/dev/scientific-skills/skills/biomedical-search
scripts/search "checkpoint inhibitor combinations trials" 40
```

### Patent Research

Search for prior art:

```bash
cd ~/dev/scientific-skills/skills/patents-search
scripts/search "CRISPR delivery methods" 25
```

### Gene Function Research

Understand gene roles:

```bash
cd ~/dev/scientific-skills/skills/pubmed-search
scripts/search "TP53 tumor suppression mechanisms" 20
```

## Requirements

- Node.js 18+ (uses built-in fetch API)
- Valyu API key from [platform.valyu.ai](https://platform.valyu.ai)
- No npm packages required - zero external dependencies

## Technical Details

### Architecture

Each skill consists of:
```
skill-name-search/
├── SKILL.md              # Skill documentation
└── scripts/
    ├── search            # Bash wrapper
    └── search.mjs        # Node.js implementation
```

### API Integration

- **Base URL**: `https://api.valyu.ai/v1`
- **Endpoint**: `/search`
- **Authentication**: X-API-Key header
- **Method**: POST with JSON body

### Script Structure

All scripts use Node.js built-in `fetch()` for HTTP requests:
- No external dependencies
- Simple, auditable code
- Fast execution
- Cross-platform compatible

## Error Handling

All skills return JSON with a `success` field:

```json
{
  "success": false,
  "error": "Error message"
}
```

Exit codes:
- `0` - Success
- `1` - Error (check JSON for details)

## FAQ

### Do I need a separate API key for each skill?

No! One Valyu API key works for all skills. The skills just query different data sources.

### How much does it cost?

New accounts get $10 in free credits. After that, costs are minimal (typically $0.01-0.05 per search depending on results).

### Can I use these skills outside of Claude?

Yes! The scripts are standalone CLI tools. Run them directly:

```bash
cd ~/dev/scientific-skills/skills/pubmed-search
./scripts/search "your query" 10
```

### What's the difference between individual and aggregated skills?

Individual skills search a single data source (e.g., only PubMed). Aggregated skills search multiple related sources simultaneously for more comprehensive results.

### How is this different from regular PubMed search?

- **Semantic understanding**: Understands query meaning, not just keywords
- **Full-text access**: Returns complete articles, not just abstracts
- **Natural language**: No need to learn search syntax
- **Unified interface**: Same approach across all scientific databases

### Can I customize the search parameters?

The skills use natural language queries by design to eliminate API complexity. For advanced filtering, you can:
1. Be more specific in your natural language query
2. Adjust the `maxResults` parameter
3. Filter results using jq after receiving them

### Are the results cached?

No. Each search queries Valyu's API in real-time for the freshest results.

## Comparison to Traditional Search

| Feature | Traditional Search | Valyu Skills |
|---------|-------------------|--------------|
| Query Format | Complex syntax, Boolean operators | Natural language |
| Search Type | Keyword matching | Semantic understanding |
| Content Returned | Abstracts, metadata | Full-text with images |
| API Complexity | High - learn each API | Zero - same interface |
| Setup Time | Hours per database | Minutes for all |
| Multiple Sources | Write separate integrations | Single unified approach |

## Contributing

This is a local development repository. For public contributions, please visit [github.com/valyu-network/scientific-skills](https://github.com/valyu-network/scientific-skills).

## Support

- **Valyu API Documentation**: [docs.valyu.ai](https://docs.valyu.ai)
- **API Status**: [status.valyu.ai](https://status.valyu.ai)
- **Support Email**: [email protected]
- **Platform**: [platform.valyu.ai](https://platform.valyu.ai)

## License

MIT License - See individual skill files for details.

## Acknowledgments

Built with [Valyu](https://valyu.ai) - The ingress layer for AI.

---

**Ready to start searching?**

1. Get your free API key: [platform.valyu.ai](https://platform.valyu.ai)
2. Pick a skill from the table above
3. Ask Claude to search in natural language

That's it! No complex setup, no API documentation to read, no syntax to learn. Just ask Claude to search, and it works.
