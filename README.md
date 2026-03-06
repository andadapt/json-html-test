# Accessibility Testing on Windows (WCAG 2.2)

A guide to setting up and running [pa11y](https://pa11y.org/) and [axe CLI](https://github.com/dequelabs/axe-cli) on Windows to perform WCAG 2.2 accessibility tests and export results as **JSON** and **HTML** files.

---

## Prerequisites

### 1. Install Node.js and npm

pa11y requires Node.js (which includes npm).

1. Go to [https://nodejs.org/](https://nodejs.org/) and download the **LTS** version.
2. Run the installer and follow the prompts — make sure **"Add to PATH"** is checked during installation.
3. Verify the installation by opening **Command Prompt** or **PowerShell** and running:

```bash
node --version
npm --version
```

Both commands should print a version number (e.g. `v20.x.x` and `10.x.x`).

---

## Project Setup

### 2. Create a project folder

Open **Command Prompt** or **PowerShell** and run:

```bash
mkdir pa11y-tests
cd pa11y-tests
```

### 3. Initialise an npm project

```bash
npm init -y
```

This creates a `package.json` file in your folder.

### 4. Install required packages

Install **pa11y**:

```bash
npm install pa11y
```

> **Note:** pa11y uses Puppeteer (headless Chrome) under the hood — it will be installed automatically as a dependency. The first install may take a few minutes.
>
> pa11y 9+ includes a built-in `html` reporter — no extra package needed.

---

## Running the Tests

pa11y does not natively output both JSON and HTML from a single command, so you run two commands — one for each format.

### 5. Run pa11y and output a JSON file

```bash
npx pa11y https://www.thetrainline.co.uk --standard WCAG2AA --runner axe --reporter json > results.json
```

### 6. Run pa11y and output an HTML file

```bash
npx pa11y https://www.thetrainline.co.uk --standard WCAG2AA --runner axe --reporter html > results.html
```

> Both commands test against the **WCAG 2.2 AA** standard (pa11y maps `WCAG2AA` to WCAG 2.2 AA when using the `axe` runner).

---

## Flag Reference

| Flag | Description |
|------|-------------|
| `--standard WCAG2AA` | Tests against WCAG 2.2 Level AA |
| `--runner axe` | Uses the [axe-core](https://github.com/dequelabs/axe-core) engine (supports WCAG 2.2) |
| `--reporter json` | Outputs results as JSON |
| `--reporter html` | Outputs results as a formatted HTML report (built-in since pa11y 9) |
| `> results.json` | Redirects output to a file |

---

## Optional: Run both outputs with a single npm script

Add a script to your `package.json` to run both outputs in one go:

```json
"scripts": {
  "test:a11y": "pa11y https://www.thetrainline.co.uk --standard WCAG2AA --runner axe --reporter json > results.json && pa11y https://www.thetrainline.co.uk --standard WCAG2AA --runner axe --reporter html > results.html"
}
```

Then run:

```bash
npm run test:a11y
```

---

## Viewing the Results

- **`results.json`** — Open in any text editor or JSON viewer.
- **`results.html`** — Open directly in a browser for a readable, formatted accessibility report.

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| `npx` not recognised | Reinstall Node.js and ensure it is added to PATH |
| Puppeteer download fails | Check your network/proxy settings; try `npm install --ignore-scripts` then install Chromium separately |
| HTML reporter not found | Ensure you are using pa11y 9+ (`npx pa11y --version`). The `html` reporter is built-in — no extra package needed. |

---

---

## axe CLI

[axe CLI](https://github.com/dequelabs/axe-cli) is Deque's official command line tool built directly on axe-core. It supports WCAG 2.2 out of the box and can save results as a JSON file.

### Install axe CLI globally

```bash
npm install -g axe-cli
```

Verify the installation:

```bash
axe --version
```

> **Note:** axe CLI also uses Puppeteer (headless Chrome). On first run it may download Chromium automatically — this can take a minute or two.

### Run axe and output a JSON file

```bash
axe https://www.thetrainline.co.uk --tags wcag2aa,wcag21aa,wcag22aa --save axe-results.json
```

This creates **`axe-results.json`** in your current folder containing all violations, incomplete checks, passes, and inapplicable rules.

### axe CLI flag reference

| Flag | Description |
|------|-------------|
| `--tags wcag2aa,wcag21aa,wcag22aa` | Runs all rules covering WCAG 2.0 AA, 2.1 AA, and 2.2 AA |
| `--save <filename>` | Saves the JSON results to the specified file |
| `--reporter raw` | Alternative: prints the full raw JSON to stdout instead of saving |
| `--browser chrome-headless` | Explicitly use headless Chrome (default) |

### Example: print JSON to stdout instead of a file

```bash
axe https://www.thetrainline.co.uk --tags wcag2aa,wcag21aa,wcag22aa --reporter raw > axe-results.json
```

### axe CLI troubleshooting

| Issue | Fix |
|-------|-----|
| `axe` not recognised after install | Close and reopen your terminal so PATH updates take effect |
| Chromium download fails | Set `PUPPETEER_SKIP_DOWNLOAD=1` and point to an existing Chrome install via `--browser` |
| Empty or partial JSON output | Ensure the page fully loads — try adding `--timeout 60000` (milliseconds) |
| Permission error on global install | Run the command in a terminal opened as Administrator |

---

## Resources

- [pa11y GitHub](https://github.com/pa11y/pa11y)
- [pa11y-reporter-html GitHub](https://github.com/pa11y/pa11y-reporter-html)
- [axe CLI GitHub](https://github.com/dequelabs/axe-cli)
- [axe-core Rules](https://dequeuniversity.com/rules/axe/)
- [WCAG 2.2 Guidelines](https://www.w3.org/TR/WCAG22/)
