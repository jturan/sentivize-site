# DNS Configuration Guide for AWS Route 53

This guide explains how to configure DNS records in AWS Route 53 to point sentivize.com to GitHub Pages.

## Prerequisites

- Access to AWS Console with Route 53 permissions
- Hosted zone for sentivize.com already created in Route 53

## Step-by-Step Instructions

### 1. Navigate to Route 53

1. Log into AWS Console: https://console.aws.amazon.com/
2. Search for "Route 53" in the services search bar
3. Click on "Hosted zones" in the left sidebar
4. Click on the hosted zone for **sentivize.com**

### 2. Configure Apex Domain (sentivize.com)

**Delete existing A records:**
1. Find any existing A records for the apex domain (sentivize.com)
2. Select them and click "Delete records"

**Create new A records for GitHub Pages:**
1. Click "Create record"
2. Leave "Record name" blank (this is for the apex domain)
3. Record type: **A**
4. Value: Enter all four GitHub Pages IP addresses (one per line):
   ```
   185.199.108.153
   185.199.109.153
   185.199.110.153
   185.199.111.153
   ```
5. TTL: **300** (5 minutes)
6. Routing policy: Simple routing
7. Click "Create records"

### 3. Configure WWW Subdomain (www.sentivize.com)

**Delete existing records for www:**
1. Find any existing A or CNAME records for www.sentivize.com
2. Select them and click "Delete records"

**Create new CNAME record:**
1. Click "Create record"
2. Record name: **www**
3. Record type: **CNAME**
4. Value: **jturan.github.io**
5. TTL: **300** (5 minutes)
6. Routing policy: Simple routing
7. Click "Create records"

### 4. Remove dev Subdomain (dev.sentivize.com)

**Delete all dev subdomain records:**
1. Find any records for dev.sentivize.com (A, CNAME, etc.)
2. Select all dev subdomain records
3. Click "Delete records"
4. Confirm deletion

This will permanently disable dev.sentivize.com.

### 5. Verify DNS Configuration

After making the changes, verify your DNS records look like this:

```
sentivize.com          A      185.199.108.153
sentivize.com          A      185.199.109.153
sentivize.com          A      185.199.110.153
sentivize.com          A      185.199.111.153
www.sentivize.com      CNAME  jturan.github.io
```

## DNS Propagation

- DNS changes typically take 5-60 minutes to propagate
- Maximum propagation time: 48 hours (rare)
- You can check DNS propagation at: https://dnschecker.org/

## Testing Your Site

### Before DNS propagation:
Visit: https://jturan.github.io/sentivize-site

### After DNS propagation:
1. Visit: http://sentivize.com (should redirect to HTTPS)
2. Visit: https://sentivize.com (primary URL)
3. Visit: https://www.sentivize.com (should work or redirect)

## Enable HTTPS Enforcement

After DNS propagates and the certificate is issued (usually 10-20 minutes):

1. Go to GitHub repository: https://github.com/jturan/sentivize-site
2. Click "Settings" > "Pages"
3. Under "Custom domain", verify sentivize.com shows as configured
4. Wait for the "DNS check" to show a green checkmark
5. Check the box for "Enforce HTTPS"

## Troubleshooting

### Site not loading after DNS change:
- Wait 15-30 minutes for DNS propagation
- Clear your browser cache
- Try in incognito/private mode
- Check DNS propagation at dnschecker.org

### HTTPS certificate not available:
- Wait 10-20 minutes after DNS propagates
- GitHub automatically issues SSL certificates for custom domains
- Certificate issuance requires DNS to be properly configured first

### www subdomain not working:
- Verify CNAME record points to: jturan.github.io (not .com)
- Wait for DNS propagation
- Check GitHub Pages settings show custom domain configured

## Post-Migration Checklist

After DNS configuration:

- [ ] sentivize.com loads correctly
- [ ] www.sentivize.com works
- [ ] HTTPS is working (green padlock in browser)
- [ ] All pages load (home, privacy, security, terms)
- [ ] Logo and favicon display correctly
- [ ] Connect button is removed from navigation
- [ ] Footer shows "© Sentivize Health 2026"
- [ ] dev.sentivize.com is disabled (returns DNS error)

## AWS Lightsail Teardown

**ONLY AFTER** confirming the GitHub Pages site is fully working:

1. Navigate to AWS Lightsail console
2. Delete the following resources:
   - dev.sentivize.com instance
   - sentivize.com instance
   - Any static IPs attached to these instances
   - Any associated storage or snapshots
3. Verify no remaining charges in AWS Billing console

## Support

If you encounter any issues:
- GitHub Pages Docs: https://docs.github.com/en/pages
- AWS Route 53 Docs: https://docs.aws.amazon.com/route53/
- Repository: https://github.com/jturan/sentivize-site
