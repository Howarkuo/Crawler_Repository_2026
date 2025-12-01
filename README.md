# Crawler Repository 2026

This repository contains Python scripts for scraping and retrieving biological and pharmaceutical data from multiple sources, including **PubMed Central (PMC)**, **UniProt**, and **MDPI journals**. The repository includes both API-based and non-API web scraping approaches.

---

## Table of Contents

- [Directory Structure](#directory-structure)
- [Requirements](#requirements)
- [PMC Full-Text Fetching (API)](#pmc-full-text-fetching-api)
- [UniProt Data Fetching (API)](#uniprot-data-fetching-api)
- [MDPI Articles Scraping (No API)](#mdpi-articles-scraping-no-api)
  - [JSON/XML Requests](#jsonxml-requests)
  - [Playwright Browser Automation](#playwright-browser-automation)
- [License](#license)

---

## Directory Structure
```
Crawler_Repository_2026/
│
├─ API_provided/
│ ├─ pmc_jsonl.py # Fetch full text from PMC using Entrez E-utilities API
│ └─ uniprot.py # Fetch protein data from UniProt and EBI Proteins API
│
└─ NoAPI/
├─ JSON_XML.py # Scrape MDPI journals via AJAX + XML requests
└─ playwright.py # Scrape MDPI journals using Playwright browser automation
```


---

## Requirements

Install required packages via pip/ poetry:

```bash
pip install requests beautifulsoup4 lxml prettytable playwright
```
For Playwright, you may also need to install browser binaries:

```
playwright install
```
## PMC Full-Text Fetching (API)
File: API_provided/pmc_jsonl.py

Fetches full text from PubMed Central (PMC) using Entrez E-utilities API. Outputs results in JSON Lines format (.jsonl).

**Usage**:

```bash
python pmc_jsonl.py
```
**Features**:

- Fetches full XML text for a list of PMCIDs.

- Parses `<body>` and `<p>` tags to extract full paragraphs.
- Saves pmcid, title, and text into a JSONL file.

**Notes**:

Use PMCID (not PMID) for efetch.

Includes polite delays (time.sleep(1)) to avoid API rate limits.
---

## UniProt Data Fetching (API)
File: API_provided/uniprot.py

Fetches protein information from UniProt REST API and EBI Proteins API.

**Usage**:

```bash
python uniprot.py
```
**Features**:

- Retrieves full JSON metadata for a given UniProt accession.

- Extracts commentType values from the comments field.

- Extracts all features of the protein and removes duplicates.

- Outputs results in console with PrettyTable formatting or JSON.

**Notes**

Supports UniProtKB REST endpoints.

Can be extended for batch queries.

---
## MDPI Articles Scraping (No API)
Some MDPI journals do not provide public bulk APIs. This repository includes two approaches to scrape article data.

JSON/XML Requests
File: NoAPI/JSON_XML.py

Uses MDPI's internal AJAX search endpoint (/search/ajax) to fetch article listings.

Retrieves raw XML for each article.

Parses article titles and paragraphs and saves in JSON Lines.

**Usage**:

```bash
python JSON_XML.py
```
**Features**:

- Fast and clean scraping.

- Can limit the number of articles for testing (MAX_ARTICLES_TO_FETCH).

- Includes browser-like headers to mimic requests.

**Notes**:

May be blocked by MDPI's bot detection (403 Forbidden).

Polite delay between requests (time.sleep(1)).

---

## Playwright Browser Automation
File: NoAPI/playwright.py

Uses a headless browser to render pages fully before scraping.

Parses HTML content for article titles, authors, PDF links, meta title, meta abstract, and first paragraph.

**Usage**:

```bash
python playwright.py
```
**Features**:

- Simulates a real user visiting the website.

- Extracts structured content including first paragraph of the introduction.

- Can handle dynamically loaded content.

**Notes**:

Timeout may occur if pages are slow to load.

Can increase page.wait_for_selector timeout or run in non-headless mode for debugging.

> [!NOTE]
> License
> This repository is for academic and research purposes. Please cite appropriately when using or modifying these scripts.
> Author: Howard Kuo (2025)
> Contact: howard900126@gmail.com
