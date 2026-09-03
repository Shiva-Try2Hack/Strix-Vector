# Strix Vector

A non-intrusive, passive Python CLI security auditing tool designed to inspect authentication endpoints, evaluate defensive posture (WAF signatures, rate-limiting response headers, anti-automation controls, CSRF protection, and security headers), and calculate defensive risk posture.

**Author**: [Shiva-Try2Hack](https://github.com/Shiva-Try2Hack)  
**GitHub**: [https://github.com/Shiva-Try2Hack/StrixVector](https://github.com/Shiva-Try2Hack/StrixVector)

---

## Key Features

1. **Target Inspection & Passive DOM Parsing**:
   - Parses target HTML to locate authentication forms (`<form>`, password inputs).
   - Detects anti-CSRF tokens (`csrf_token`, `xsrf`, `authenticity_token`, dynamic nonces).
   - Identifies client-side anti-automation mechanisms (reCAPTCHA, Cloudflare Turnstile, hCaptcha, GeeTest).

2. **Defense & Header Fingerprinting**:
   - Inspects HTTP response headers for missing security headers (`HSTS`, `CSP`, `X-Content-Type-Options`, `X-Frame-Options`, `Referrer-Policy`).
   - Detects rate-limiting headers (`Retry-After`, `RateLimit-*`, `X-RateLimit-*`).
   - Identifies WAF / CDN signatures (Cloudflare, AWS WAF, Akamai, Imperva Incapsula, Fastly, F5 BIG-IP).

3. **Non-Intrusive Probing**:
   - Sends **no more than 3 intentionally benign test requests** to observe sequential access behavior (exclusively checking for HTTP 429 Too Many Requests or throttling headers).
   - Strictly prohibits wordlists, dictionary attacks, or password stuffing.

4. **Reporting & Risk Assessment**:
   - Generates ANSI colored terminal summaries and structured JSON outputs (`--json` or `--output`).
   - Calculates a **Defensive Posture Score (0-100)** and assigns a Defensive Risk Rating (LOW, MEDIUM, HIGH RISK).

---

## Installation

```bash
pip install -r requirements.txt
```

---

## Usage

### Interactive Mode (Simply run without arguments)

```bash
python main.py
```
**Prompt**: `Enter target website URL (e.g., https://example.com/login):`

---

### Command Flags Mode

```bash
python main.py --target <TARGET_URL> [options]
```

### Options

| Flag | Description |
|---|---|
| `--target`, `-t` | **Required.** Target URL or endpoint (e.g., `https://example.com/login`). |
| `--json`, `-j` | Output raw JSON summary to stdout. |
| `--output`, `-o` | Save structured JSON report to specified file path. |
| `--no-probe` | Skip the 3-probe safe rate limiting check. |
| `--insecure`, `-k` | Disable SSL certificate verification. |
| `--timeout` | Set custom HTTP request timeout in seconds (default: 10.0s). |

---

## Example Usage

### Terminal Summary Output

```bash
python main.py --target https://example.com/login
```

### Export JSON Report

```bash
python main.py --target https://example.com/login --json --output report.json
```

---

## Running Unit Tests

```bash
pytest tests/
```
