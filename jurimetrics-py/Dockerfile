# Imagem pública de reprodutibilidade para os artigos Python-only
# (binding-precedents-brazil, abusive-litigation-jurimetrics, genai-legal-drafting-benchmark,
#  e a camada Python de Port-Network-Resilience).
#
# Publicada como ghcr.io/valbergregory/jurimetrics-py:<PY_VERSION>
# Cada repositório continua usando o seu próprio pyproject.toml/uv.lock; a imagem só garante
# Python fixo + uv + dependências de sistema + pacotes pesados pré-instalados (sem torch/transformers:
# os modelos de embeddings entram pelo uv.lock de quem precisa).
#
# Uso local:
#   docker run --rm -it -v "$PWD":/work -w /work ghcr.io/valbergregory/jurimetrics-py:3.12 \
#     sh -c 'uv sync --frozen && uv run pytest'

ARG PY_VERSION=3.12
FROM python:${PY_VERSION}-slim-bookworm

LABEL org.opencontainers.image.source="https://github.com/valbergregory/research-containers" \
      org.opencontainers.image.description="Python + uv + DuckDB + Polars + Arrow + scikit-learn: ambiente reprodutível dos artigos de jurimetria (Python-only) de valbergregory" \
      org.opencontainers.image.licenses="MIT"

ENV DEBIAN_FRONTEND=noninteractive \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    UV_LINK_MODE=copy \
    UV_PROJECT_ENVIRONMENT=/opt/venv \
    PATH="/opt/venv/bin:${PATH}" \
    TZ=America/Maceio

# Dependências de sistema (compilação eventual, SSL, XML, PDF)
RUN apt-get update && apt-get install -y --no-install-recommends \
      git curl ca-certificates build-essential \
      libssl-dev libxml2-dev libxslt1-dev \
      poppler-utils \
    && rm -rf /var/lib/apt/lists/*

# uv (gerenciador de ambientes usado em todos os repositórios Python-only)
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# Pacotes pesados pré-instalados num venv base (o uv.lock de cada repo ainda manda)
RUN uv venv /opt/venv && uv pip install --python /opt/venv/bin/python \
      duckdb polars pyarrow pandas numpy scipy \
      scikit-learn networkx statsmodels \
      httpx requests beautifulsoup4 lxml pypdf \
      pydantic pyyaml openpyxl tqdm \
      pytest pytest-cov ruff doit \
    && rm -rf /root/.cache

WORKDIR /work
CMD ["python"]
