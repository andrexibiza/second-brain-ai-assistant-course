devlog
2025-12-29 12:21:19

# 1. Create the virtual environment

uv venv .venv-rag

# 2. Activate it (Windows uses 'Scripts', not 'bin')

. .\.venv-rag\Scripts\activate

# 3. Install dependencies

uv pip install -e .
