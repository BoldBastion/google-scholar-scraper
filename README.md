[Google Scholar Scraper](https://apify.com/lulzasaur/google-scholar-scraper?fpr=data)

Scrape Google Scholar search results, paper citations, abstracts, and author profiles — no API key required.

Google Scholar has no official API, making this scraper invaluable for researchers, academics, bibliometricians, and developers who need structured access to academic publication data.

## Features

- **Search mode**: Search papers by keyword with title, authors, journal, year, abstract, citation count, and PDF links
- **Author profile mode**: Fetch an author's complete profile including h-index, i10-index, total citations, and full publication list
- Year range filtering (`yearFrom` / `yearTo`)
- Sort by relevance or date
- Automatic pagination (10 results per page, up to 1000)
- CAPTCHA detection with automatic retry (30–60s wait, up to 3 attempts)
- Residential proxy support (required — Google blocks datacenter IPs)

## Input

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `mode` | select | `search` | `search` or `authorProfile` |
| `query` | string | `machine learning` | Search keywords (search mode) |
| `authorId` | string | — | Google Scholar user ID (author profile mode) |
| `yearFrom` | integer | — | Filter papers from this year |
| `yearTo` | integer | — | Filter papers up to this year |
| `sortBy` | select | `relevance` | `relevance` or `date` |
| `limit` | integer | `10` | Max results (1–1000) |
| `proxyConfiguration` | object | Apify Residential | Proxy settings |

### Finding an Author ID

Go to any Google Scholar author profile page. The URL will look like:

```
https://scholar.google.com/citations?user=XXXXXXXXXXX&hl=en
```

Copy the value after `user=` — that is the author ID.

## Output

### Search Mode

```
{
  "title": "Attention Is All You Need",
  "url": "https://arxiv.org/abs/1706.03762",
  "authors": ["A Vaswani", "N Shazeer", "N Parmar"],
  "source": "Advances in neural information processing systems",
  "year": 2017,
  "abstract": "The dominant sequence transduction models are based on complex recurrent...",
  "citationCount": 98432,
  "pdfUrl": "https://arxiv.org/pdf/1706.03762",
  "allVersionsUrl": "https://scholar.google.com/scholar?cluster=...",
  "relatedArticlesUrl": "https://scholar.google.com/scholar?q=related:...",
  "scholarUrl": "https://arxiv.org/abs/1706.03762",
  "searchQuery": "transformer attention mechanism",
  "scrapedAt": "2024-01-15T10:23:45.123Z"
}
```

### Author Profile Mode

```
{
  "name": "Geoffrey Hinton",
  "affiliation": "University of Toronto",
  "hIndex": 155,
  "i10Index": 322,
  "totalCitations": 783210,
  "publications": [
    {
      "title": "Deep learning",
      "url": "https://scholar.google.com/citations?view_op=view_citation&...",
      "authorsSource": "Y LeCun, Y Bengio, G Hinton - nature, 2015",
      "year": 2015,
      "citationCount": 62450
    }
  ],
  "scholarUrl": "https://scholar.google.com/citations?user=JicYPdAAAAAJ&hl=en",
  "scrapedAt": "2024-01-15T10:23:45.123Z"
}
```

## Use Cases

- **Literature reviews**: Collect papers on a topic with citation counts to identify seminal works
- **Bibliometric analysis**: Track citation trends, prolific authors, top journals
- **Research monitoring**: Watch for new papers on keywords you care about
- **Academic recruiting**: Find researchers by h-index and publication record
- **Competitive intelligence**: Monitor competitor research output and emerging tech areas
- **Grant writing**: Find citation stats and related work for proposals

## Example Inputs

### Search for recent AI safety papers

```
{
  "mode": "search",
  "query": "AI alignment safety",
  "yearFrom": 2022,
  "sortBy": "date",
  "limit": 50
}
```

### Get Geoffrey Hinton's author profile

```
{
  "mode": "authorProfile",
  "authorId": "JicYPdAAAAAJ",
  "limit": 100
}
```

### Top cited NLP papers ever

```
{
  "mode": "search",
  "query": "natural language processing BERT transformer",
  "sortBy": "relevance",
  "limit": 100
}
```

## Pricing

This actor uses Pay Per Event (PPE) pricing — **$0.005 per result** scraped.

- 100 papers = $0.50
- 500 papers = $2.50
- 1,000 papers = $5.00

## Notes

- Google Scholar enforces rate limits. The scraper adds 2–6 second delays between pages to stay within limits.
- If a CAPTCHA is detected, the scraper waits 30–60 seconds and retries up to 3 times before skipping.
- Residential proxies are mandatory. The default configuration uses Apify's residential proxy pool.
- Google Scholar shows a maximum of ~100 results per search query through normal pagination. For larger datasets, split your query into multiple narrower searches.

## Related Actors

- [Google Search Scraper](https://apify.com/apify/google-search-scraper)
- [Google News Scraper](https://apify.com/apify/google-news-scraper)