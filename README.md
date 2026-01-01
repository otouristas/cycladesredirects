# Cyclades Redirects Site

Simple HTML redirect site to redirect `greececyclades.com` to `discovercyclades.gr`.

## Purpose

This is a minimal redirect site that can be deployed separately to handle 301 redirects from the old domain to the new domain for Google Search Console change of address.

## Files

- `index.html` - HTML redirect page with meta refresh and JavaScript
- `.htaccess` - Apache server redirect rules (301 permanent)
- `_redirects` - Netlify redirect rules
- `netlify.toml` - Netlify configuration with domain redirects

## Deployment Options

### Option 1: Netlify (Recommended)

1. Connect this repository to Netlify
2. Set publish directory to root (`.`)
3. Add `greececyclades.com` as a custom domain
4. Deploy - redirects will work automatically

### Option 2: Apache Server

1. Upload all files to the root directory of `greececyclades.com`
2. The `.htaccess` file will handle all redirects automatically
3. Make sure `mod_rewrite` is enabled

### Option 3: Any Static Hosting

1. Upload `index.html` to the root directory
2. The HTML file has meta refresh and JavaScript redirects
3. Works on any hosting provider

## How It Works

- **HTML**: Meta refresh and JavaScript redirect (works everywhere)
- **Apache**: Server-level 301 redirects via `.htaccess` (requires Apache)
- **Netlify**: Edge-level 301 redirects via `_redirects` and `netlify.toml` (requires Netlify)

All methods preserve the full path, query string, and hash when redirecting.

## Testing

After deployment, test:
- `http://greececyclades.com/` → Should redirect to `https://discovercyclades.gr/`
- `https://greececyclades.com/guides/milos` → Should redirect to `https://discovercyclades.gr/guides/milos`

## Google Search Console

This redirect site handles all the URLs Google needs to test:
- ✅ `http://greececyclades.com/`
- ✅ `https://greececyclades.com/`
- ✅ `https://greececyclades.com/ferry-tickets/tracking`
- ✅ `https://greececyclades.com/guides/milos`
- ✅ `https://www.greececyclades.com/en/`
- ✅ `https://greececyclades.com/guides/kimolos`

