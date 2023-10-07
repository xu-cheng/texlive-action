# texlive-action

[![GitHub Actions Status](https://github.com/xu-cheng/texlive-action/workflows/Test%20Github%20Action/badge.svg)](https://github.com/xu-cheng/texlive-action/actions)

GitHub Action to run arbitrary commands in a [TeXLive](https://www.tug.org/texlive/) environment.

It comes with two flavors, `full` and `small`. The `full` action has all TeXLive packages installed. The `small` one however only has basic packages installed to reduce its size. You can use `tlmgr install <package>` to install the missing packages.
The base image is [alpine](https://alpinelinux.org). Use `apk add <package>` to install additional system packages.
See [latex-docker](https://github.com/xu-cheng/latex-docker) for the scripts to build the respective docker images.

This action is suitable to run arbitrary commands in a LaTeX environment. If you simply want to compile LaTeX documents, [latex-action](https://github.com/xu-cheng/latex-action) is better recommended.

## Inputs

* `run`: Arbitrary bash codes to be executed. It will be executed in the form of `bash -eo pipefail -c {input}`.
* `scheme`: The scheme of TeXLive to be used, either full or small. By default, full TeXLive is used.
* `texlive_version`: The version of TeXLive to be used. Supported inputs include 2020, 2021, 2022, 2023, and latest. By default the latest TeXLive is used. This input cannot co-exist with `docker_image` input.
* `docker_image`: Custom which docker image to be used. Only [latex-docker images](https://github.com/xu-cheng/latex-docker/pkgs/container/texlive-full) are supported.

## Example

* Run commands in a **full** TeXLive environment.

  ```yaml
  on: [push]
  jobs:
    build_latex:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - uses: xu-cheng/texlive-action@v2
          with:
            scheme: full
            run: |
              apk add make
              make
  ```

* Run commands in a **small** TeXLive environment.

  ```yaml
  on: [push]
  jobs:
    build_latex:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - uses: xu-cheng/texlive-action@v2
          with:
            scheme: small
            run: |
              apk add make
              make
  ```

* Run commands in a 2022 TeXLive environment.

  ```yaml
  on: [push]
  jobs:
    build_latex:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - uses: xu-cheng/texlive-action@v2
          with:
            texlive_version: 2022
            run: |
              apk add make
              make
  ```

* Run commands using custom docker image.

  ```yaml
  on: [push]
  jobs:
    build_latex:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - uses: xu-cheng/texlive-action@v2
          with:
            docker_image: ghcr.io/xu-cheng/texlive-full:20230801
            run: |
              apk add make
              make
  ```

## License

MIT
