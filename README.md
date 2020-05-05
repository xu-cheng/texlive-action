# texlive-action

[![GitHub Actions Status](https://github.com/xu-cheng/texlive-action/workflows/Test%20Github%20Action/badge.svg)](https://github.com/xu-cheng/texlive-action/actions)

GitHub Action to run arbitrary commands in a [TeXLive](https://www.tug.org/texlive/) environment.

It comes with two favors, `full` and `small`. The `full` favor action has all TeXLive packages installed. The `small` one however only has basic packages installed to reduce its size. You can use `tlmgr install <package>` to install the missing packages.
The base image is [alpine](https://alpinelinux.org). Use `apk add <package>` to install additional system packages.
See [latex-docker](https://github.com/xu-cheng/latex-docker) for the scripts to build the respective docker images.

This action is suitable to run arbitrary commands in a LaTeX environment. If you simply want to compile LaTeX documents, [latex-action](https://github.com/xu-cheng/latex-action) is better recommended.

## Inputs

* `run`: Arbitrary bash codes to be executed.

## Example

* Run commands in a **full** TeXLive environment.

  ```yaml
  on: [push]
  jobs:
    build_latex:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v2
        - uses: xu-cheng/texlive-action/full@v1
          with:
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
        - uses: actions/checkout@v2
        - uses: xu-cheng/texlive-action/small@v1
          with:
            run: |
              apk add make
              make
  ```

## License

MIT
