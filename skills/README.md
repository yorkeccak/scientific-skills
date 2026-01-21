# Scientific Skills for Claude Code

Complete collection of scientific literature and drug discovery search skills powered by Valyu's semantic search API.

## Skills Overview

### Individual Data Sources

| Skill | Description | Source |
|-------|-------------|--------|
| **arxiv-search** | Search arXiv preprints (physics, math, CS, quant bio) | valyu/valyu-arxiv |
| **biorxiv-search** | Search bioRxiv biological preprints | valyu/valyu-biorxiv |
| **medrxiv-search** | Search medRxiv medical preprints | valyu/valyu-medrxiv |
| **chembl-search** | Search ChEMBL bioactive molecules database | valyu/valyu-chembl |
| **drugbank-search** | Search DrugBank comprehensive drug database | valyu/valyu-drugbank |
| **clinical-trials-search** | Search ClinicalTrials.gov | valyu/valyu-clinical-trials |
| **drug-labels-search** | Search FDA drug labels | valyu/valyu-drug-labels |
| **open-targets-search** | Search Open Targets drug-disease associations | valyu/valyu-open-targets |
| **patents-search** | Search global patent database | valyu/valyu-patents |
| **pubmed-search** | Search PubMed biomedical literature | valyu/valyu-pubmed |

### Aggregated Skills

| Skill | Description | Sources |
|-------|-------------|---------|
| **literature-search** | Search all literature sources simultaneously | PubMed, arXiv, bioRxiv, medRxiv |
| **biomedical-search** | Search all biomedical databases simultaneously | PubMed, bioRxiv, medRxiv, ClinicalTrials, Drug Labels |
| **drug-discovery-search** | Search all drug discovery databases simultaneously | ChEMBL, DrugBank, Drug Labels, Open Targets |

## Quick Start

All skills follow the same pattern:

```bash
# Find the script
SKILL_SCRIPT=$(find ~/.claude/plugins/cache -name "search" -path "*/<skill-name>/*/scripts/*" -type f 2>/dev/null | head -1)

# Setup API key (first time only)
$SKILL_SCRIPT setup <your-valyu-api-key>

# Search
$SKILL_SCRIPT "your natural language query" 20
```

## Features

- **Natural Language Queries**: No API parameter parsing needed
- **Semantic Search**: Understands query meaning, not just keywords
- **Full-Text Access**: Complete article/document content
- **Image Links**: Includes figures and molecular structures
- **Zero Dependencies**: Uses Node.js built-in fetch
- **Unified Configuration**: Single API key for all skills

## Getting Your API Key

Get a free Valyu API key ($10 credits) at https://platform.valyu.ai

## Directory Structure

Each skill contains:
```
<skill-name>/
├── SKILL.md           # Complete documentation
└── scripts/
    ├── search         # Bash wrapper (executable)
    └── search.mjs     # Node.js implementation
```

## Use Cases

### Academic Research
- Literature reviews across multiple sources
- Finding recent preprints and publications
- Cross-disciplinary research

### Drug Discovery
- Target identification and validation
- Lead compound optimization
- Safety and interaction assessment

### Clinical Research
- Treatment protocol development
- Clinical trial planning
- Evidence gathering

### Competitive Intelligence
- Patent landscape analysis
- Prior art search
- Technology trend monitoring
