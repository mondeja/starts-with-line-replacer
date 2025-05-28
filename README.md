# starts-with-line-replacer

[![Tests](https://img.shields.io/github/actions/workflow/status/mondeja/starts-with-line-replacer/tests.yml?label=tests&logo=github)](https://github.com/mondeja/starts-with-line-replacer/actions)

GitHub Action that replaces lines in a file which start with a specific string.

> [!WARNING]  
> This action is not intended to be used with untrusted input.

## Usage

For example, given the next file

```toml
# Cargo.toml
[package]
name = "my-package"
version = "0.1.0"
```

the version can be updated with

```yaml
jobs:
  update-version:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Update version
        uses: mondeja/starts-with-line-replacer@main
        with:
          file: Cargo.toml
          starts-with: 'version ='
          replace-by: 'version = "0.2.0"'
```

- If the input file is not found, the action will fail.
- If the line that starts with the specified string is not found, the action will fail.

## How it works

It uses a `sed -i 's/^${{ inputs.starts-with }}.*/${{ inputs.replace-by }}/'` command in a composite action to replace the line that starts with `starts` with `replace`.
