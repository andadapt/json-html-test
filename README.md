# pa11y Accessibility Testing on Windows (WCAG 2.2)

A guide to setting up and running [pa11y](https://pa11y.org/) on Windows to perform WCAG 2.2 accessibility tests and export results as both **JSON** and **HTML** files.

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

Install **pa11y** and **pa11y-reporter-html** (for HTML output):

```bash
npm install pa11y pa11y-reporter-html
```

> **Note:** pa11y uses Puppeteer (headless Chrome) under the hood — it will be installed automatically as a dependency. The first install may take a few minutes.

---

## Running the Tests

pa11y does not natively output both JSON and HTML from a single command, so you run two commands — one for each format.

### 5. Run pa11y and output a JSON file

```bash
npx pa11y https://www.thetrainline.co.uk --standard WCAG2AA --runner axe --reporter json > results.json
```

### 6. Run pa11y and output an HTML file

```bash
npx pa11y https://www.thetrainline.co.uk --standard WCAG2AA --runner axe --reporter pa11y-reporter-html > results.html
```

> Both commands test against the **WCAG 2.2 AA** standard (pa11y maps `WCAG2AA` to WCAG 2.2 AA when using the `axe` runner).

---

## Flag Reference

| Flag | Description |
|------|-------------|
| `--standard WCAG2AA` | Tests against WCAG 2.2 Level AA |
| `--runner axe` | Uses the [axe-core](https://github.com/dequelabs/axe-core) engine (supports WCAG 2.2) |
| `--reporter json` | Outputs results as JSON |
| `--reporter pa11y-reporter-html` | Outputs results as a formatted HTML report |
| `> results.json` | Redirects output to a file |

---

## Optional: Run both outputs with a single npm script

Add a script to your `package.json` to run both outputs in one go:

```json
"scripts": {
  "test:a11y": "pa11y https://www.thetrainline.co.uk --standard WCAG2AA --runner axe --reporter json > results.json && pa11y https://www.thetrainline.co.uk --standard WCAG2AA --runner axe --reporter pa11y-reporter-html > results.html"
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
| `pa11y-reporter-html` not found | Run `npm install pa11y-reporter-html` in your project folder |
| Empty `results.html` | Ensure pa11y-reporter-html is installed locally, not globally |

---

## Resources

- [pa11y GitHub](https://github.com/pa11y/pa11y)
- [pa11y-reporter-html GitHub](https://github.com/pa11y/pa11y-reporter-html)
- [WCAG 2.2 Guidelines](https://www.w3.org/TR/WCAG22/)
- [axe-core Rules](https://dequeuniversity.com/rules/axe/)
