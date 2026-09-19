# APIVerve DevOps Utilities Action

> Validate JSON schemas, test regex patterns, and decode JWTs

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-DevOps_Utilities-blue?logo=github)](https://github.com/apiverve/action-devops-utilities)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**[Browse All APIs](https://apiverve.com/marketplace?utm_source=github&utm_medium=action&utm_campaign=devops-utilities)** | **[Get Free API Key](https://dashboard.apiverve.com/signup?utm_source=github&utm_medium=action&utm_campaign=devops-utilities)** | **[Documentation](https://docs.apiverve.com?utm_source=github&utm_medium=action&utm_campaign=devops-utilities)**

---

## What does this action do?

This action provides access to APIVerve's DevOps Utilities APIs directly in your GitHub workflows:

- Validate JSON against schemas
- Generate JSON schemas from data
- Test regex patterns
- Decode and inspect JWTs

### Available APIs

| API | Description |
|-----|-------------|
| `jsonschemavalidator` | JSON Schema Validator tests any JSON object against a provided JSON Schema definition to verify data structures and types. It reports pass-fail status and total error count, while paid plans add specific error messages. |
| `jsonschemagenerator` | JSON Schema Generator inspects sample JSON payloads and generates Draft-07 JSON Schema definitions with inferred types and property formats. It identifies formats like emails and dates, applies custom titles, and lists all properties as required. |
| `regextester` | Regex Tester tests regular expressions against input strings across operations like match, search, replace, and split. It reports compilation validity, syntax error details, execution timing, and structural pattern analysis. |
| `jwtdecoder` | JWT Decoder decodes JWT tokens to reveal header and payload information without performing signature verification. |
| `cronparser` | Cron Expression Parser decodes 5-field and 6-field cron expressions into human-readable schedules. Pass any cron string to verify syntax, read plain-language timing explanations, and get the next five execution timestamps. |

---

## Quick Start

```yaml
- name: DevOps Utilities
  uses: apiverve/action-devops-utilities@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: jsonschemavalidator
    params: '{"json": {"name": "test"}, "schema": {"type": "object", "properties": {"name": {"type": "string"}}}}'
```

---

## Setup

### 1. Get Your API Key

Sign up for a free account at [dashboard.apiverve.com/signup](https://dashboard.apiverve.com/signup?utm_source=github&utm_medium=action&utm_campaign=devops-utilities) and create an API key.

### 2. Add Secret to Repository

Go to your repository **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

- Name: `APIVERVE_KEY`
- Value: Your API key from the dashboard

### 3. Use in Workflow

```yaml
- name: DevOps Utilities
  uses: apiverve/action-devops-utilities@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: jsonschemavalidator
    params: '{"your": "parameters"}'
```

---

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `api_key` | Your APIVerve API key (or set `APIVERVE_API_KEY` env var) | Yes* | - |
| `api` | API to use: `jsonschemavalidator`, `jsonschemagenerator`, `regextester`, `jwtdecoder`, `cronparser` | No | `jsonschemavalidator` |
| `params` | JSON parameters for the API | No | `{}` |
| `output_file` | Path to save binary output (images, PDFs) | No | - |
| `format` | Response format: `json`, `yaml`, or `xml` | No | `json` |
| `fail_on_error` | Fail workflow if API returns error | No | `true` |
*\*API key is required but can be provided via input OR `APIVERVE_API_KEY` / `APIVERVE_KEY` environment variable.*

## Outputs

| Output | Description |
|--------|-------------|
| `result` | Full API response as JSON |
| `data` | The `data` field from response as JSON |
| `status` | API status (`ok` or `error`) |
| `file` | Path to downloaded file (if `output_file` was used) |
---

## Examples

### JSON Schema Validation

Validate JSON data against a schema

```yaml
- name: JSON Schema Validation
  id: devops-utilities-0
  uses: apiverve/action-devops-utilities@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: jsonschemavalidator
    params: '{"json": {"name": "test"}, "schema": {"type": "object", "properties": {"name": {"type": "string"}}}}'

- name: Use result
  run: echo "Result: ${{ steps.devops-utilities-0.outputs.data }}"
```

### JWT Decode

Decode a JWT token

```yaml
- name: JWT Decode
  id: devops-utilities-1
  uses: apiverve/action-devops-utilities@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: jwtdecoder
    params: '{"token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."}'

- name: Use result
  run: echo "Result: ${{ steps.devops-utilities-1.outputs.data }}"
```


---

## Full Workflow Example

```yaml
name: DevOps Utilities Workflow

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  devops-utilities:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run DevOps Utilities
        id: result
        uses: apiverve/action-devops-utilities@v1
        with:
          api_key: ${{ secrets.APIVERVE_KEY }}
          api: jsonschemavalidator
          params: '{"json": {"name": "test"}, "schema": {"type": "object", "properties": {"name": {"type": "string"}}}}'

      - name: Show result
        run: |
          echo "Status: ${{ steps.result.outputs.status }}"
          echo "Data: ${{ steps.result.outputs.data }}"
```

---

## Related Actions

Looking for more APIVerve actions?

- [apiverve/action](https://github.com/apiverve/action) - Generic action for all 350+ APIs
- [apiverve/action-release-assets](https://github.com/apiverve/action-release-assets) - Generate QR codes, barcodes, and badges for your GitHub releases
- [apiverve/action-visual-testing](https://github.com/apiverve/action-visual-testing) - Capture screenshots and generate PDFs for visual regression testing and documentation
- [apiverve/action-dns-monitor](https://github.com/apiverve/action-dns-monitor) - Verify DNS configuration, check propagation, and validate DNSSEC after deployments

**[Browse all APIVerve Actions →](https://github.com/marketplace?query=apiverve)**

---

## Pricing

- **Free tier** - Get started with generous free limits
- **Pro plans** - Higher rate limits and priority support for production use

Check out [pricing details](https://apiverve.com/pricing?utm_source=github&utm_medium=action&utm_campaign=devops-utilities).

---

## Resources

- **API Documentation**: [docs.apiverve.com](https://docs.apiverve.com?utm_source=github&utm_medium=action&utm_campaign=devops-utilities)
- **API Marketplace**: [apiverve.com/marketplace](https://apiverve.com/marketplace?utm_source=github&utm_medium=action&utm_campaign=devops-utilities)
- **Issues & Support**: [GitHub Issues](https://github.com/apiverve/action-devops-utilities/issues)
- **Email**: support@apiverve.com

---

## License

MIT - see [LICENSE](LICENSE)

---

Built by [APIVerve](https://apiverve.com?utm_source=github&utm_medium=action&utm_campaign=devops-utilities) - 350+ APIs for developers
