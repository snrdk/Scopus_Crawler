# Scopus_Crawler
## Collection Process

The pipeline follows a two-stage collection strategy.

### 1. Search stage
* The Search stage performs broad record collection. It loads query keywords from `keywords.txt`, combines them with year-based query chunks, and submits them to Scopus using `ScopusSearch`.

* This step is designed to maximize collection efficiency. Instead of requesting detailed metadata for every paper immediately, it first saves only the core search results. Each unique paper is stored in `scopus_records.csv`, while keyword-to-paper relationships are stored in `scopus_matches.csv`.

* This design keeps the first-pass collection lightweight and reduces unnecessary API usage.

### 2. Enrichment stage
* The Enrichment stage performs record-level metadata expansion. It reads the previously collected EIDs from `scopus_records.csv` and uses `AbstractRetrieval` to fetch additional fields.

* This step is intended for metadata that is more expensive to collect, such as abstracts, references, affiliations, and funding information. Because it runs after the Search stage, the user can decide whether detailed enrichment is necessary before spending additional API quota.

### 3. Pipeline wrapper
* The Pipeline wrapper controls the overall execution flow. It provides a single entry point for the full workflow while keeping the Search stage and Enrichment stage logically separated.

* This makes the code easier to maintain, easier to debug, and easier to reuse for different collection settings.

cf. _Why separate Search and Enrichment?_

: Scopus search results can be collected relatively efficiently, but detailed record retrieval is more expensive and slower. Separating the workflow into two stages makes large-scale collection more efficient, reduces unnecessary API usage, and allows the user to enrich only the records that are actually needed.
