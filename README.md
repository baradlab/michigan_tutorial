# U Michigan CryoET Segmentation & Modeling Tutorial

Material for a ~half-day tutorial on segmentation and modeling for cryo-ET, which Ben presents at the University of Michigan CryoET Workshop.

**Live site:** <https://baradlab.com/michigan_tutorial>

This is a [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) site, versioned with [mike](https://github.com/jimporter/mike). Two editions are published:

| Edition | Tools covered | Source | URL |
| --- | --- | --- | --- |
| **2026** (latest) | MemBrain-seg, Mosaic, Surface Morphometrics, Surforama | `main` branch | <https://baradlab.com/michigan_tutorial/> (and `/2026/`) |
| **2024** (archived) | MemBrain-Seg, Dragonfly, Surface Morphometrics | `archive/2024` branch | <https://baradlab.com/michigan_tutorial/2024/> |

Use the version selector in the site header to switch between editions.

## How publishing works

* Pushing to `main` triggers `.github/workflows/build-and-deploy-docs.yml`, which runs `mike deploy --push --update-aliases 2026 latest` — i.e. it (re)publishes the **2026** edition as `latest`. It never touches the archived 2024 version.
* The **2024** edition is a frozen one-time deploy from the `archive/2024` branch (see below). Its source is preserved on that branch.

## Local development

```bash
python -m venv .venv-docs
.venv-docs/bin/pip install mkdocs-material mike
.venv-docs/bin/mkdocs serve     # live preview of the current branch's edition
```

## Deploying (maintainers)

The current edition deploys automatically from `main` via CI. The one-time/manual deploys are:

```bash
# One-time: publish the archived 2024 edition
git checkout archive/2024
mike deploy --push 2024
git checkout main

# One-time: make `latest` the default landing page (root redirect)
mike set-default --push latest
```

## License

Licensed under the BSD license. Please feel free to reuse liberally!
