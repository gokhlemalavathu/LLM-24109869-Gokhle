# barbados-imf-llm

A Google Colab notebook that reads one IMF report with Gemini Flash models and tests the results against the report.

**Source:** IMF Country Report No. 26/155, *Barbados: 2026 Article IV Consultation and Request for a Stand-By Arrangement* (June 2026, 162 pages), in `data/`.

## Run it

Open `Barbados_SBA_Article_IV.ipynb` in Colab with `data/` and `cache/` in `/content`, then choose *Runtime > Run all*. The first cell installs `poppler-utils`. All 166 model requests are answered from `cache/replies.parquet`, so no API key is needed. To call the models again, add a Gemini key as the Colab secret `GEMINI_FREE_KEY` and delete that file.

The stored replies were recorded on Colab with poppler 24.02. Another poppler version can split the text slightly differently, which changes some prompts, and those calls then need a key.

## How it is built

Each finished result is published on a `blinker` signal. Three listeners keep it in memory, save it to `results/` and log it. Model replies are dataclasses read with `dacite`. The bank soundness table on page 49 exists only as an image, so the notebook sends that page image to Gemini for transcription.

Drafter: `gemini-3.5-flash`. Reviewer: `gemini-3.6-flash`. Embeddings: `sentence-transformers/multi-qa-MiniLM-L6-cos-v1` with a FAISS `IndexFlatL2`.

MIT licence, see `LICENSE`. The IMF report remains the property of the International Monetary Fund and is included only so the notebook can run.
