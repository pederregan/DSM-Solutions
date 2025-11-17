# SCO Keyword Research Tool - n8n Workflow

A complete production-ready n8n workflow for automated Search-Content-Opportunity (SCO) keyword research.

## Overview

This workflow performs comprehensive keyword research including:
- SERP analysis
- Search intent classification
- Content gap identification
- Difficulty & opportunity scoring
- Semantic keyword clustering
- Related keyword discovery

## Workflow Components

### Nodes

1. **Webhook Trigger** - POST endpoint at `/sco-keyword-research`
2. **Validate Input** - Validates and cleans input data
3. **Fetch SERP** - Retrieves Google search results via SerpAPI
4. **Fetch Related Keywords** - Gets related searches and questions
5. **Process SERP Data** - Structures SERP results
6. **Process Related Keywords** - Structures related keyword data
7. **Merge Data** - Combines all collected data
8. **Gap Analysis** - Identifies content opportunities
9. **Classify Intent** - AI-powered search intent classification (OpenAI)
10. **Parse Intent Response** - Structures intent data
11. **Score Difficulty & Opportunity** - Calculates competitive scores
12. **Cluster Keywords** - AI-powered semantic clustering (OpenAI)
13. **Parse Clusters** - Structures cluster data
14. **Build Output** - Assembles final SCO report
15. **Respond** - Returns JSON response

## Setup Instructions

### Prerequisites

1. **n8n Instance** - Running n8n (cloud or self-hosted)
2. **SerpAPI Account** - Get API key from https://serpapi.com
3. **OpenAI Account** - Get API key from https://platform.openai.com

### Installation

1. **Import Workflow**
   - Open n8n
   - Click "Import from File"
   - Select `sco-keyword-research-workflow.json`

2. **Configure Credentials**

   **SerpAPI Credential:**
   - Go to Credentials → Add Credential
   - Select "SerpAPI"
   - Enter your SerpAPI key
   - Save as "SerpAPI"

   **OpenAI Credential:**
   - Go to Credentials → Add Credential
   - Select "OpenAI"
   - Enter your OpenAI API key
   - Save as "OpenAI"

3. **Activate Workflow**
   - Open the imported workflow
   - Click "Activate" in the top right

## Usage

### Request Format

**Endpoint:** `POST https://your-n8n-instance.com/webhook/sco-keyword-research`

**Body:**
```json
{
  "keyword": "cosmetic dentist NYC",
  "depth": 20
}
```

**Parameters:**
- `keyword` (required): The keyword or topic to research
- `depth` (optional): Number of SERP results to analyze (default: 20, max: 100)

### Example Request

```bash
curl -X POST https://your-n8n-instance.com/webhook/sco-keyword-research \
  -H "Content-Type: application/json" \
  -d '{
    "keyword": "cosmetic dentist NYC",
    "depth": 20
  }'
```

### Response Format

```json
{
  "keyword": "cosmetic dentist NYC",
  "search_intent": {
    "primary": "Commercial",
    "confidence_scores": {
      "informational": 20,
      "commercial": 70,
      "transactional": 8,
      "navigational": 2
    },
    "reasoning": "SERP dominated by service providers and comparison content"
  },
  "difficulty_score": 78,
  "opportunity_score": 45,
  "priority": "Medium",
  "score_breakdown": {
    "difficulty_factors": { ... },
    "opportunity_factors": { ... }
  },
  "serp_overview": [
    {
      "rank": 1,
      "title": "...",
      "snippet": "...",
      "link": "...",
      "domain": "..."
    }
  ],
  "content_gaps": {
    "missing_subtopics": [...],
    "thin_content_areas": [...],
    "repeated_competitor_angles": [...],
    "content_opportunities": [...]
  },
  "keyword_clusters": [
    {
      "topic_name": "...",
      "keywords": [...],
      "primary_keyword": "...",
      "secondary_keywords": [...],
      "recommended_page_type": "...",
      "content_angle": "..."
    }
  ],
  "related_keywords": [...],
  "metadata": {
    "search_depth": 20,
    "total_serp_results": 20,
    "total_related_keywords": 15,
    "total_clusters": 3,
    "timestamp": "2025-11-17T00:00:00.000Z"
  }
}
```

## Scoring Methodology

### Difficulty Score (0-100)
- **Domain Authority** (0-40): High-authority domains in SERP
- **Domain Diversity** (0-20): Unique vs dominated results
- **Content Saturation** (0-20): Repeated content angles
- **Intent Competition** (0-20): Transactional = harder

### Opportunity Score (0-100)
- **Content Gaps** (0-40): Missing topics and thin areas
- **Keyword Expansion** (0-25): Related keyword volume
- **Intent Alignment** (0-20): Content marketing suitability
- **Competition Weakness** (0-15): Inverse of difficulty

### Priority Classification
- **High**: Opportunity > 60 and combined score > 30
- **Medium**: Opportunity > 40 and combined score > 10
- **Low**: Everything else

## Cost Considerations

### SerpAPI Costs
- ~$0.002-0.005 per search request
- 2 requests per workflow execution
- Estimated: $0.004-0.01 per run

### OpenAI Costs
- Intent Classification: ~1,000 tokens (~$0.0015 with GPT-4o-mini)
- Keyword Clustering: ~2,000 tokens (~$0.003 with GPT-4o-mini)
- Estimated: $0.004-0.005 per run

**Total estimated cost per execution: ~$0.01-0.015**

## Customization

### Adjust Search Parameters
Edit the "Fetch SERP" node to modify:
- Geographic location (`gl` parameter)
- Language (`hl` parameter)
- Search engine (`engine` parameter)

### Change AI Model
Edit the OpenAI nodes to use:
- `gpt-4o` for better quality (higher cost)
- `gpt-3.5-turbo` for faster/cheaper results

### Modify Scoring Weights
Edit the "Score Difficulty & Opportunity" node to adjust:
- Factor weights
- Priority thresholds
- Score calculations

## Troubleshooting

### Common Issues

**"Authentication failed" error:**
- Verify SerpAPI and OpenAI credentials are correctly configured
- Check API key validity and quota

**"keyword is required" error:**
- Ensure POST body includes `keyword` field
- Check Content-Type header is `application/json`

**AI nodes returning errors:**
- Check OpenAI API quota and billing
- Verify model availability (gpt-4o-mini)
- Review temperature and max_tokens settings

**Empty or incomplete results:**
- Verify SerpAPI has data for your keyword/location
- Check depth parameter isn't exceeding available results
- Review node error logs in n8n

## Security Best Practices

1. **Never hard-code API keys** - Always use n8n credential system
2. **Rate limiting** - Consider adding rate limits to webhook
3. **Input validation** - Validate and sanitize all inputs
4. **HTTPS only** - Use HTTPS for webhook endpoint
5. **Authentication** - Add webhook authentication for production

## Support & Documentation

- n8n Documentation: https://docs.n8n.io
- SerpAPI Documentation: https://serpapi.com/docs
- OpenAI API Documentation: https://platform.openai.com/docs

## License

MIT
