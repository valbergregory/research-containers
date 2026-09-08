# research-containers

Reproducible container images for my research repositories (jurimetrics, law and
economics, applied economics, marine fisheries). Each article repository keeps its own
`renv.lock`; these images only pin the R version, the system libraries and the heavy
packages so that `renv::restore()` is fast and identical on any machine.

| Image | Contents | Used by |
|---|---|---|
| `ghcr.io/valbergregory/jurimetrics-r` | R 4.4.2 (rocker/r-ver) + DuckDB, Arrow, renv, targets, fixest, sf/H3, ggplot2, testthat | [Pricing-Non-Pecuniary-Harm](https://github.com/valbergregory/Pricing-Non-Pecuniary-Harm), [STJ-Moral-Damages-Jurimetrics](https://github.com/valbergregory/STJ-Moral-Damages-Jurimetrics), [Geography-of-Judicial-Delay](https://github.com/valbergregory/Geography-of-Judicial-Delay), [binding-precedents-brazil](https://github.com/valbergregory/binding-precedents-brazil), [marine-fisheries-data-cube](https://github.com/valbergregory/marine-fisheries-data-cube), [fisheries-closures-brazil](https://github.com/valbergregory/fisheries-closures-brazil), [Port-Digitalization-Observatory](https://github.com/valbergregory/Port-Digitalization-Observatory), [transmissao-precos-leite-brasil](https://github.com/valbergregory/transmissao-precos-leite-brasil) |

## Usage

```bash
docker pull ghcr.io/valbergregory/jurimetrics-r:4.4.2

# run a repository's pipeline inside the container
docker run --rm -it -v "$PWD":/work -w /work ghcr.io/valbergregory/jurimetrics-r:4.4.2 \
  Rscript -e 'renv::restore(prompt = FALSE); targets::tar_make()'
```

Tags: `4.4.2` (pinned R version) and `latest`. Images are rebuilt monthly for base-OS
security updates; the R version only changes with an explicit bump in the workflow.

## Build locally

```bash
docker build -t jurimetrics-r:dev --build-arg R_VERSION=4.4.2 jurimetrics-r
```

## Licence

MIT (see [LICENSE](LICENSE)). The image bundles R and CRAN packages under their own licences.
