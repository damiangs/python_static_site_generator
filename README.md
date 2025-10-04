# Python Static Site Generator

A small, dependency-free static site generator written in Python. It takes markdown-like content files from `content/`, renders them into HTML using a simple `template.html`, and copies static assets from `static/` into the output `docs/` folder.

This project is intentionally minimal and easy to read — great as a learning project, a starter for a tiny site, or as a basis for experimentation.

## Features

- Single-file entrypoint: `src/main.py` drives the whole build.
- Copies static files (CSS, images, etc.) from `static/` to the public folder.
- Generates HTML pages from content files under `content/` using `template.html`.
- No external dependencies — works with the Python standard library.

## Quick start

Requirements: Python 3.10+ (the repo was developed and tested on Python 3.12).

To build the site locally, run:

```bash
python3 src/main.py
```

You can optionally pass a basepath as the first argument (useful if deploying to a subpath):

```bash
python3 src/main.py /my/subpath/
```

The script will:

1. Remove the existing `docs/` directory (if present).
2. Copy `static/` into `docs/`.
3. Generate HTML files into `docs/` from `content/` using `template.html`.

Open `docs/index.html` in your browser to view the generated site.

## Project layout

Top-level files and directories:

- `src/` — Python source files for the generator.
  - `main.py` — CLI entrypoint.
  - `gencontent.py` — logic to read content files and render pages.
  - `copystatic.py` — utility to copy the `static/` tree to the output.
  - `inline_markdown.py`, `markdown_blocks.py`, `textnode.py`, `htmlnode.py` — lightweight markdown/HTML helpers and AST-like nodes used by the renderer.
  - `test_*.py` — small unit tests.
- `content/` — source content; each folder represents a URL path. Files are simple markdown-like documents.
- `static/` — CSS, images and other assets that are copied verbatim to `docs/`.
- `template.html` — the HTML template used when generating pages. It expects `{{ Title }}` and `{{ Content }}` placeholders.
- `docs/` — build output (generated). This folder is created by the build and can be served or deployed.

## Example

The repository includes an example blog under `content/blog/` and matching generated HTML under `docs/blog/` so you can inspect the input/output quickly.

## Development

Run tests with the included `test_*.py` files. If you have pytest installed, run:

```bash
pytest -q
```

If you prefer the simple included test runner, there's also a `test.sh` script at the project root which runs a basic check.

When editing templates, `template.html` is used as-is; it looks for the tokens `{{ Title }}` and `{{ Content }}` (case sensitive).

Tips for extending the generator:

- Add more sophisticated markdown support by enhancing `inline_markdown.py` and `markdown_blocks.py`.
- Add front-matter parsing (YAML/TOML) to the content pipeline to support per-page metadata like titles, dates, or templates.
- Add permalink and slug generation in `gencontent.py` to control URLs more precisely.

## License

This project is provided as-is for learning and small personal projects. No license is specified in the repository — add one if you plan to reuse or redistribute the code.
