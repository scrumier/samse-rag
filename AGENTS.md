# pdf-rag

Ask a folder of PDFs a question in plain language, get the answer back with the document it came from.

## When asked to set this up

Follow this sequence, in order, without improvising.

1. Check `uv`: `uv --version`. If it is missing, install it with
   `curl -LsSf https://astral.sh/uv/install.sh | sh`, then say so.

2. `cp .env.example .env`.
3. Ask the user for their OpenRouter key and write it into `.env` under
   `OPENROUTER_API_KEY`. Never invent one, never put it in a file tracked by
   git, and do not print it back in plain text.
4. `make setup`. The first run downloads an embedding model of several
   hundred megabytes: warn the user before, and do not interrupt it.
5. `make test`, to confirm the install holds.
6. Give the user the one command that matters: `make run`, then
   http://127.0.0.1:5050. Six demo PDFs are already indexed, so a question can
   be asked immediately.
7. For their own documents, point `DOCS_PATH` at their folder in `.env` and
   run `make run` again.

Do nothing else unless asked: no refactoring, no added dependency, no model
change, no commit and no push.

## What actually breaks

- A scanned PDF yields no text. It needs OCR first, and this repo does none.
  Say so instead of hunting for why the answers are empty.
- Port 5050 may be taken. `PORT=5060 make run` changes it.
- The vector store lives in `chroma_db/`. Deleting it forces a full reindex on
  the next start, which is slow but harmless.

## Shape of the repo

`rag/` holds the code, `demo_docs/` the sample PDFs, `tests/` the suite.
An MCP server sits in `rag/mcp_server.py` for querying the corpus from an MCP
client instead of the browser.
