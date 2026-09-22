# research-containers

Reproducible container images for my research repositories (jurimetrics, law and
economics, applied economics, marine fisheries). Each article repository keeps its own
`renv.lock`; these images only pin the R version, the system libraries and the heavy
packages so that `renv::restore()` is fast and identical on any machine.

| Image | Contents | Used by |
|---|---|---|
| `ghcr.io/valbergregory/jurimetrics-r` | R 4.4.2 (rocker/r-ver) + DuckDB, Arrow, renv, targets, fixest, did/HonestDiD/Synth/plm, tsibble/fable, sf/H3/terra/ncdf4, heatwaveR, sidrar/WDI, ggplot2, modelsummary/gt/texreg, testthat | [Pricing-Non-Pecuniary-Harm](https://github.com/valbergregory/Pricing-Non-Pecuniary-Harm), [STJ-Moral-Damages-Jurimetrics](https://github.com/valbergregory/STJ-Moral-Damages-Jurimetrics), [Geography-of-Judicial-Delay](https://github.com/valbergregory/Geography-of-Judicial-Delay), [marine-fisheries-data-cube](https://github.com/valbergregory/marine-fisheries-data-cube), [fisheries-closures-brazil](https://github.com/valbergregory/fisheries-closures-brazil), [Port-Digitalization-Observatory](https://github.com/valbergregory/Port-Digitalization-Observatory), [Port-Network-Resilience](https://github.com/valbergregory/Port-Network-Resilience), [transmissao-precos-leite-brasil](https://github.com/valbergregory/transmissao-precos-leite-brasil), [tourism-gravity-brazil](https://github.com/valbergregory/tourism-gravity-brazil), [pix-geography-brazil](https://github.com/valbergregory/pix-geography-brazil), [maquinas-agricolas-fabricas-desenvolvimento](https://github.com/valbergregory/maquinas-agricolas-fabricas-desenvolvimento), [maquinas-agricolas-cenarios](https://github.com/valbergregory/maquinas-agricolas-cenarios), [maquinas-agricolas-cambio](https://github.com/valbergregory/maquinas-agricolas-cambio) |
| `ghcr.io/valbergregory/jurimetrics-py` | Python 3.12 (slim) + uv, DuckDB, Polars, Arrow, pandas, scikit-learn, networkx, statsmodels, httpx, pypdf, pytest, ruff | [binding-precedents-brazil](https://github.com/valbergregory/binding-precedents-brazil), [abusive-litigation-jurimetrics](https://github.com/valbergregory/abusive-litigation-jurimetrics), the Python layer of [Port-Network-Resilience](https://github.com/valbergregory/Port-Network-Resilience) |

## Usage

```bash
docker pull ghcr.io/valbergregory/jurimetrics-r:4.4.2

# run a repository's pipeline inside the container
docker run --rm -it -v "$PWD":/work -w /work ghcr.io/valbergregory/jurimetrics-r:4.4.2 \
  Rscript -e 'renv::restore(prompt = FALSE); targets::tar_make()'
```

Tags: `4.4.2` / `3.12` (pinned interpreter version) and `latest`. Images are rebuilt monthly for base-OS
security updates; the R version only changes with an explicit bump in the workflow.

## Build locally

```bash
docker build -t jurimetrics-r:dev --build-arg R_VERSION=4.4.2 jurimetrics-r
docker build -t jurimetrics-py:dev --build-arg PY_VERSION=3.12 jurimetrics-py
```

## Licence

MIT (see [LICENSE](LICENSE)). The image bundles R and CRAN packages under their own licences.
