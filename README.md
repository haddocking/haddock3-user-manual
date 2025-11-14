# HADDOCK3 User Manual

[![build](https://github.com/haddocking/haddock3-user-manual/actions/workflows/build.yml/badge.svg)](https://github.com/haddocking/haddock3-user-manual/actions/workflows/build.yml)
[![deploy book](https://github.com/haddocking/haddock3-user-manual/actions/workflows/mdbook.yml/badge.svg)](https://github.com/haddocking/haddock3-user-manual/actions/workflows/mdbook.yml)
[![links](https://github.com/haddocking/haddock3-user-manual/actions/workflows/link-checker.yml/badge.svg)](https://github.com/haddocking/haddock3-user-manual/actions/workflows/link-checker.yml)
[![Codacy Badge](https://app.codacy.com/project/badge/Grade/ec845bd05cbc4e499df24e70ebe6c6d9)](https://app.codacy.com/gh/haddocking/haddock3-user-manual/dashboard?utm_source=gh&utm_medium=referral&utm_content=&utm_campaign=Badge_grade)

This repository contains the source files for the HADDOCK3 user manual, built with [mdbook](https://rust-lang.github.io/mdBook/).

**📖 Read the manual online:** [https://www.bonvinlab.org/haddock3-user-manual](https://www.bonvinlab.org/haddock3-user-manual)

## Building Locally

To build and preview the manual locally:

1. Install mdbook following the [installation guide](https://rust-lang.github.io/mdBook/index.html)
2. Run from the repository root:
   ```bash
   mdbook build .
   ```
3. Open `doc/index.html` in your browser

## Repository Structure

- `book.toml` - Book configuration (title, authors, output settings)
- `src/` - Markdown content and assets
- `src/SUMMARY.md` - Table of contents
- `theme/` - Custom styling and assets

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on how to contribute to this manual.

## Automated Workflows

GitHub Actions automatically:
- Build and validate the book on pull requests
- Deploy to GitHub Pages on merge to main
- Check for broken links


