# Contributing

Thanks for your interest in contributing to **hugo-carbon-footprint**.

## Development setup

1. Fork and clone the repository.
2. Use Node.js 18+.
3. Run the script locally:

```bash
npm run carbon
```

On PowerShell:

```powershell
$env:CARBON_SITE_URL="https://www.yoursite.com"; npm run carbon
```

## Pull request guidelines

1. Keep changes focused and minimal.
2. Update documentation when behavior or usage changes.
3. Keep all project text in English.
4. Do not commit secrets or environment-specific files.
5. Open a pull request with a clear description of:
   - what changed
   - why it changed
   - any migration or compatibility notes

## Reporting issues

Use the issue templates and include:

- Steps to reproduce
- Expected behavior
- Actual behavior
- Environment details (OS, Node version, Hugo version)
