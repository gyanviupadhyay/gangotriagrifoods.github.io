# Gangotri Agrifoods – GitHub Pages Site

Live site: **https://www.gangotriagrifoods.com**

## GitHub Pages Configuration

| Setting | Value |
|---|---|
| Source | GitHub Actions (`Deploy static content to Pages`) |
| Custom domain | `www.gangotriagrifoods.com` |
| HTTPS enforced | Yes |

The `CNAME` file in this repository root contains `www.gangotriagrifoods.com`. This file must be present so that every Pages deployment preserves the custom domain setting.

## DNS Configuration (GoDaddy)

To keep GitHub Pages health checks passing, the following DNS records must exist at your DNS provider (GoDaddy):

### Required for `www` (primary domain)

| Type | Name | Value | TTL |
|------|------|-------|-----|
| CNAME | `www` | `gyanviupadhyay.github.io.` | 1 Hour |

### Required for apex domain (`gangotriagrifoods.com`)

GitHub's health check also validates the apex domain. To resolve the `NotServedByPagesError` for `gangotriagrifoods.com`, add the following **A records** pointing to GitHub Pages servers:

| Type | Name | Value | TTL |
|------|------|-------|-----|
| A | `@` | `185.199.108.153` | 1 Hour |
| A | `@` | `185.199.109.153` | 1 Hour |
| A | `@` | `185.199.110.153` | 1 Hour |
| A | `@` | `185.199.111.153` | 1 Hour |

> **Note:** If you prefer not to use the apex domain with GitHub Pages directly, you can instead set up **domain forwarding** in GoDaddy:
> Forward `gangotriagrifoods.com` → `https://www.gangotriagrifoods.com` (Permanent 301, forward only).

### Verify DNS propagation

```bash
# Check www CNAME (expected: gyanviupadhyay.github.io.)
dig +short www.gangotriagrifoods.com CNAME

# Check apex A records (expected: 185.199.108-111.153)
dig +short gangotriagrifoods.com A
```

## Troubleshooting Pages Health Check

| Error | Cause | Fix |
|-------|-------|-----|
| `DNS check unsuccessful` / `InvalidDNSError` | CNAME record not found in public DNS | Ensure GoDaddy CNAME `www → gyanviupadhyay.github.io.` is saved and propagated |
| `NotServedByPagesError` for apex | `gangotriagrifoods.com` not pointing to GitHub Pages IPs | Add A records for `@` (see table above) **or** set up GoDaddy domain forwarding |
| `CNAME file mismatch` | `CNAME` file in repo has wrong/old domain | Ensure `CNAME` file contains exactly `www.gangotriagrifoods.com` |
| DNS valid but warning persists | GitHub UI cache lag after domain change | Remove custom domain in Settings → Pages → Save, then re-add and Save again |
