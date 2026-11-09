# Glean
A web-based OSINT monitor on war and conflict. It reads public reporting, groups articles into events, and shows each event with its sources and scores (relevance and confidence). The default client is New Zealand but the client profile (countries, allies, deployments, trade and shipping routes, Pacific interests) lives in a config file, so the client can be swapped by editing it.

The scope for v1 will be the Russia-Ukraine war as it is a high-volume conflict so it will give plenty of data to test properly. Future versions of the program may include more discrete events like maritime incidents, Pacific diplomacy, and regional tensions.

## Data
- **Stage A:** A frozen dataset of a few weeks of collected articles, so results are reproducible and fair to evaluate.
- **Stage B:** A scheduled fetch every few hours

## Pipeline
**Ingest > Extract > Cluster > Score > Display**

### Ingest
Every few hours, the monitor fetches new items from its sources (e.g. a news feed, GDELT, a UN humanitarian site, etc.) and saves them in a database.

### Extract
For each item, spaCy (a pre-built language tool) extracts the entities it finds.

For example, let's say a cargo ship is reported damaged near a port. For each item fetched by the program, spaCy will pull out the port's name, the country, the shipping company, the date, and more.

### Cluster
The program converts each item into a list of numbers that captures its meaning (an embedding) and groups items whose meanings are similar. Articles with shared entities and times would be closer together.

### Display
The web pages will show all the information on a topic as one event, with its sources and scores, in a list of the events gathered by the monitor.

#### Scores
- **Relevance** (1-5): how strongly it matches the NZ client profile.
- **Confidence** (1-5): how well it's corroborated, from the number of independent sources and their reliability.

## Tech
- Python
- spaCy for entities
- A sentence-embedding model for clustering
- SQLite for storage
- FastAPI or Streamlit as the web layer
- pytest with GitHub actions for testing & CI
- The pipeline is kept separate from the web app

### Evaluation
Hand-label about 50 items. Measure a hand-built baseline (rules and TF-IDF) against spaCy and embeddings, and write up where each fails.

### Future Ideas
- Daily brief
- Entity-link graph
- Second theatre or topic through config
- NZ-specific sources