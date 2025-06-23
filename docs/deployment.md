# Deployment Guide

## GitHub Pages Setup

### Repository Configuration

The site is automatically deployed via GitHub Pages with the following configuration:

```
Repository: valto/valto.github.io
Branch: main
Domain: valtoai.com
SSL: Enabled (automatic)
CDN: GitHub's global CDN
```

### Custom Domain Configuration

**CNAME File**
```
valtoai.com
```

**DNS Configuration**
```
# DNS Records (configured at domain registrar)
Type: CNAME
Name: www
Value: valto.github.io

Type: A
Name: @
Value: 185.199.108.153
Value: 185.199.109.153
Value: 185.199.110.153
Value: 185.199.111.153
```

### SSL Certificate
- **Status**: Automatically provisioned by GitHub
- **Renewal**: Automatic
- **Force HTTPS**: Enabled in repository settings

## Deployment Process

### Automatic Deployment

GitHub Pages automatically builds and deploys when:
1. Changes are pushed to the `main` branch
2. Build process completes successfully
3. Site is available within 1-10 minutes

### Deployment Workflow
```bash
# 1. Local development
git checkout -b feature/update-content
# Make changes
git add .
git commit -m "Update: workshop content"

# 2. Push to GitHub
git push origin feature/update-content

# 3. Create Pull Request
# Review changes on GitHub

# 4. Merge to main
git checkout main
git pull origin main
git merge feature/update-content
git push origin main

# 5. Automatic deployment
# GitHub Pages builds and deploys automatically
# Site updates live within minutes
```

### Build Status

Monitor deployment status:
- **GitHub Actions tab**: Build logs and status
- **Settings > Pages**: Domain status and SSL info
- **Commits**: Green checkmark indicates successful deployment

## Pre-Deployment Checklist

### Content Review
- [ ] All text content proofread
- [ ] Links tested and functional
- [ ] Images optimized and loading
- [ ] Contact information current
- [ ] Call-to-action buttons working

### Technical Validation
- [ ] HTML validates (W3C validator)
- [ ] CSS syntax correct
- [ ] JavaScript error-free
- [ ] Mobile responsive design
- [ ] Cross-browser compatibility

### Performance Check
- [ ] Images under 1MB each
- [ ] Page load time under 3 seconds
- [ ] Lighthouse score above 90
- [ ] Core Web Vitals passing
- [ ] External resources loading

### SEO Optimization
- [ ] Meta descriptions present
- [ ] Title tags optimized
- [ ] Alt text for all images
- [ ] Semantic HTML structure
- [ ] Open Graph tags configured

## Rollback Procedures

### Quick Rollback
```bash
# If issues discovered after deployment
# Revert to previous commit
git revert HEAD
git push origin main

# Or reset to specific commit
git reset --hard COMMIT_HASH
git push --force origin main
```

### Emergency Rollback
```bash
# Immediate rollback to last known good state
git log --oneline # Find last good commit
git reset --hard GOOD_COMMIT_HASH
git push --force origin main
```

## Monitoring & Maintenance

### Health Checks
```bash
# Daily checks
curl -I https://valtoai.com
# Should return: HTTP/2 200

# Weekly checks
# - SSL certificate validity
# - DNS resolution
# - Page load performance
# - Mobile responsiveness
```

### Uptime Monitoring
- **Primary URL**: https://valtoai.com
- **Workshop Page**: https://valtoai.com/1.html
- **AI Twin Link**: https://hey.speak-to.ai/valto

### Analytics Integration
```html
<!-- Google Analytics (to be added) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

## Troubleshooting

### Common Issues

**Site Not Loading**
1. Check GitHub Pages status
2. Verify DNS settings
3. Clear browser cache
4. Test incognito mode

**Custom Domain Issues**
1. Verify CNAME file contents
2. Check DNS propagation (24-48 hours)
3. Confirm domain registrar settings
4. Test with dig/nslookup

**SSL Certificate Problems**
1. Disable and re-enable custom domain
2. Wait for certificate provisioning
3. Clear browser SSL cache
4. Contact GitHub Support if persistent

**Build Failures**
1. Check GitHub Actions logs
2. Validate HTML/CSS syntax
3. Verify file permissions
4. Check for large files (>100MB)

### Debug Commands
```bash
# DNS lookup
nslookup valtoai.com
dig valtoai.com

# SSL certificate check
openssl s_client -connect valtoai.com:443

# HTTP headers
curl -I https://valtoai.com

# Page load test
curl -w "@curl-format.txt" -o /dev/null -s https://valtoai.com
```

## Performance Optimization

### CDN Benefits
- **Global distribution**: GitHub's worldwide CDN
- **Automatic caching**: Static assets cached globally
- **Fast delivery**: Reduced latency worldwide
- **DDoS protection**: GitHub's infrastructure

### Cache Strategy
```
# Default GitHub Pages caching
HTML: 10 minutes
CSS/JS: 1 year (with versioning)
Images: 1 year
Fonts: 1 year
```

### Performance Monitoring
- **PageSpeed Insights**: Monthly reviews
- **GTmetrix**: Performance tracking
- **Real User Monitoring**: Analytics integration
- **Core Web Vitals**: Google Search Console

## Security Considerations

### HTTPS Enforcement
- All traffic redirected to HTTPS
- HSTS headers enabled
- Secure cookies only

### Content Security
- Static site = minimal attack surface
- No server-side processing
- External resources from trusted CDNs only
- Regular dependency updates

### Access Control
- Repository access limited to collaborators
- Two-factor authentication required
- Deploy keys for automated systems

---

*Next: Review [Analytics & Performance](./analytics.md) for tracking setup*