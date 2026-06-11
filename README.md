# WSTG Plus

**OWASP Web Security Testing Guide (WSTG)** is the premier cybersecurity testing resource for web application developers and security professionals. However, navigating the guide, finding practical exploit payloads, and using it across different languages can be time-consuming.

**WSTG Plus** solves this by taking the core OWASP WSTG and significantly upgrading it. It maps every single WSTG vulnerability to practical exploit resources (like HackTricks and PortSwigger Web Security Academy) and provides ready-to-use, comprehensive checklists in **19 different languages**.

## Features

- **Multi-Language Support:** Ready-to-use checklists available in 19 languages.
- **Resource Enrichment:** Every WSTG test ID is contextually mapped to:
  - Official OWASP Documentation
  - HackTricks Wiki & Payloads
  - PortSwigger Web Security Academy Labs
  - PayloadsAllTheThings
- **Clean Formats:** Checklists are available in both single-page Markdown format and beautifully formatted PDF.
- **Actionable & Structured Scenarios:** Replaces generic, vague checks with precise, scenario-based multiple-choice questions to ensure consistent penetration testing execution.
  
  *Example:*
  ```markdown
  ### Are identifying strings present in the HTTP response headers?
  - [ ] No — `Server` and `X-Powered-By` headers are **not present** or contain generic values  
  - [ ] Yes — headers reveal server software but **not** specific version numbers  
  - [ ] Yes — headers reveal specific server software and **exact** version numbers  
  ```


## Supported Languages & Downloads

| Language | Code | Full Markdown | Full PDF | Language Folder |
|----------|------|---------------|----------|-----------------|
| Arabic | `ar` | [View Markdown](full-mds/WSTG_ar.md) | [Download PDF](full-pdfs/WSTG_ar.pdf) | [Folder](translations/ar) |
| Bengali | `bn` | [View Markdown](full-mds/WSTG_bn.md) | [Download PDF](full-pdfs/WSTG_bn.pdf) | [Folder](translations/bn) |
| Chinese | `zh` | [View Markdown](full-mds/WSTG_zh.md) | [Download PDF](full-pdfs/WSTG_zh.pdf) | [Folder](translations/zh) |
| Dutch | `nl` | [View Markdown](full-mds/WSTG_nl.md) | [Download PDF](full-pdfs/WSTG_nl.pdf) | [Folder](translations/nl) |
| English | `en` | [View Markdown](full-mds/WSTG_en.md) | [Download PDF](full-pdfs/WSTG_en.pdf) | [Folder](translations/en) |
| French | `fr` | [View Markdown](full-mds/WSTG_fr.md) | [Download PDF](full-pdfs/WSTG_fr.pdf) | [Folder](translations/fr) |
| German | `de` | [View Markdown](full-mds/WSTG_de.md) | [Download PDF](full-pdfs/WSTG_de.pdf) | [Folder](translations/de) |
| Hindi | `hi` | [View Markdown](full-mds/WSTG_hi.md) | [Download PDF](full-pdfs/WSTG_hi.pdf) | [Folder](translations/hi) |
| Indonesian | `id` | [View Markdown](full-mds/WSTG_id.md) | [Download PDF](full-pdfs/WSTG_id.pdf) | [Folder](translations/id) |
| Italian | `it` | [View Markdown](full-mds/WSTG_it.md) | [Download PDF](full-pdfs/WSTG_it.pdf) | [Folder](translations/it) |
| Japanese | `ja` | [View Markdown](full-mds/WSTG_ja.md) | [Download PDF](full-pdfs/WSTG_ja.pdf) | [Folder](translations/ja) |
| Korean | `ko` | [View Markdown](full-mds/WSTG_ko.md) | [Download PDF](full-pdfs/WSTG_ko.pdf) | [Folder](translations/ko) |
| Persian | `fa` | [View Markdown](full-mds/WSTG_fa.md) | [Download PDF](full-pdfs/WSTG_fa.pdf) | [Folder](translations/fa) |
| Polish | `pl` | [View Markdown](full-mds/WSTG_pl.md) | [Download PDF](full-pdfs/WSTG_pl.pdf) | [Folder](translations/pl) |
| Portuguese | `pt` | [View Markdown](full-mds/WSTG_pt.md) | [Download PDF](full-pdfs/WSTG_pt.pdf) | [Folder](translations/pt) |
| Russian | `ru` | [View Markdown](full-mds/WSTG_ru.md) | [Download PDF](full-pdfs/WSTG_ru.pdf) | [Folder](translations/ru) |
| Spanish | `es` | [View Markdown](full-mds/WSTG_es.md) | [Download PDF](full-pdfs/WSTG_es.pdf) | [Folder](translations/es) |
| Turkish | `tr` | [View Markdown](full-mds/WSTG_tr.md) | [Download PDF](full-pdfs/WSTG_tr.pdf) | [Folder](translations/tr) |
| Urdu | `ur` | [View Markdown](full-mds/WSTG_ur.md) | [Download PDF](full-pdfs/WSTG_ur.pdf) | [Folder](translations/ur) |

## Repository Content

This repository focuses purely on delivering the final, compiled checklists for ease of use.
- `full-mds/` : Contains the full single-page Markdown file for each language.
- `full-pdfs/` : Contains the printable, paginated PDF format for each language.
- `translations/` : Contains specific translation files and components for each language.

## Contributing
Contributions, issues, and feature requests are welcome! If you find any translations that need improvement or missing resource links, feel free to submit an issue or pull request.

## License
This project is inspired by and builds upon the [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/).
