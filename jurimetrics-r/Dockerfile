# Imagem pública de reprodutibilidade para os artigos R-only
# (Pricing-Non-Pecuniary-Harm, STJ-Moral-Damages-Jurimetrics,
#  Geography-of-Judicial-Delay, marine-fisheries-data-cube, fisheries-closures-brazil,
#  Port-Digitalization-Observatory, Port-Network-Resilience, transmissao-precos-leite-brasil,
#  tourism-gravity-brazil, pix-geography-brazil, maquinas-agricolas-*).
#
# Publicada como ghcr.io/valbergregory/jurimetrics-r:<R_VERSION>
# Cada repositório continua usando o seu próprio renv.lock; a imagem só garante
# R fixo + dependências de sistema + pacotes pesados pré-compilados.
#
# Uso local:
#   docker run --rm -it -v "$PWD":/work -w /work ghcr.io/valbergregory/jurimetrics-r:4.4.2 \
#     Rscript -e 'renv::restore(); targets::tar_make()'

ARG R_VERSION=4.4.2
FROM rocker/r-ver:${R_VERSION}

LABEL org.opencontainers.image.source="https://github.com/valbergregory/research-containers" \
      org.opencontainers.image.description="R + DuckDB + Arrow + renv/targets: ambiente reprodutível dos artigos de jurimetria e economia de valbergregory" \
      org.opencontainers.image.licenses="MIT"

ENV DEBIAN_FRONTEND=noninteractive \
    RENV_CONFIG_REPOS_OVERRIDE=https://p3m.dev/cran/__linux__/jammy/latest \
    TZ=America/Maceio

# Dependências de sistema (geoespacial, XML, SSL, fontes, compressão)
RUN apt-get update && apt-get install -y --no-install-recommends \
      git curl ca-certificates \
      libcurl4-openssl-dev libssl-dev libxml2-dev \
      libgdal-dev libgeos-dev libproj-dev libudunits2-dev \
      libfontconfig1-dev libharfbuzz-dev libfribidi-dev \
      libfreetype6-dev libpng-dev libtiff5-dev libjpeg-dev \
      zlib1g-dev libzstd-dev liblz4-dev \
      libnetcdf-dev libglpk-dev \
      pandoc \
    && rm -rf /var/lib/apt/lists/*

# Pacotes pesados pré-instalados (o renv.lock de cada repo ainda manda)
RUN install2.r --error --skipinstalled \
      renv targets tarchetypes \
      duckdb arrow DBI dbplyr \
      data.table dplyr tidyr readr stringr purrr lubridate \
      fixest sandwich lmtest modelsummary marginaleffects broom \
      sf h3jsr terra exactextractr ncdf4 \
      ggplot2 scales patchwork \
      jsonlite yaml testthat \
      knitr rmarkdown igraph httr2 \
    && rm -rf /tmp/downloaded_packages

# Métodos usados nos artigos de economia aplicada e oceanos
# (inferência causal em painel, controle sintético, séries temporais, heatwaves, APIs de dados BR)
RUN install2.r --error --skipinstalled \
      did HonestDiD Synth plm \
      tsibble fable fabletools \
      heatwaveR \
      sidrar WDI \
      gt texreg kableExtra \
    && rm -rf /tmp/downloaded_packages

WORKDIR /work
CMD ["R"]
