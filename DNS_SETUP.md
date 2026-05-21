DNS setup for legal.gymbody.com

This document describes the recommended DNS records and verification steps to point `legal.gymbody.com` to the GitHub Pages site for the `gymbody-policies` repository.

Recommended configuration (subdomain):

1) Add a CNAME record at your DNS provider

- Type: CNAME
- Name / Host: legal
- Value / Target: gymbody.github.io
- TTL: 3600 (or automatic)

Example (Cloud DNS / Route53 / other panels):
- Host: legal
- Value: gymbody.github.io

2) Verify the DNS record

- Use `dig` or `nslookup` to confirm:

  dig CNAME legal.gymbody.com +short
  # Expected: gymbody.github.io.

  nslookup -type=CNAME legal.gymbody.com

3) Configure GitHub Pages

- In the `gymbody-policies` repository Settings → Pages:
  - Custom domain: `legal.gymbody.com` (enter and save)
  - GitHub will detect the `CNAME` file present in the repo root and use it.
  - After verification, enable **Enforce HTTPS** once GitHub issues the TLS certificate.

4) Troubleshooting / notes

- If you use Cloudflare, do NOT enable proxy (orange cloud). Set the CNAME as DNS-only (gray cloud) so GitHub can provision TLS.
- If you need the apex domain (gymbody.com) instead of a subdomain, use A records pointing to GitHub Pages IPs:
  - 185.199.108.153
  - 185.199.109.153
  - 185.199.110.153
  - 185.199.111.153
  (and add a CNAME for `www` to `gymbody.github.io` if desired)

- DNS propagation may take up to 24-48 hours, but usually completes in minutes to a few hours.

5) Quick verification after Pages publishes

- Visit: https://legal.gymbody.com (or https://github.com/gymbody/gymbody-policies/blob/main/terms.html until DNS is propagated)
- Check TLS: `curl -I https://legal.gymbody.com` should return HTTP 200 and valid cert information.

6) Additional options

- If you prefer org-root pages (no repo path), create a repository named `gymbody.github.io` and place the files at its root.
- This repo already includes a `CNAME` file; after DNS is configured and Pages is enabled, GitHub will complete the setup.

If you'd like, I can also add a small verification script or a CI check that uses `dig` to confirm the CNAME is published. Let me know.