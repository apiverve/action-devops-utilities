# APIVerve DevOps Utilities Action

> Validate JSON schemas, test regex patterns, and decode JWTs

> **Beta Release** - This action is in beta. We'd love your feedback! [Open an issue](https://github.com/apiverve/action-devops-utilities/issues) if you encounter any problems.

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-DevOps Utilities-blue?logo=github)](https://github.com/marketplace/actions/apiverve-devops-utilities)
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
| `jsonschemavalidator` | JSON Schema Validator is a comprehensive tool for validating JSON data against JSON Schema definitions. It provides detailed error reporting with field-level validation results. |
| `jsonschemagenerator` | JSON Schema Generator is a tool for automatically generating JSON schemas from sample JSON data. It creates Draft-07 compatible schemas with type inference and format detection. |
| `regextester` | Regex Tester is a comprehensive tool for testing and validating regular expressions. It supports multiple operations (test, match, search, replace, split) with detailed performance analysis and pattern suggestions. |
| `jwtdecoder` | JWT Decoder decodes JWT tokens to reveal header and payload information without performing signature verification. |
| `cronparser` | Cron Expression Parser is a comprehensive tool for parsing and validating cron expressions. It supports both 5-field and 6-field formats and returns detailed information about each field. |

---

## Quick Start

```yaml
- name: DevOps Utilities
  uses: apiverve/action-devops-utilities@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: jsonschemavalidator
    params: '{&quot;json&quot;: {&quot;name&quot;: &quot;test&quot;}, &quot;schema&quot;: {&quot;type&quot;: &quot;object&quot;, &quot;properties&quot;: {&quot;name&quot;: {&quot;type&quot;: &quot;string&quot;}}}}'
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
    params: '{&quot;json&quot;: {&quot;name&quot;: &quot;test&quot;}, &quot;schema&quot;: {&quot;type&quot;: &quot;object&quot;, &quot;properties&quot;: {&quot;name&quot;: {&quot;type&quot;: &quot;string&quot;}}}}'

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
    params: '{&quot;token&quot;: &quot;eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...&quot;}'

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
          params: '{&quot;json&quot;: {&quot;name&quot;: &quot;test&quot;}, &quot;schema&quot;: {&quot;type&quot;: &quot;object&quot;, &quot;properties&quot;: {&quot;name&quot;: {&quot;type&quot;: &quot;string&quot;}}}}'

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
