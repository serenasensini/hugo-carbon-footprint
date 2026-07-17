# 🌱 hugo-carbon-footprint

A **Hugo Module** + Node.js script to measure and display the carbon footprint of any Hugo site, regardless of the theme in use.

Data is calculated through the public [Website Carbon](https://api.websitecarbon.com/) API and saved to `data/carbon.json`, which Hugo reads natively as `site.Data.carbon`.

---

## Requirements

| Tool | Minimum version |
|------|-----------------|
| Hugo | 0.112.0+ |
| Node.js | 18+ |
| Go | 1.21+ *(for Hugo Modules)* |

---

## Installation

### 1. Declare the module in your `config.toml`

```toml
[module]
  [[module.imports]]
    path = "github.com/serenasensini/hugo-carbon-footprint"
```

Or in `config.yaml`:

```yaml
module:
  imports:
    - path: github.com/serenasensini/hugo-carbon-footprint
```

### 2. Download the module

```bash
hugo mod get github.com/serenasensini/hugo-carbon-footprint
```

### 3. Get the Node.js script

Copy `scripts/carbon.js` into the root of your Hugo project, or run it via `npx`:

```bash
npx hugo-carbon
```

---

## Usage

### Update CO2 data before the build

```bash
# With environment variables (recommended)
CARBON_SITE_URL=https://www.yoursite.com node scripts/carbon.js

# On Windows PowerShell
$env:CARBON_SITE_URL="https://www.yoursite.com"; node scripts/carbon.js
```

Or add a script in `package.json`:

```json
{
  "scripts": {
    "carbon": "node scripts/carbon.js"
  }
}
```

then run:

```bash
npm run carbon
```

### Add the badge to your template

In any Hugo partial or layout (footer, base layout, etc.):

```html
{{ partial "carbon-badge.html" . }}
```

With custom options:

```html
{{ partial "carbon-badge.html" (dict
    "showRating" true
    "showLink"   true
) }}
```

---

## Environment variables

| Variable | Default | Description |
|----------|---------|-------------|
| `CARBON_SITE_URL` | `http://localhost:1313` | Homepage URL used to measure HTML byte size |
| `CARBON_GREEN` | `0` | `1` if your hosting uses renewable energy, `0` otherwise |
| `CARBON_BYTES` | *(unset)* | Fixed byte value sent to the API (skips page fetch) |
| `CARBON_OUTPUT` | `data/carbon.json` | Output path for the JSON file (relative to cwd) |

> **Note:** `CARBON_SITE_URL` should point to your production site for accurate measurements. During local development, you can use `http://localhost:1313` with Hugo server running.

---

## `data/carbon.json` structure

```json
{
  "bytes": 50928,
  "green": false,
  "gco2e": 0.0053,
  "rating": "A+",
  "cleanerThan": 0.99,
  "statistics": { ... },
  "_meta": {
    "siteUrl": "https://www.yoursite.com",
    "green": false,
    "bytes": 50928,
    "measuredAt": "2026-07-16T13:41:51.281Z"
  }
}
```

| Field | Description |
|-------|-------------|
| `gco2e` | Grams of CO2 equivalent per visit |
| `rating` | Digital Carbon Rating (A+, A, B, C, D, E, F) |
| `cleanerThan` | Percentage (0-1) of sites with higher emissions |

---

## CSS customization

The partial adds the `.carbon-badge` class to the paragraph. Minimal style example:

```css
.carbon-badge {
  font-size: 0.85rem;
  color: #4a7c59;
  text-align: center;
  margin-top: 1rem;
}

.carbon-badge a {
  color: inherit;
  text-decoration: underline;
}
```

---

## CI/CD integration (GitHub Actions)

```yaml
- name: Measure CO2 footprint
  env:
    CARBON_SITE_URL: https://www.yoursite.com
    CARBON_GREEN: "0"
  run: node scripts/carbon.js

- name: Build Hugo
  run: hugo --minify
```

---

## How it works

```
npm run carbon
      │
      ▼
GET https://www.yoursite.com          ← measures HTML byte size
      │
      ▼
GET https://api.websitecarbon.com/data?bytes={n}&green={0/1}
      │
      ▼
data/carbon.json  ←── written with gco2e, rating, cleanerThan
      │
      ▼
hugo build  →  {{ partial "carbon-badge.html" . }}  →  footer 🌱
```

> The `/data` API is the only public Website Carbon endpoint since July 14, 2025 and does not require authentication.

---

## License

[MIT](LICENSE) © Serena Sensini

---

## Community and project health

- Contributions: see [CONTRIBUTING.md](CONTRIBUTING.md)
- Code of Conduct: see [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)
- Security policy: see [SECURITY.md](SECURITY.md)
- CI: GitHub Actions workflow in [`.github/workflows/ci.yml`](.github/workflows/ci.yml)
- Dependency updates: [Dependabot](.github/dependabot.yml)
