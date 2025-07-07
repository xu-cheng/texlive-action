# texlive-action

[![GitHub Actions Status](https://github.com/xu-cheng/texlive-action/workflows/test/badge.svg)](https://github.com/xu-cheng/texlive-action/actions)

A GitHub Action that provides a complete [TeXLive](https://www.tug.org/texlive/) environment for running LaTeX-related commands and scripts.

> [!TIP]
> This action is designed for running custom commands, scripts, and build processes in a LaTeX environment. If you only need to compile LaTeX documents, consider using [latex-action](https://github.com/xu-cheng/latex-action) instead.

## Features

- 🐳 **Containerized**: Runs in a [Docker container](https://github.com/xu-cheng/latex-docker/) with a complete [TeXLive](https://www.tug.org/texlive/) installation.
- 📅 **Version flexibility**: Supports both the latest and historic versions of TeXLive (2020-2025).
- 🐧 **Multi-platform**: Support both [Alpine](https://alpinelinux.org) and [Debian](https://www.debian.org/)-based Docker images.
- 📦 **Two installation schemes**: Choose between full or small TeXLive installation.
  - **Full scheme**: Complete TeXLive installation with all packages
  - **Small scheme**: Minimal TeXLive installation with basic packages only (smaller image size)
- ⚙️ **Fully customizable**: Run arbitrary commands to build your custom LaTeX documents.

All Docker images are built using scripts from [latex-docker](https://github.com/xu-cheng/latex-docker).

## Inputs

Configure the action by providing these inputs in the `with` section:

### Required Inputs

- **`run`** (required)
  Bash commands to execute. Commands are executed with `bash -eo pipefail -c {input}`.

  ```yaml
  - uses: xu-cheng/texlive-action@v3
    with:
      run: |
        latexmk -pdf -file-line-error -halt-on-error -interaction=nonstopmode main.tex
  ```

### Optional Inputs

- **`texlive_version`**
  TeXLive version to use (2020-2025 or 'latest'). Defaults to 'latest'. Cannot be used with `docker_image`.

  ```yaml
  - uses: xu-cheng/texlive-action@v3
    with:
      texlive_version: 2024
      run: ...
  ```

- **`os`**
  Base operating system for the Docker image (`alpine` or `debian`). Defaults to `alpine`.

  ```yaml
  - uses: xu-cheng/texlive-action@v3
    with:
      os: debian
      run: ...
  ```

- **`scheme`**
  TeXLive installation scheme (`full` or `small`). Defaults to `full`. Cannot be used with `docker_image`. When using `small`, `os` must be `alpine` and `texlive_version` must be `latest`.

  ```yaml
  - uses: xu-cheng/texlive-action@v3
    with:
      texlive_version: latest
      os: alpine
      scheme: small
      run: ...
  ```

- **`docker_image`**
  Custom Docker image to use (overrides `texlive_version`, `os`, and `scheme`). We recommend using [latex-docker images](https://github.com/xu-cheng/latex-docker/).

  ```yaml
  - uses: xu-cheng/texlive-action@v3
    with:
      docker_image: ghcr.io/xu-cheng/texlive-alpine@sha256:<hash>
      run: ...
  ```

## Examples

### Basic LaTeX compilation

```yaml
on: [push]
jobs:
  build_latex:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: xu-cheng/texlive-action@v3
        with:
          run: |
            latexmk -pdf -file-line-error -halt-on-error -interaction=nonstopmode main.tex
```

### Using a small TeXLive installation

```yaml
on: [push]
jobs:
  build_latex:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: xu-cheng/texlive-action@v3
        with:
          scheme: small
          run: |
            # Install additional packages if needed
            tlmgr install collection-fontsextra
            pdflatex main.tex
```

### Using Debian-based environment

```yaml
on: [push]
jobs:
  build_latex:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: xu-cheng/texlive-action@v3
        with:
          os: debian
          run: |
            # Install system dependencies
            export DEBIAN_FRONTEND=noninteractive
            apt-get update -y
            apt-get install -y make
            make
```

### Using a specific TeXLive version

```yaml
on: [push]
jobs:
  build_latex:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: xu-cheng/texlive-action@v3
        with:
          texlive_version: 2024
          run: |
            latexmk -pdf -file-line-error -halt-on-error -interaction=nonstopmode main.tex
```

### Using a custom Docker image

```yaml
on: [push]
jobs:
  build_latex:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: xu-cheng/texlive-action@v3
        with:
          docker_image: ghcr.io/xu-cheng/texlive-alpine@sha256:<hash>
          run: |
            latexmk -pdf -file-line-error -halt-on-error -interaction=nonstopmode main.tex
```

## License

MIT
