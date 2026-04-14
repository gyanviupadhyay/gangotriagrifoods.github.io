# Gangotri Agrifoods Pvt. Ltd. — GitHub Pages Site

This repository hosts the static website for **Gangotri Agrifoods Pvt. Ltd.** published via GitHub Pages at the custom domain:

> **https://github-pages-challenge-gyanviupadhyay.gangotriagrifoods.com**

---

## Custom Domain & HTTPS Setup

### What is already configured in this repository

- A `CNAME` file in the repository root contains the custom domain  
  (`github-pages-challenge-gyanviupadhyay.gangotriagrifoods.com`).  
  GitHub Pages reads this file to know which domain to serve and to provision a TLS certificate.

---

## DNS Configuration on GoDaddy (step-by-step)

The custom domain is a **subdomain** of `gangotriagrifoods.com`, so you need a **CNAME record** in GoDaddy DNS — not A/AAAA records.

### Step 1 — Sign in to GoDaddy

1. Go to <https://dcc.godaddy.com> and sign in.
2. Click **My Products** → find `gangotriagrifoods.com` → click **DNS**.

### Step 2 — Add (or correct) the CNAME record

Click **Add New Record** and fill in:

| Field          | Value                            |
|----------------|----------------------------------|
| **Type**       | `CNAME`                          |
| **Name / Host**| `github-pages-challenge-gyanviupadhyay` |
| **Value / Points to** | `gyanviupadhyay.github.io` |
| **TTL**        | `1 Hour` (or `3600`)             |

Click **Save**.

> **Why `gyanviupadhyay.github.io`?**  
> This repository is a *user* GitHub Pages site owned by the GitHub account `gyanviupadhyay`, so the correct CNAME target is `<username>.github.io`.

### Step 3 — Remove conflicting records (important)

In GoDaddy DNS, look for any existing record with the **Name**  
`github-pages-challenge-gyanviupadhyay` (any type: A, CNAME, AAAA, Redirect).  
**Delete all of them** before saving the new CNAME — only one record should exist for that host.

### Step 4 — Wait for DNS propagation

DNS changes typically take **a few minutes to 1 hour** on GoDaddy, but can take up to **48 hours** globally. You can check propagation with:

```
nslookup github-pages-challenge-gyanviupadhyay.gangotriagrifoods.com
```

It should resolve to the same address as `gyanviupadhyay.github.io`.

---

## GitHub Pages Settings (repository side)

1. Go to the repository → **Settings** → **Pages**.
2. Under **Build and deployment**, confirm Source is set to **GitHub Actions** (or the correct branch/folder).
3. Under **Custom domain**, enter:
   ```
   github-pages-challenge-gyanviupadhyay.gangotriagrifoods.com
   ```
   and click **Save**.
4. GitHub will run a DNS check. Once it passes, the **Enforce HTTPS** checkbox becomes available — enable it.

> **Why was the DNS check failing?**  
> The most common causes are:
> - The `CNAME` file was missing from the repository (now added).
> - The GoDaddy CNAME record pointed to the wrong target (e.g. `username.github.io` typed incorrectly), or an old A/CNAME record was conflicting.
> - The custom domain field in GitHub Pages settings was blank or mistyped.

---

## (Optional) Verify the domain in your GitHub account

Domain verification prevents other GitHub users from hijacking your custom domain.

1. Go to **GitHub → Settings (account)** → **Pages** → **Add a domain**.
2. Enter `gangotriagrifoods.com` (or the full subdomain).
3. GitHub provides a **TXT record** — add it in GoDaddy DNS:

| Field          | Value (from GitHub)         |
|----------------|-----------------------------|
| **Type**       | `TXT`                       |
| **Name / Host**| as shown by GitHub          |
| **Value**      | as shown by GitHub          |
| **TTL**        | `1 Hour`                    |

4. Click **Verify** in GitHub.

---

## Summary of required DNS records

| Type  | Name (host)                                      | Value / Points to          |
|-------|--------------------------------------------------|----------------------------|
| CNAME | `github-pages-challenge-gyanviupadhyay`          | `gyanviupadhyay.github.io` |

No A or AAAA records are needed for a subdomain.
