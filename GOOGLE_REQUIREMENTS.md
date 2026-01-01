# ✅ Google Change of Address Requirements Checklist

Based on Google's official documentation, here are the exact requirements:

## Required Pre-Work (Step 1)

### ✅ 1. Implement 301 redirect from old homepage to new homepage

**Must Have:**
- `http://greececyclades.com/` → `https://discovercyclades.gr/` (301)
- `https://greececyclades.com/` → `https://discovercyclades.gr/` (301)

**Current Status:** ✅ Configured in `netlify.toml` and `_redirects`

### ✅ 2. Also implement 301 redirects for canonical pages

**Must Have:** 301 redirects for important pages like:
- `/ferry-tickets/tracking`
- `/guides/milos`
- `/guides/kimolos`
- `/en/`

**Current Status:** ✅ All paths redirect via wildcard (`/*`)

## Step 2: Change of Address Tool Requirements

### ✅ 1. Must be owner of both properties
- ✅ Old site: `sc-domain:greececyclades.com`
- ✅ New site: `https://discovercyclades.gr/`
- ✅ Same Google account

### ✅ 2. Domain-level properties only
- ✅ Using `sc-domain:greececyclades.com` (domain property)
- ✅ Not using path-level property

### ✅ 3. All protocols move together
- ✅ If moving `http://greececyclades.com`, it also moves `https://greececyclades.com`
- ✅ Both protocols must redirect properly

## 🔍 Current Issue: "Couldn't fetch the page"

### Why Google Can't Fetch

Google says it can't fetch `http://greececyclades.com/`. This means:

1. **HTTP requests might not be working**
   - Check if `http://greececyclades.com/` actually redirects (test with curl)
   - DNS might not be pointing to Netlify
   - SSL certificate might not be provisioned

2. **Redirect chain might be wrong**
   - Googlebot tries HTTP first
   - HTTP must redirect to HTTPS (same domain)
   - Then HTTPS redirects to new domain
   - Too many redirects = failure

3. **DNS/SSL issues**
   - Domain might not be properly added to Netlify
   - SSL certificate might not be active for old domain
   - DNS might not be propagated

## 🔧 Solution Steps

### Step 1: Verify Domain is on Netlify

1. Go to Netlify Dashboard
2. Site → Domain settings
3. Verify `greececyclades.com` is listed
4. Check status:
   - ✅ Green = Verified
   - ❌ Red = Not configured

### Step 2: Test HTTP Redirect

```bash
# Test if HTTP redirects work
curl -I http://greececyclades.com/

# Should return:
# HTTP/1.1 301 Moved Permanently
# Location: https://discovercyclades.gr/
```

**If this fails:**
- DNS not pointing to Netlify
- Or domain not added to Netlify site

### Step 3: Test HTTPS Redirect

```bash
# Test if HTTPS redirects work
curl -I https://greececyclades.com/

# Should return:
# HTTP/1.1 301 Moved Permanently
# Location: https://discovercyclades.gr/
```

**If this fails:**
- SSL certificate not provisioned
- Or domain not properly configured

### Step 4: Verify Redirect Chain

The current configuration:
1. `http://greececyclades.com/` → `https://greececyclades.com/` (301)
2. `https://greececyclades.com/` → `https://discovercyclades.gr/` (301)

**Total: 2 redirects** (acceptable, but could be 1)

**Better approach** (direct redirect):
- `http://greececyclades.com/` → `https://discovercyclades.gr/` (301) directly
- `https://greececyclades.com/` → `https://discovercyclades.gr/` (301) directly

### Step 5: Use Google's URL Inspection Tool

1. Go to Search Console
2. Select property: `sc-domain:greececyclades.com`
3. Use **URL Inspection Tool**
4. Enter: `http://greececyclades.com/`
5. Click **Test Live URL**
6. See what Googlebot sees

### Step 6: Request Indexing

After fixing:
1. URL Inspection Tool
2. Test each URL:
   - `http://greececyclades.com/`
   - `https://greececyclades.com/`
   - `https://greececyclades.com/guides/milos`
3. Click **Request Indexing**
4. Wait a few hours
5. Try Change of Address again

## ⚠️ Common Problems

### Problem: HTTP not redirecting

**Solution:**
- Make sure DNS points to Netlify
- Make sure domain is added to Netlify
- Check `.htaccess` if using Apache

### Problem: SSL certificate missing

**Solution:**
- Netlify should auto-provision SSL
- Go to Domain settings → SSL
- Manually trigger certificate provisioning if needed

### Problem: Too many redirects

**Solution:**
- Use direct redirect (HTTP → New Domain, not HTTP → HTTPS → New Domain)
- Update `netlify.toml` to redirect directly

### Problem: DNS not propagated

**Solution:**
- Check DNS with: `nslookup greececyclades.com`
- Should show Netlify IPs
- Wait 24-48 hours if recently changed

## 📋 Final Checklist

Before trying Change of Address:

- [ ] Domain `greececyclades.com` added to Netlify
- [ ] DNS points to Netlify (check with nslookup)
- [ ] SSL certificate active (check with browser or SSL Labs)
- [ ] `http://greececyclades.com/` returns 301 redirect (test with curl)
- [ ] `https://greececyclades.com/` returns 301 redirect (test with curl)
- [ ] All redirects point to `https://discovercyclades.gr/`
- [ ] robots.txt allows Googlebot (not blocking)
- [ ] Site responds quickly (< 2 seconds)
- [ ] Tested with Google's URL Inspection Tool
- [ ] Requested indexing for test URLs

## 🎯 Next Actions

1. **Test HTTP redirect** with curl
2. **Check DNS** points to Netlify
3. **Verify domain** is in Netlify dashboard
4. **Use URL Inspection Tool** to see what Googlebot sees
5. **Request indexing** for failed URLs
6. **Wait 24-48 hours** after fixes
7. **Try Change of Address** again

---

**Most Important:** HTTP must work! Googlebot tries HTTP first.

