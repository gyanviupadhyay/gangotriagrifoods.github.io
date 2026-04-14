# Gangotri Agrifoods Pvt. Ltd. — GitHub Pages Site

Live at: **https://www.gangotriagrifoods.com**

---

## Custom Domain Setup (GoDaddy + GitHub Pages)

This site is served from the `main` branch and deployed via GitHub Actions to GitHub Pages with the custom domain `www.gangotriagrifoods.com`.

### How it works

- The `CNAME` file in this repository contains `www.gangotriagrifoods.com`.
  GitHub Pages reads this file on every deployment to know which custom domain to serve.
  **Do not delete or modify the `CNAME` file** — removing it will break the custom domain.

---

## Required GoDaddy DNS Records

Log in to GoDaddy → **My Products → Domains → gangotriagrifoods.com → DNS**.

### 1. CNAME record for `www` (required — already set)

| Type  | Name | Value                    | TTL    |
|-------|------|--------------------------|--------|
| CNAME | www  | gyanviupadhyay.github.io | 1 Hour |

> **Important:** The Name must be `www` (not the full domain). The Value must be exactly `gyanviupadhyay.github.io` (no trailing dot needed in the GoDaddy UI).

### 2. A records for apex domain `@` (recommended — allows `gangotriagrifoods.com` to redirect to `www`)

Add all four A records so the bare domain (`gangotriagrifoods.com`) also resolves to GitHub Pages:

| Type | Name | Value           | TTL    |
|------|------|-----------------|--------|
| A    | @    | 185.199.108.153 | 1 Hour |
| A    | @    | 185.199.109.153 | 1 Hour |
| A    | @    | 185.199.110.153 | 1 Hour |
| A    | @    | 185.199.111.153 | 1 Hour |

### 3. AAAA records for apex domain `@` (optional, for IPv6)

| Type | Name | Value                  | TTL    |
|------|------|------------------------|--------|
| AAAA | @    | 2606:50c0:8000::153    | 1 Hour |
| AAAA | @    | 2606:50c0:8001::153    | 1 Hour |
| AAAA | @    | 2606:50c0:8002::153    | 1 Hour |
| AAAA | @    | 2606:50c0:8003::153    | 1 Hour |

---

## GitHub Pages Settings

In the repository: **Settings → Pages**

| Setting       | Value                      |
|---------------|----------------------------|
| Source        | GitHub Actions             |
| Custom domain | `www.gangotriagrifoods.com` |
| Enforce HTTPS | Enabled (after DNS check passes) |

---

## Troubleshooting: "DNS check unsuccessful" / InvalidDNSError

### Checklist

1. **CNAME file present?**
   Confirm the file `CNAME` exists in the root of this repository and contains exactly `www.gangotriagrifoods.com`.
   Without this file, GitHub Actions deployments will clear your custom domain setting on every push.

2. **GoDaddy CNAME record correct?**
   - Type: `CNAME`
   - Name: `www` (not the full domain, not `@`)
   - Value: `gyanviupadhyay.github.io`

3. **Only one `www` record in GoDaddy?**
   Check there is no conflicting `A`, `AAAA`, or second `CNAME` record for `www`.
   Delete any duplicates — only the single CNAME above should exist for `www`.

4. **GoDaddy nameservers active?**
   GoDaddy → Domain Settings → **Nameservers** should show "Using GoDaddy nameservers".
   If it shows custom/external nameservers (e.g. Cloudflare), you must add the records there instead.

5. **Wait for DNS propagation.**
   DNS changes can take up to 1 hour (sometimes up to 48 hours globally).
   After changing, click **Check again** in GitHub Pages settings.

6. **Enable HTTPS.**
   Once the DNS check passes, GitHub will automatically provision a TLS certificate.
   When it's ready (can take a few minutes), go to **Settings → Pages** and tick **Enforce HTTPS**.

### Quick DNS verification

Run the following from your terminal (or use an online tool like [dnschecker.org](https://dnschecker.org)):

```
dig +short www.gangotriagrifoods.com CNAME
```

Expected output:
```
gyanviupadhyay.github.io.
```

If this returns nothing or a different value, the CNAME record has not propagated yet or is entered incorrectly in GoDaddy.

---

## References

- [GitHub Docs — Managing a custom domain for your GitHub Pages site](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)
- [GitHub Docs — About custom domains and GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages)
- [GoDaddy Help — Add a CNAME record](https://help-center.dc-aws.godaddy.com/help/add-a-cname-record-19236)
