# ── Build stage ────────────────────────────────────────────────────────────────
FROM python:3.12-slim AS builder

WORKDIR /app

# Install uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /usr/local/bin/uv

# Copy dependency definition first (layer cache optimization)
COPY pyproject.toml ./

# Create venv and install dependencies (excluding the package itself for now)
RUN uv venv .venv && \
    uv pip install --python .venv/bin/python \
    fastmcp "notebooklm-py>=0.3.0" python-dotenv

# Copy source code
COPY src/ ./src/

# Install the package itself in editable mode
RUN uv pip install --python .venv/bin/python -e . --no-deps

# ── Runtime stage ────────────────────────────────────────────────────────────────
FROM python:3.12-slim AS runtime

WORKDIR /app

# Copy the virtual environment and source from builder
COPY --from=builder /app/.venv /app/.venv
COPY --from=builder /app/src /app/src
COPY --from=builder /app/pyproject.toml /app/pyproject.toml

# Activate the venv by updating PATH
ENV PATH="/app/.venv/bin:$PATH" \
    PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1

EXPOSE 8089



CMD ["python", "-m", "nblm_mcp_server", "--transport", "http", "--host", "0.0.0.0", "--port", "8089"]
