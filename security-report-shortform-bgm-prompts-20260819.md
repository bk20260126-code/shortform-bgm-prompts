# Security Report — shortform-bgm-prompts

**Date:** 2026-08-19 · **Scan type:** full (pre-commit) · **Severity threshold:** high

## Executive summary

**Risk score: 0/100 (No Risk)**

Score breakdown: `min(100, round(log10((0×100) + (0×30) + (0×10) + (0×3) + 1) × 25)) = min(100, round(log10(1) × 25)) = min(100, round(0)) = 0`

| Severity | Count |
|---|---|
| Critical | 0 |
| High | 0 |
| Medium | 0 |
| Low | 0 |
| **Total** | **0** |

## Scan coverage

| Scanner | Category | Status |
|---|---|---|
| gitleaks | Secrets | Skipped — not installed on host, user-approved |
| semgrep | SAST | Skipped — not installed on host, user-approved |
| grype | Dependency/container CVEs | Skipped — not installed on host, user-approved |
| checkov | IaC | Skipped — not installed on host, user-approved |
| hadolint | Dockerfile | Skipped — not installed on host, user-approved |
| package-leakage (built-in) | Publish-artifact leakage across npm/Docker/Maven/Gradle/Python/Go/Ruby/NuGet/Rust | Passed — 0 findings (repo has none of the applicable manifest/build-output files) |
| Manual secret-pattern grep | API keys, tokens, private-key headers, AWS/GitHub token prefixes | Passed — 0 matches across all tracked files |
| Manual brand/identity leak grep | Internal workspace/brand references that shouldn't ship publicly | Passed — 0 matches |

**Why five scanners were skipped:** this repository contains only Markdown documentation, one JSON manifest (`plugin.json`), a `LICENSE`, and a `.gitignore` — no application code, no `Dockerfile`, no infrastructure-as-code, and no dependency manifest. gitleaks/semgrep/grype/checkov/hadolint each target a category of artifact this repo does not contain. The user reviewed this and approved proceeding without installing them, given the manual checks above already covered the realistic risk (accidental secrets or leaked internal brand references in documentation text).

## STRIDE threat analysis

| Category | Count |
|---|---|
| Spoofing | 0 |
| Tampering | 0 |
| Repudiation | 0 |
| Information Disclosure | 0 |
| Denial of Service | 0 |
| Elevation of Privilege | 0 |

No findings in any scanned category — no per-category detail to report.

## Findings

None.

## Quick wins / this sprint / next quarter

Not applicable — no findings.
