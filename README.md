# Markdown Link Checker

A Node.js command-line tool that reads Markdown files, pulls out every `[text](url)` link, and can optionally check each URL and report its HTTP status.

## Usage

```bash
git clone https://github.com/victorhubarb/nodejs-library.git
cd nodejs-library
npm install
```

List the links in a file:

```bash
node ./src/cli.js ./arquivos/texto.md
```

Check every link as well, adding `valida`:

```bash
node ./src/cli.js ./arquivos/texto.md valida
```

Point it at a folder to process every file inside it:

```bash
node ./src/cli.js ./arquivos valida
```

Each link is printed as `{ "link text": "https://..." }`, and with `valida` it also gets a `status` such as `200 - OK`, `link não encontrado` (DNS failure) or `ocorreu algum erro`. Output is color-coded with Chalk. The messages are in Portuguese.

## How it works

- **`index.js`** reads the file with `fs.promises.readFile` and extracts the links with a regular expression (`matchAll`).
- **`http-validacao.js`** sends one `fetch` per link inside `Promise.all`, so all requests run in parallel, then merges the status codes back into the link list.
- **`cli.js`** has the `#!/usr/bin/env node` shebang. It reads `process.argv`, checks whether the path is a file or a directory, and prints the results.

It uses ES modules and the built-in `fetch`, so it needs Node.js 18 or newer. Chalk is the only dependency.

## Known gaps

- A file without links makes `valida` fail, because the extractor returns a text message instead of an empty list in that case.
- Directories are read one level deep, and every entry is treated as a file.
- Only `http` and `https` links match the regex, and links with a `?` or `#` right after the first character are skipped.
- The `cli:valida` npm script passes `--valida`, but the CLI expects plain `valida`.
- No tests.

---

Victor Hugo Barbosa · [GitHub](https://github.com/victorhubarb) · [LinkedIn](https://www.linkedin.com/in/victorhbarbosa/)
