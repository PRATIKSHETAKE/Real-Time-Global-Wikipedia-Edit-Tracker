# Real-Time Wikipedia Edit Streaming Pipeline

## Overview
This project is a real-time Streaming ETL (Extract, Transform, Load) pipeline that taps directly into Wikimedia's live Server-Sent Events (SSE) firehose. It continuously captures global Wikipedia edits as they happen, parses the high-velocity JSON payloads on the fly, and structures the data into a relational database for live analytics.

## Architecture
1. **Extract (Stream):** Establishes a persistent, open connection to the Wikimedia `recentchange` API stream using Python's `requests` library, bypassing traditional batch processing for millisecond-latency data ingestion.
2. **Transform (In-Memory):** Catches continuous nested JSON packets, extracting key metrics such as user type (bot vs. human), target page, domain, and character delta (bytes added/removed).
3. **Load (Continuous Append):** Instantly loads the parsed, structured data into a local SQLite database (`live_wikipedia.db`) without interrupting the data stream.
4. **Analyze:** Utilizes `pandas` and SQL to query the live database, generating insights such as bot-to-human edit ratios and identifying heavily targeted articles in real-time.

## Tech Stack
* **Language:** Python 3
* **Streaming Protocol:** Server-Sent Events (SSE)
* **Data Processing:** JSON parsing, Pandas
* **Database & Querying:** SQLite, SQL

## How to Run
1. Clone the repository to your local machine.
2. Ensure you have the required libraries installed (`pip install requests pandas`).
3. Run the streaming script to open the firehose and populate the database:
   ```bash
   python wiki_stream_pipeline.py
