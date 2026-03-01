mdBook is a command line tool to create books with Markdown. It is ideal for creating product or API documentation, tutorials, course materials or anything that requires a clean, easily navigable and customizable presentation.

```sh
# install
cargo install mdbook

# tools
## init --theme --title --ignore --force
mdbook init
mdbook init path/to/book

## The build command is used to render your book --open --dest-dir
mdbook build
mdbook build path/to/book

## The watch command is useful when you want your book to be rendered on every file change.
mdbook watch path/to/book

## The serve command is used to preview a book by serving it via HTTP at localhost:3000 by default:
mdbook serve
mdbook serve path/to/book

## mdBook supports a test command that will run all available tests in a book
mdbook test path/to/book

## The clean command is used to delete the generated book and any other build artifacts.
mdbook clean
mdbook clean path/to/book
mdbook clean --dest-dir=path/to/book
```

## mdbook-mermaid

`mdbook-mermaid` 不会自动更新 `mermaid.min.js` 文件，如果需要更新 Mermaid 版本，可以重新运行 `mdbook-mermaid install` 或手动替换文件。

```sh
cargo install mdbook-mermaid

mdbook-mermaid install /path/to/your/book
```

```toml
[preprocessor.mermaid]
command = "mdbook-mermaid"

[output.html]
additional-js = ["mermaid.min.js", "mermaid-init.js"]
```

## mdBook Documentation

- [mdBook Documentation](https://rust-lang.github.io/mdBook/index.html)
  - [mdBook-specific features](https://rust-lang.github.io/mdBook/format/mdbook.html)
