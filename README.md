# Node.js Library 🔗

![Status](https://img.shields.io/badge/status-completed-green?style=for-the-badge)
![Node](https://img.shields.io/badge/Node.js-ESModules%20%2B%20CLI-green?style=for-the-badge&logo=node.js)

A Node.js CLI tool that extracts all hyperlinks from Markdown files and optionally validates each URL — checking HTTP status codes concurrently with `Promise.all`.

---

## Overview

Point this tool at a `.md` file or an entire directory, and it finds every `[text](url)` link using a regex, then either lists them or hits each URL with a fetch request and reports the HTTP status. It works as a proper CLI tool with a shebang (`#!/usr/bin/env node`) and reads the file path and optional `valida` flag from `process.argv`. All async operations use `async/await`, and concurrent URL validation is handled with `Promise.all` so all requests run in parallel instead of one by one.

---

## Architecture

```
nodejs-library/
├── src/
│   ├── index.js            # Core — reads the file with fs.promises and extracts links via regex
│   ├── http-validacao.js   # Validator — fetches each URL concurrently and returns HTTP status
│   └── cli.js              # CLI entry point — parses process.argv, handles file or directory input
├── arquivos/
│   └── texto.md            # Sample Markdown file with links for testing
└── package.json            # type: "module" — ES Modules; Chalk dependency
```

### Key Concepts Applied

| Concept | How it shows up |
|---|---|
| **ES Modules** | `"type": "module"` in `package.json` — all files use `import`/`export` |
| **CLI tool** | `cli.js` starts with `#!/usr/bin/env node` — can be run as a global command |
| **process.argv** | File path and `valida` flag are read from command line arguments |
| **fs.promises** | `fs.promises.readFile()` and `fs.promises.readdir()` — async file system operations |
| **Regex** | `/\[([^\[\]]*?)\]\((https?:\/\/[^\s?#.][^\s]*)\)/gm` extracts all Markdown link pairs |
| **Promise.all** | All URLs are validated concurrently — one fetch per URL, results merged with the link list |
| **async/await** | Every async operation uses `await` inside `try/catch/finally` |
| **Chalk** | Terminal output is color-coded — yellow for status messages, red for errors, green for identifiers |
| **Directory support** | `cli.js` checks `lstatSync` — processes a single file or all files in a directory |
| **Error handling** | `ENOENT` (file not found) and `ENOTFOUND` (broken URL) are caught and reported clearly |

---

## How to Run

**Prerequisites:** Node.js installed

```bash
git clone https://github.com/victorhubarb/nodejs-library.git
cd nodejs-library
npm install
```

**Extract links from a file:**
```bash
node ./src/cli.js ./arquivos/texto.md
```

**Extract and validate links (check HTTP status):**
```bash
node ./src/cli.js ./arquivos/texto.md valida
```

**Process all files in a directory:**
```bash
node ./src/cli.js ./arquivos valida
```

---

## Technologies

- **Node.js** — runtime
- **ES Modules** — native module system (`import`/`export`)
- **Chalk v5** — color-coded terminal output
- **fs.promises** — async file system API
- **fetch** — built-in HTTP client (Node.js 18+)

---

## Author

**Victor Hugo Barbosa**
[GitHub](https://github.com/victorhubarb) · [LinkedIn](https://www.linkedin.com/in/victorhbarbosa/)
