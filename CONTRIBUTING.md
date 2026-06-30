# Contributing to HADDOCK3 User Manual

Thank you for your interest in improving the HADDOCK3 user manual!

## How to Contribute

1. **Fork the repository** and create a new branch for your changes
2. **Make your edits** to the markdown files in the `src/` directory
3. **Test locally** by building the book (see below)
4. **Submit a pull request** with a clear description of your changes

## Building the Book Locally

This manual is built with [mdbook](https://rust-lang.github.io/mdBook/). To test your changes:

1. Install mdbook following the [installation guide](https://rust-lang.github.io/mdBook/index.html)
2. Run from the repository root:
   ```bash
   mdbook build .
   ```
3. Open `doc/index.html` in your browser to preview

## Guidelines

- All content goes in the `src/` directory
- Use relative paths for links and images
- Follow the existing markdown style
- Update `src/SUMMARY.md` if adding new pages
- Automated checks will run on your PR:
  - Link validation
  - Book build verification

## Questions?

Feel free to open an issue for discussion before making large changes.
