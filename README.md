# Haddock3 user manual

This repository holds the MarkDown files and assets used to compile the Haddock3 user manual !

Thanks to github actions, this book is compiled on pull request and directly published online, accessible at: [https://www.bonvinlab.org/haddock3-user-manual](https://www.bonvinlab.org/haddock3-user-manual)

## Contribute to the book

The book source code is located in `src/`.
From here, consider only relative paths for things to be functional.

## Structure of the book

Three major files/directory are used here:

- `book.toml`: File containing data such as name of the book, list of authors and where to find the source code. This file must be named `book.toml` for `mdbook` to work with default arguments.
- `src/`: Hold all the pages / subdirectories of the book content.
- `src/SUMMARY.md`: The table of content of the book. It is used to create the TOC and define the order in which files will be added.
- `theme/`: Holds extra files such as icons, spcific css files and other allowing to fine tune the appearance of the final book.

# Compile the book

This manual is ment to be compiled by `mdbook`.

To generate it, mdbook must be installed (see [how to install mdbook](https://rust-lang.github.io/mdBook/index.html)).

Then run the following command, from the root directory:

```bash
mdbook build .
```

Once built, the book index can be found at `doc/index.html`


