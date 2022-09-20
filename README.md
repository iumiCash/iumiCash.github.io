# Vendor API

REST API for vendors.

## Info

Documentation uses beautiful library [mkdocs](https://squidfunk.github.io/mkdocs-material/) under the hood.

To deploy documentation we use [Github Pages](https://docs.github.com/en/enterprise-cloud@latest/pages/quickstart).

As dependency manager we use [Poetry](https://python-poetry.org/).


## Installation

First, install project dependencies:

```shell script
poetry install
```

Activate virtual environment:

```shell script
poetry shell
```

Or run command directly through current env:

```shell script
poetry run mkdocs serve
```

After activating virtual environment, run server:

```shell script
mkdocs serve
```

**Documentation will automatically reload after saving sources.**

## Deploy

Documentation automatically deploys to Github Pages. To deploy documentation, push your changes to `master` branch.

```shell script
git commit -am "some meaningful commit message"
git push -u origin master
```
