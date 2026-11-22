# DAIL-SQL Backend Server

A FastAPI backend for the NL2SQL VS Code extension, built on **DAIL-SQL**. It supports schema linking, few-shot example retrieval, prompt construction, SQL validation, and corrective prompting for natural-language-to-SQL workflows.

The backend and the VS Code extension are maintained in separate repositories. The extension provides the interface, database connections, SQL execution, and GitHub Copilot or Kimi model integration. The backend handles DAIL-SQL processing and validation.

## Workflow

```text
Natural-language question (VS Code extension)
              |
              v
    DAIL-SQL backend
    - Schema linking
    - Few-shot retrieval
    - Prompt construction
              |
              v
    GitHub Copilot / Kimi
    (SQL generation in the extension)
              |
              v
    Backend SQL validation
    and optional correction
              |
              v
    Editable SQL and optional execution
    in the VS Code extension
```

## Prerequisites

- **Python 3.8** (the original project's target version)
- **Conda** (recommended)
- **Java** for Stanford CoreNLP

## Installation

Create and activate a Python environment from the backend repository root:

```bash
conda create -n DAIL-SQL python=3.8
conda activate DAIL-SQL
python -m pip install --upgrade pip
pip install -r requirements.txt
```

All Python dependencies are listed in a single `requirements.txt`. The separate `requirements-api.txt` is no longer needed after replacing the original requirements file with the merged version.

> **Compatibility note:** The dependency list consolidates the existing project requirements; it is not a fully pinned or independently tested environment lockfile. In particular, PyTorch and TorchText versions must be mutually compatible with the selected Python version.

### Stanford CoreNLP

Download Stanford CoreNLP from the [official project](https://stanfordnlp.github.io/CoreNLP/) and place the extracted `stanford-corenlp-full-2018-10-05` directory at:

```text
third_party/stanford-corenlp-full-2018-10-05/
```

Start CoreNLP in a separate terminal:

```bash
cd third_party/stanford-corenlp-full-2018-10-05
java -mx4g -cp "*" edu.stanford.nlp.pipeline.StanfordCoreNLPServer -port 9000
```

Then return to the backend repository root in the terminal where you will start the Python server.

## Configuration

Create a local `.env` file based on the supplied example:

```powershell
Copy-Item .env.example .env
```

The backend can read `OPENAI_API_KEY` for its own model-driven generation path. When using GitHub Copilot or Kimi through the VS Code extension, model authentication is handled by the extension instead.

The server currently starts on port `8000`, as configured in `api_server.py`. Do not commit real API keys or the `.env` file to Git.

## Run the server

With the Python environment active and CoreNLP running, start the backend from the repository root:

```bash
python api_server.py
```

The default server address is `http://localhost:8000`. Start the backend before using server-dependent features in the VS Code extension.

## Notes

This is a research/development implementation. The combined dependency file was assembled from the existing requirements files and additional packages referenced by the backend; a clean end-to-end installation and runtime test has not been performed as part of this documentation update.

## Attribution

This project builds on the DAIL-SQL approach. See the [original DAIL-SQL implementation](https://github.com/BeachWang/DAIL-SQL). Preserve the repository's existing license and applicable third-party attributions.
