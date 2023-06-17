# About

The open source knowledge repository for common smart contract bugs.

The goal is to make a comprehensive checklist of common smart contract errors that can be used in many different contexts. This list intends to make it clearer what is an incorrect and correct pattern looks like to make it easier for anyone to better understand these common issues.

Is something missing? Review the [contributing guidelines](./CONTRIBUTING.md) and open a PR!

## How to run locally

This project is [built with MkDocs](https://squidfunk.github.io/mkdocs-material/). To run the docs locally, install the Python `mkdocs-material` package with `pip install mkdocs-material` or `pip3 install mkdocs-material`. After the package is installed, navigate to the top level directory of this project and run `mkdocs serve`.

## Seriously, why another security checklist?!

This checklist of security issues in smart contracts aims to differentiate itself in a few ways:

1. Seek community contributions rather than attempting a solo effort
2. Give an explanation about what differentiates a correct implementation from an incorrect implementation, making it easier for anyone (security novices included) to avoid common errors
3. Link to additional information related to each vulnerability
4. Provide a nicer layout rendering than a markdown file