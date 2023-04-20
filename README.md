# Rust Package GitHub Action

Build, test and publish simple Rust packages

## Use this action: CI

```yaml
name: CI

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
  schedule:
    - cron: 0 5 * * *

jobs:
  ci:
    strategy:
      matrix:
        target:
          - x86_64-apple-darwin
          - x86_64-pc-windows-msvc
          - x86_64-unknown-linux-gnu
        include:
          - target: x86_64-apple-darwin
            host_os: macos-latest
          - target: x86_64-pc-windows-msvc
            host_os: windows-latest
          - target: x86_64-unknown-linux-gnu
            host_os: ubuntu-latest
    runs-on: ${{ matrix.host_os }}
    steps:
      - name: Build and test Rust package
        uses: rcook/rust-package-action@v0.0.1
        with:
          target: ${{ matrix.target }}
```

## Use this action: publishing

```yaml
name: Publish

on:
  push:
    tags:
      - v*.*.*

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - name: Build, test and publish Rust package
        uses: rcook/rust-package-action@v0.0.1
        with:
          target: x86_64-unknown-linux-gnu
          publish: true
        env:
          RUST_PACKAGE_ACTION_CRATES_IO_API_TOKEN: ${{ secrets.CRATES_IO_API_TOKEN }}
```
