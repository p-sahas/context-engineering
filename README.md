This repository features a complete pipeline implementation demonstrated on `nawaloka.com` (Nawaloka Hospitals). It showcases a real-world scenario of scraping a corporate website, structuring the content, and building a multi-provider LLM chatbot over that specific knowledge base.

## Core Features

- **Document Ingestion Service (Web Crawling & Parsing)**
  - Automated deep crawling (configurable depth limits) using Playwright and BeautifulSoup4.
  - Extracts clean Markdown files and consolidates them into JSONL corpora.
  - Handles Javascript-rendered content and filters out short/irrelevant pages.
- **Advanced Text Chunking Service**
  - Configurable semantic and fixed-size text chunking strategies (Fixed, Sliding Window, Parent-Child, Late Chunking) to optimize embeddings and vector retrieval.
- **Advanced RAG Chat Services**
  - **Standard RAG (`RAGService`)**: Solid baseline standard RAG pipeline built with LangChain (LCEL) connected to ChromaDB.
  - **Cache-Augmented Generation (`CAGService`)**: Extremely fast semantic caching system. Identifies paraphrased FAQs and recent user history to return instant LLM responses and avoid redundant API calls.
  - **Corrective RAG (`CRAGService`)**: Answers queries with confidence scoring to evaluate the initial retrieval quality. If confidence is low, the context is expanded by modifying the retrieval strategy dynamically to provide a more accurate answer.
- **Multi-Provider LLM Integration**
  - Out-of-the-box support for OpenRouter (GPT-4o, Claude 3.5 Sonnet, Gemini, etc.) and direct OpenAI API integration for both chat and embedding tasks.
- **Vector Storage**
  - Built-in capabilities to generate embeddings and index them securely using a local ChromaDB instance.
- **Interactive Demos**
  - Jupyter Notebooks mapping out the complete pipeline: crawling (`01_crawl_nawaloka.ipynb`), embedding (`02_chunk_and_embed.ipynb`), and an interactive chat demo (`03_chat_with_web_demo.ipynb`).

## Project Structure

- `data/`: Directory for storing raw website JSONL data, generated Markdown pages, caching databases (`cag_cache`), and the `vectorstore` ChromaDB registry.
- `notebooks/`: Fully functional Jupyter notebooks for step-by-step pipeline execution and testing.
- `src/context_engineering/`: Core application framework containing Domain, Application, and Infrastructure layers.
  - `application/`: Contains `chat_service`, `ingest_documents_service`, and `evaluation_service`.
  - `domain/`: Contains models (Document, Chunk, Evidences, Queries) and business logic helpers.
  - `infrastructure/`: Defines the LLM bindings, embedding connections, and vector store configurations.
- `config/`: Pipeline configurations and hardcoded datasets (such as known `faqs.yaml`).

## Setup and Installation

1. Clone the repository to your local machine:
2. Ensure you have Python 3.9 or higher installed. 
3. Install the required dependencies:
```bash
pip install -r requirements.txt
```
*(If web crawling is required, Playwright browsers must also be installed via `python -m playwright install chromium`)*

4. Create a `.env` file in the root directory and configure your chosen provider API keys:
```env
OPENROUTER_API_KEY=your_key_here
# OR
OPENAI_API_KEY=your_key_here
```

## Running the Pipeline

The primary workflow is documented and runnable through the `notebooks` directory:

1. **`01_crawl_nawaloka.ipynb`**: Initiates the `NawalokaWebCrawler`, scrapes to the maximum desired depth, cleans HTML, and exports Markdown files representing the website.
2. **`02_chunk_and_embed.ipynb`**: Takes the JSONL corpus and evaluates different chunking methodologies before embedding them into the ChromaDB vector database.
3. **`03_chat_with_web_demo.ipynb`**: Initializes the conversational system. Allows users to test the Standard RAG, CAG (Cache response), and CRAG (Corrective response) services natively and interactively.

## License

This architecture is open-sourced and provided under the MIT License. See the [LICENSE](LICENSE) file for more details.
