# DNS & GitHub Pages Setup Guide

This document explains how to configure DNS (GoDaddy) and GitHub Pages for
**www.gangotriagrifoods.com** so that the site loads over HTTPS.

---

## 1. Repository CNAME File

The `CNAME` file in the root of this repository must contain exactly one line:

```
www.gangotriagrifoods.com
```

This tells GitHub Pages which custom domain to serve. **Do not edit or delete
this file.** If you change the custom domain in GitHub Pages settings, GitHub
will update this file automatically; manually changing the GitHub Pages
"Custom domain" field without a matching CNAME file (or vice-versa) will cause
the DNS check to fail.

---

## 2. Required DNS Records (GoDaddy)

Log in to GoDaddy → your domain **gangotriagrifoods.com** → **DNS** → **Records**.

### 2a. CNAME record for `www` (required)

| Type  | Name | Value                    | TTL     |
|-------|------|--------------------------|---------|
| CNAME | www  | gyanviupadhyay.github.io | Default |

This record makes `www.gangotriagrifoods.com` resolve to GitHub Pages.

### 2b. A records for the apex domain (recommended)

Adding these A records lets `gangotriagrifoods.com` (without `www`) also reach
GitHub Pages, and GoDaddy can forward the apex to `www`.

| Type | Name | Value           | TTL     |
|------|------|-----------------|---------|
| A    | @    | 185.199.108.153 | Default |
| A    | @    | 185.199.109.153 | Default |
| A    | @    | 185.199.110.153 | Default |
| A    | @    | 185.199.111.153 | Default |

### 2c. AAAA records for the apex domain (optional but recommended)

| Type | Name | Value                  | TTL     |
|------|------|------------------------|---------|
| AAAA | @    | 2606:50c0:8000::153    | Default |
| AAAA | @    | 2606:50c0:8001::153    | Default |
| AAAA | @    | 2606:50c0:8002::153    | Default |
| AAAA | @    | 2606:50c0:8003::153    | Default |

### 2d. (Optional) Forward apex to www

In GoDaddy, set up **Domain Forwarding** so that:

- `gangotriagrifoods.com` → `https://www.gangotriagrifoods.com` (301 redirect)

This ensures visitors who type the bare domain are automatically redirected to
the `www` version.

---

## 3. GitHub Pages Settings

1. Open the repository on GitHub.
2. Go to **Settings → Pages** (under "Code and automation").
3. Under **Custom domain**, enter: `www.gangotriagrifoods.com`
4. Click **Save**. GitHub will start a DNS check.
5. Once the DNS check passes, tick **Enforce HTTPS**.

---

## 4. Troubleshooting: "DNS Check Unsuccessful"

### 4a. Wrong or missing CNAME record

Verify the CNAME record in GoDaddy DNS:

- **Type:** CNAME
- **Name:** `www` (not the full domain, just `www`)
- **Value:** `gyanviupadhyay.github.io` (GitHub Pages hostname for this account)

A common mistake is entering the full domain (`www.gangotriagrifoods.com`) in
the **Name** field — GoDaddy appends the domain automatically, so you should
enter only `www`.

### 4b. Conflicting DNS records

Check GoDaddy DNS for any existing record whose **Name** is `www`:

- If you see an **A record** for `www`, delete it.
- If you see a **CNAME** for `www` pointing somewhere other than
  `gyanviupadhyay.github.io`, update or delete it.
- There must be **only one CNAME** for `www` pointing to
  `gyanviupadhyay.github.io`.

### 4c. Nameservers are not GoDaddy's

If your domain's nameservers point to Cloudflare or another provider, changes
you make in GoDaddy's DNS editor have **no effect**. To check:

1. In GoDaddy, go to **Domain Settings → Nameservers**.
2. If they are not GoDaddy nameservers (e.g., `ns1.domaincontrol.com`), you
   must manage DNS at the provider shown there.

To use GoDaddy DNS, switch the nameservers back to GoDaddy defaults.

### 4d. DNS propagation delay

DNS changes can take up to **48 hours** to propagate worldwide, though most
changes are visible within 1–2 hours. You can verify propagation with:

```
nslookup www.gangotriagrifoods.com 8.8.8.8
```

The result should show a CNAME pointing to `gyanviupadhyay.github.io` (and
eventually to GitHub's IP addresses).

You can also use https://www.whatsmydns.net/ to check propagation globally.

### 4e. HTTPS / certificate not yet issued

Even after the DNS check passes, the HTTPS certificate can take a few minutes
to be issued by GitHub. If **Enforce HTTPS** is still greyed out:

1. Wait 10–30 minutes.
2. Refresh the **Settings → Pages** page.
3. Once the certificate is ready the checkbox will become active.

---

## 5. Summary Checklist

- [ ] `CNAME` file in repository root contains `www.gangotriagrifoods.com`
- [ ] GitHub Pages **Custom domain** set to `www.gangotriagrifoods.com`
- [ ] GoDaddy: CNAME record — Name `www`, Value `gyanviupadhyay.github.io`
- [ ] GoDaddy: No conflicting A or CNAME records for `www`
- [ ] GoDaddy: Nameservers are GoDaddy's (or DNS is managed at the correct provider)
- [ ] DNS check in GitHub Pages shows **successful**
- [ ] **Enforce HTTPS** checkbox enabled
